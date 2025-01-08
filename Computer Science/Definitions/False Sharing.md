If different items of the array are on the same *cache line*, releasing a lock may give cache coherence overhead after all.

This can be avoided by padding: The array size is say quadrupled, and slots are separated by three (unused) places in the array.

Guidelines to avoid false sharing:  
- Fields that are accessed independently should end up on different cache lines (e.g. by padding).  
- Keep read-only data separate from data that is modified often.
- Split an object into thread-local pieces.
- If a lock protects data that is frequently modified, then keep lock and data on different cache lines.