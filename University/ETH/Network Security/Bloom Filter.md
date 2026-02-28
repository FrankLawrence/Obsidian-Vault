Given is a vector of size $m$, and $k$ hash functions $h_{i}$ that map to $\{0,...,m\}$. For each hash, we insert it into the bloom filter array.
When testing membership of an element, check if value is set. If a position is *not set*, then the element is not in the set.
**Membership tests in this data structure may be false positives, but never a false negative**.

![[Bloom Filter.svg|700]]