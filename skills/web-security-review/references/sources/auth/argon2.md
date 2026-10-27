# Argon2

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `b173a5f74e0ad7f9eb6d916740fd4bb8af7b2f4552a0e1987dce56084299c057`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


Argon2 is a newer family of password hashing algorithms. It is designed to be memory-intensive to thwart attacks using GPUs and other dedicated hardware. I recommend using Argon2id with at least 16MiB of memory, 3 iterations, and 1 degree of parallelism.

Argon2 has three variations: Argon2i, Argon2d, and Argon2id. Argon2i is designed to be resistant against side-channel attacks, whereas Argon2d offers superior resistance against GPU-based attacks. Argon2id combines the mechanisms of both variants to provide a balanced defense.

The algorithm is configured using 3 main parameters. First is the memory size, which determines the size of memory each process uses. Doubling the memory size doubles the runtime. Ideally this should be larger than 128MiB to truly see the benefits on Argon2 but this is unrealistic for most web applications. Next is the iteration count. If you're using Argon2id, use a minimum of 3 iterations. Finally, the degree of parallelism. This dictates the number of parallel threads utilized per hashing operation. While increasing parallelism speeds up individual execution times, it doesn't affect the number of overall concurrent hashing operations your server can handle. I recommend always using a value of 1 to maximize total server throughput.

Argon2 can also be configured to output any size of data, but it is typically set to 32 bytes.

There is no standardized output format but the PHC string format is often used. It takes the form:

```text
$argon2id$v=19$m=MEMORY,t=ITERATION,p=PARALLELISM$SALT$HASH
```

where `MEMORY` is the memory in KiB, `ITERATION` is the iteration count, `PARALLELISM` is the degree of parallelism, `SALT` is the salt encoded in base64, and `HASH` is the hash encoded in base64.

The fastest consumer-grade GPU (RTX 5090) can calculate about 10,000 hashes/second and the fastest enterprise-grade GPU (H100) can calculate about 20,000 hashes/second at the recommended configuration. The H100s cost about $2 to $5 per hour to rent.
