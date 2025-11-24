# [Garbage Collection in kdb+ — Key GC Notes](https://dataintellect.com/blog/garbage-collection-kdb/)

| Item | Type | What it is / does | role related to GC |
|---|---|---|---|
| `-g 0` | Startup flag | Default GC mode: freed blocks are **kept and recycled internally**, not returned to OS | Explains why `used` can drop but `heap` stays high; best for performance |
| `-g 1` | Startup flag | Immediate GC for **large blocks (>32MB)**: such blocks are returned to OS right away | Does **not** auto-return small (≤32MB) blocks, so `heap` may remain high after deletes |
| `.Q.gc[]` | System function | Manual GC: returns large free blocks to OS and **coalesces adjacent small free blocks** so they can be returned | Works regardless of `-g`; key tool when `-g 1` doesn’t shrink `heap` |
| `.Q.w[]` | System function | Prints q process memory stats | Essential to observe GC effects before/after (`used`, `heap`, `peak`) |
| `used` | `.Q.w[]` field | Portion of heap **currently in use** | Drops after `delete`; confirms data is actually freed |
| `heap` | `.Q.w[]` field | Total memory **allocated from OS** to the q process | Primary GC target; may not shrink automatically |
| `peak` | `.Q.w[]` field | Historical maximum heap size | Shows how far heap has grown; useful for capacity/ops context |
| `delete x from \`.'` | Command | Deletes a variable/table from the root namespace | Frees objects (`used` decreases) but may not return memory to OS without GC |

