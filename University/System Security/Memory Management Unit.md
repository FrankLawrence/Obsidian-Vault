---
Tags: 
Created: 2026-01-09 00:03:05
aliases: MMU
---
The **Memory Management Unit** is responsible for translating [[Memory Management#Virtual Memory|virtual addresses]] into physical addresses by traversing the appropriate [[Four-Level Page Tables.canvas|page tables]].

> [!info]- Structure of PML4 Entry
| Bit(s) | Field       | Meaning                                         |
| ------ | ----------- | ----------------------------------------------- |
| 0      | P (Present) | 1 if the table below is in memory               |
| 1      | **R/W**     | 1 if writable                                   |
| 2      | **U/S**     | 1 if user-mode can access, super-user otherwise |
| 3      | PWT         | Page write-through                              |
| 4      | **PCD**     | Cache disable                                   |
| 5      | A           | Accessed                                        |
| 7      | PS          | Page size (usually 0 here)                      |
| 12-51  | Address     | Physical base address of next table (PDPT)      |
| 63     | **NX**      | No-execute bit (if enabled)                     |
