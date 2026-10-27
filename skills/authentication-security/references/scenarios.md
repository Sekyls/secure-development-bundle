# Observable security scenarios

These are original local verification fixtures, not production middleware or a complete assessment of any application. The execution results are in the bundle's validation report. Each attack demonstrates the difference between an insecure boundary and the selected control; secure counterparts retain intended behavior.

## Scenario expectations

| Scenario | Insecure outcome | Secure counterpart | Guide |
| --- | --- | --- | --- |
| Tenant isolation | A query returns another tenant's row | Query binds the authorized tenant and object | [Authorization](authorization.md) |
| SQL injection | Quoted input changes query logic | Bound value remains a literal | [Data](api-data.md) |
| SSRF redirect | An allowed origin redirects to a forbidden test destination | Redirect is rejected and the second server is never hit | [Server requests](server-fetch-http.md) |
| Session revocation | Deleting a client cookie leaves a stolen token usable | Server invalidation rejects replay; expiry also rejects | [Sessions](sessions.md) |
| JWT acceptance | Decode-only logic accepts the wrong audience or unsigned token | Signature, purpose/profile, audience, issuer, and lifetime are checked | [Tokens](oauth-jwt.md) |
| Concurrent spending | Two workers spend 160 from a balance of 100 | Atomic database predicate and transaction permit one spend | [State](business-state.md) |
| Browser XSS | Raw HTML insertion executes an event handler | Text insertion creates no element and preserves literal content | [Browser](browser.md) |
| TypeScript and mass assignment | A cast leaves an attacker-supplied role present | Runtime validation and explicit field selection retain the original role | [Data](api-data.md) |

## Executable Node fixtures

The following block was run using Node 26.3.0. It uses built-in modules, local-only test servers, temporary SQLite data, and worker threads. No external service is contacted. It illustrates an exact-origin feature with redirects forbidden; it is not a general public-URL SSRF defense. The JWT HMAC verifier is a narrow test fixture; production code should use a maintained verifier and the real token profile. The database race is exercised across separate connections, but does not prove PostgreSQL or a distributed service's isolation behavior.

```javascript
import assert from 'node:assert/strict';
import test from 'node:test';
import { DatabaseSync } from 'node:sqlite';
import { createHmac, randomBytes, createHash, timingSafeEqual } from 'node:crypto';
import { createServer } from 'node:http';
import { once } from 'node:events';
import { Worker } from 'node:worker_threads';
import { mkdtemp, rm } from 'node:fs/promises';
import { tmpdir } from 'node:os';
import { join } from 'node:path';

// Local behavioral fixtures, not reusable production security middleware.
test('tenant query: cross-tenant access rejected; intended access preserved', () => {
  const db = new DatabaseSync(':memory:');
  try {
    db.exec("CREATE TABLE documents(id TEXT, tenant TEXT, body TEXT); INSERT INTO documents VALUES('doc-b','tenant-b','private')");
    const unsafe = id => db.prepare('SELECT body FROM documents WHERE id=?').get(id);
    const safe = (id, trustedTenant) => db.prepare('SELECT body FROM documents WHERE id=? AND tenant=?').get(id, trustedTenant);
    assert.equal(unsafe('doc-b').body, 'private');
    assert.equal(safe('doc-b', 'tenant-a'), undefined);
    assert.equal(safe('doc-b', 'tenant-b').body, 'private');
  } finally { db.close(); }
});

test('SQL values: concatenation leaks rows; binding preserves literal input', () => {
  const db = new DatabaseSync(':memory:');
  try {
    db.exec("CREATE TABLE users(name TEXT); INSERT INTO users VALUES('alice'),('bob')");
    const attack = "' OR 1=1 --";
    const unsafe = value => db.prepare(`SELECT name FROM users WHERE name='${value}'`).all();
    const safe = value => db.prepare('SELECT name FROM users WHERE name=?').all(value);
    assert.equal(unsafe(attack).length, 2);
    assert.equal(safe(attack).length, 0);
    assert.equal(safe('alice')[0].name, 'alice');
  } finally { db.close(); }
});

test('SSRF redirect: initial-origin check alone reaches forbidden destination', async () => {
  let privateHits = 0;
  const internal = createServer((req, res) => { privateHits++; res.end('private'); });
  const allowed = createServer((req, res) => {
    if (req.url === '/ok') return res.end('public');
    res.writeHead(302, { location: `http://127.0.0.1:${internal.address().port}/secret` }); res.end();
  });
  internal.listen(0, '127.0.0.1'); allowed.listen(0, '127.0.0.1');
  await Promise.all([once(internal, 'listening'), once(allowed, 'listening')]);
  try {
    const origin = `http://127.0.0.1:${allowed.address().port}`;
    const unsafe = async url => {
      assert.equal(new URL(url).origin, origin);
      return (await fetch(url)).text();
    };
    const safe = async url => {
      assert.equal(new URL(url).origin, origin);
      const response = await fetch(url, { redirect: 'manual' });
      if (response.status >= 300 && response.status < 400) throw new Error('redirect forbidden');
      return response.text();
    };
    assert.equal(await unsafe(origin + '/redirect'), 'private');
    const before = privateHits;
    await assert.rejects(safe(origin + '/redirect'), /redirect forbidden/);
    assert.equal(privateHits, before);
    assert.equal(await safe(origin + '/ok'), 'public');
  } finally {
    await Promise.all([new Promise(r => internal.close(r)), new Promise(r => allowed.close(r))]);
  }
});

test('sessions: deleting client cookie alone leaves replay possible', () => {
  const token = randomBytes(32).toString('hex');
  const hash = value => createHash('sha256').update(value).digest('hex');
  const sessions = new Map([[hash(token), { user: 'alice', expires: 100 }]]);
  const verify = (value, now) => {
    const session = sessions.get(hash(value));
    return session && now < session.expires ? session.user : undefined;
  };
  let cookie = token;
  cookie = undefined;
  assert.equal(cookie, undefined);
  assert.equal(verify(token, 50), 'alice');
  sessions.delete(hash(token));
  assert.equal(verify(token, 50), undefined);
  sessions.set(hash(token), { user: 'alice', expires: 100 });
  assert.equal(verify(token, 100), undefined);
});

test('JWT: signature, audience, expiry and algorithm are separate checks', () => {
  const key = randomBytes(32);
  const encode = object => Buffer.from(JSON.stringify(object)).toString('base64url');
  const signature = input => createHmac('sha256', key).update(input).digest();
  const sign = claims => {
    const input = `${encode({ alg: 'HS256', typ: 'JWT' })}.${encode(claims)}`;
    return `${input}.${signature(input).toString('base64url')}`;
  };
  const decodeOnly = token => JSON.parse(Buffer.from(token.split('.')[1], 'base64url'));
  const verifyFixture = token => {
    const parts = token.split('.'); assert.equal(parts.length, 3);
    const [header, payload, supplied] = parts;
    const h = JSON.parse(Buffer.from(header, 'base64url'));
    assert.equal(h.alg, 'HS256'); assert.equal(h.typ, 'JWT');
    const actual = Buffer.from(supplied, 'base64url');
    const expected = signature(`${header}.${payload}`);
    assert.equal(actual.length, expected.length);
    assert.ok(timingSafeEqual(actual, expected));
    const claims = decodeOnly(token);
    assert.equal(claims.iss, 'fixture-issuer'); assert.equal(claims.aud, 'fixture-api');
    assert.equal(typeof claims.exp, 'number'); assert.ok(claims.exp > 100);
    if (claims.nbf !== undefined) assert.ok(claims.nbf <= 100);
    return claims;
  };
  const claims = { sub: 'alice', iss: 'fixture-issuer', aud: 'fixture-api', exp: 200 };
  assert.equal(verifyFixture(sign(claims)).sub, 'alice');
  const wrongAudience = sign({ ...claims, aud: 'another-api' });
  assert.equal(decodeOnly(wrongAudience).sub, 'alice');
  assert.throws(() => verifyFixture(wrongAudience));
  assert.throws(() => verifyFixture(sign({ ...claims, exp: 99 })));
  assert.throws(() => verifyFixture(sign({ ...claims, nbf: 101 })));
  const forged = `${encode({ alg: 'none', typ: 'JWT' })}.${encode(claims)}.`;
  assert.equal(decodeOnly(forged).sub, 'alice');
  assert.throws(() => verifyFixture(forged));
  const altered = sign(claims).split('.'); altered[1] = encode({ ...claims, sub: 'admin' });
  assert.throws(() => verifyFixture(altered.join('.')));
});

const workerSource = String.raw`
  const { workerData, parentPort } = require('node:worker_threads');
  const { DatabaseSync } = require('node:sqlite');
  const db = new DatabaseSync(workerData.path);
  db.exec('PRAGMA busy_timeout=5000');
  const barrier = new Int32Array(workerData.barrier);
  const oldBalance = workerData.safe ? undefined : db.prepare('SELECT balance FROM account').get().balance;
  Atomics.add(barrier, 0, 1); Atomics.notify(barrier, 0);
  while(Atomics.load(barrier, 0) < 2) Atomics.wait(barrier, 0, 1, 5000);
  db.exec('BEGIN IMMEDIATE');
  let success;
  if(workerData.safe) {
    success = db.prepare('UPDATE account SET balance=balance-80 WHERE balance>=80').run().changes === 1;
  } else {
    success = oldBalance >= 80;
    if(success) db.prepare('UPDATE account SET balance=?').run(oldBalance-80);
  }
  if(success) db.exec('INSERT INTO ledger(amount) VALUES(80)');
  db.exec('COMMIT'); db.close(); parentPort.postMessage(success);
`;
async function concurrentSpend(safe) {
  const dir = await mkdtemp(join(tmpdir(), 'secure-skill-race-'));
  const path = join(dir, 'fixture.sqlite');
  const setup = new DatabaseSync(path);
  setup.exec('CREATE TABLE account(balance INTEGER); INSERT INTO account VALUES(100); CREATE TABLE ledger(amount INTEGER)');
  setup.close();
  const barrier = new SharedArrayBuffer(4);
  const workers=[];
  try {
    const tasks=[0,1].map(() => new Promise((resolve,reject) => {
      const worker = new Worker(workerSource, { eval:true, workerData:{ path,safe,barrier } });
      workers.push(worker);
      const timer = setTimeout(() => reject(new Error('worker timeout')), 12000);
      worker.once('error', error => {clearTimeout(timer); reject(error);});
      worker.once('message', result => {clearTimeout(timer); resolve(result);});
      worker.once('exit', code => {if(code !== 0) reject(new Error('worker exit '+code));});
    }));
    const outcomes=await Promise.all(tasks);
    const check = new DatabaseSync(path);
    try {
      return { successes:outcomes.filter(Boolean).length, spent:check.prepare('SELECT COALESCE(SUM(amount),0) AS total FROM ledger').get().total, balance:check.prepare('SELECT balance FROM account').get().balance };
    } finally { check.close(); }
  } finally {
    await Promise.all(workers.map(w => w.terminate()));
    await rm(dir, { recursive:true, force:true });
  }
}
test('concurrency: separate workers and database connections preserve spending invariant', async () => {
  const insecure=await concurrentSpend(false);
  assert.equal(insecure.successes, 2); assert.equal(insecure.spent, 160); assert.equal(insecure.balance, 20);
  const secure=await concurrentSpend(true);
  assert.equal(secure.successes, 1); assert.equal(secure.spent, 80); assert.equal(secure.balance, 20);
});
```

## Browser XSS fixture

This block was run with Playwright and a temporary headless Edge profile. The imported package is an optional test dependency, not a dependency for reading or using the skill. Test content is created locally. This checks the real DOM behavior of the two sinks; rich HTML sanitization, framework escape hatches, and URL/CSS contexts require their own tests.

```javascript
const assert = require('node:assert/strict');
const { chromium } = require('playwright');
(async () => {
  const browser = await chromium.launch({ channel: 'msedge', headless: true });
  try {
    const page = await browser.newPage();
    const payload = '<img src="data:,invalid" onerror="globalThis.__fixtureXss=1">';
    await page.setContent('<main id="output"></main>');
    await page.evaluate(value => { document.querySelector('#output').innerHTML = value; }, payload);
    await page.waitForFunction(() => globalThis.__fixtureXss === 1);
    await page.evaluate(value => {
      delete globalThis.__fixtureXss;
      document.querySelector('#output').textContent = value;
    }, payload);
    assert.equal(await page.locator('#output img').count(), 0);
    assert.equal(await page.locator('#output').textContent(), payload);
    assert.equal(await page.evaluate(() => globalThis.__fixtureXss), undefined);
    await page.evaluate(() => { document.querySelector('#output').textContent = 'ordinary text'; });
    assert.equal(await page.locator('#output').textContent(), 'ordinary text');
    console.log('PASS XSS: unsafe HTML executes; textContent preserves text and creates no executable element.');
  } finally { await browser.close(); }
})().catch(error => { console.error(error); process.exitCode=1; });
```

## TypeScript runtime fixture

Run the following with a Node version supporting TypeScript type stripping, or the project's existing TypeScript test runner. It demonstrates a runtime boundary; it does not require changing the project's validation library.

```typescript
import assert from 'node:assert/strict';
type ProfileInput = { displayName: string };
type Account = { displayName: string; role: 'user' | 'admin' };
const existing: Account = { displayName: 'Old name', role: 'user' };
const input: unknown = JSON.parse('{"displayName":"Alice","role":"admin"}');
const unsafe = { ...existing, ...(input as ProfileInput) };
assert.equal(unsafe.role, 'admin'); // A type assertion did not remove the field.
function updateProfile(value: unknown): Account {
  if (typeof value !== 'object' || value === null ||
      !('displayName' in value) || typeof value.displayName !== 'string' ||
      value.displayName.length > 100) throw new Error('invalid profile');
  return { ...existing, displayName: value.displayName };
}
const safe = updateProfile(input);
assert.equal(safe.role, 'user');
assert.equal(safe.displayName, 'Alice');
assert.throws(() => updateProfile({ displayName: 12 }));
assert.throws(() => updateProfile(null));
console.log('PASS TypeScript: runtime validation and field selection prevent privilege-bearing mass assignment.');
```

## Manual workflow checks

- Implementation request: add an authorized export; inspect the existing guard and storage conventions, enforce the tenant boundary, test a denied request and a permitted request. Do not propose replacing the identity provider.
- Review-only request: inspect that same change and return evidence-supported findings without modifications. A scoped parameterized query and correctly authorized tenant are a safe counterpart, not an injection or IDOR finding.
- Authentication request: use the existing provider's verifier and revocation model. A correctly signed access token for another audience must be rejected; a valid intended token remains accepted.

These checks were walked through against the skill instructions and the executable fixtures by the authoring agent. They are not independent model evaluations or proof that every future invocation will follow the instructions.
