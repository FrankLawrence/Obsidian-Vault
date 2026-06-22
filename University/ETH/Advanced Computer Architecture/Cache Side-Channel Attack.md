---
Tags: 
Created: 2026-06-15 13:41:58
---

The cache side-channel attack is the oldest and perhaps most powerful example of a [[Microarchitectural Side-Channel Attacks]] (cache shared but not architecturally visible). An attacker can *differentiate hits and misses* using some high-resolution timing measurement (e.g. processor cycles). When the victim memory is accessed (*where* the victim loads or stores), it leaks secrets.

Examples of cache side-channel attacks in practice:
- D$ accesses to an [[AES]] `sbox()` depend on the secret key
- I$ accesses to different [[RSA]] functions depend on the secret key ([[Side Channel Attacks#Timing analysis of RSA|RSA timing attack]])

This is dependent on the attack being able to *run victim code* (e.g. write to a file into an encrypted volume, send packets through a [[Virtual Private Networks|VPN]] interface).

Both of the following strategies assume knowledge on [[The Memory System#Set-Associative Mapping|set-associative cache]].
# Evict+Time
1. Run the victim code which populates cache with blocks
2. Run the victim again and time it (this should be fast because all data are in cache already)
3. Evict content from a specific set by replacing with attacker content. Make sure to pollute *all ways* of this set
4. Run the victim and time it -> If this takes longer than before then we know the victim accessed something in that set

This approach answers the question: Does the victim access location $X$ (in set $Y$)?

![[Cache SCA Evit+Time.svg|500]]

The problem with this approach is that it relies on measuring the *precise execution time of the victim*. We need to repeat the *exact same execution*, so no variability in the executed code. Other environmental factors such as [[system call]], branch prediction and instruction scheduling can lead to varying profiling measurements. These slight deviations can be filtered out by *repeating the experiment many times*.

# Prime+Probe
1. Fill all sets with attacker content (*prime*)
2. Read all pieces of data for all sets and time each set, which must be fast because all data are in the cache
3. Run the victim code
4. Read all pieces of data for all sets and time each set (*probe*) -> If this takes longer for a specific set than before then we know the victim accessed something in that set

This approach answers the question: What location (set) does the victim access?
The key advantage over evict+time is the attacker *times their own code* and not the victim's code, arguably allowing *better control of measurement noise*.

This attack is used for [[tracking the page-level execution state of CVMs]].

---
References: