# Bcrypt

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `fac87a09df258a88329d7225bc8c6789eca5ecbebc9112ae1ea13a13c60e997f`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


Bcrypt is one of the oldest password-hashing algorithm still commonly in used. It is configured using a single cost (work) parameter where increasing this value by 1 doubles the computational workload. I recommend setting the cost factor to at least 10.

Bcrypt has a maximum password length limit of 72 bytes. This includes the null terminator so the effective limit is 71 bytes. Note that this is in bytes and not in characters. An UTF-16 or UTF-8 encoded character can take up to 4 bytes. As such, I recommend limiting user passwords to printable ASCII characters as 1 character will always take 1 byte. Additionally, because Bcrypt expects a null-terminated string, it cannot handle raw binary data. Do not directly pass the output of other hashing and encryption algorithms into it.

Most implementations handle salting internally and output a formatted-string that includes the hash, salt, and cost parameter.

```text
$2a$10$TEBu87xaQfRF3lrFq2mfxe.HScQPBKFpmuvhCywrIAri3gvTtwhwO
```

Both the fastest consumer-grade GPU (RTX 5090) and the fastest enterprise-grade GPU (H100) can calculate about 20,000 hashes/second at a cost value of 9. The H100s cost about $2 to $5 per hour to rent. Note that dedicated ASIC/FPGA-based hardware can calculate hashes at 10 times the efficiency, though these are hard to acquire and run.
