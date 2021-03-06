# Permanant_Temporary_MemoryAllocator

Inspired by [EASTL](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/n2271.html#Appendix_18)       
```
7 - Permanent and temporary memory
A useful technique for reducing memory fragmentation which is often used by game developers is to separate allocated memory into two classes: temporary memory and permanent memory. Some EA games have consumed system memory completely enough that that they would fail to run without this optimization. The way it works is that memory which is allocated once on startup is deemed to be permanently allocated. Similarly, memory which is allocated once for a new game level is deemed to be permanently allocated. Memory which is dynamically allocated and freed in a arbitrary or out-of-order way is deemed to be temporarily allocated. Memory which is permanent is allocated from the top of the heap downward, and memory which is temporary is allocated from the low end of the heap.

[temporary memory -->            (free space)             <-- permanent memory]
(low memory)                                                      (high memory)
The result is that permanent allocations are tightly packed together in high memory and don't create dead spots in low memory. Sometimes permanent and temporary memory are sometimes referred to as high and low memory, for reasons that should be obvious. This technique is useful on both fixed-memory systems and on mapped virtual memory systems such as Windows. The custom heaps in use at EA explicitly support the concept of permanent and temporary (high and low) memory.
```
-
```
30 - Problems created by global memory allocators
It is mentioned elsewhere in this document that game software often strives to avoid global memory allocation (e.g. global operator new). We take a moment here to point out some of the practical problems in high performance software.

Built-in global operator new and malloc provide no way to tell where the memory request is coming from and thus all memory requests are necessarily treated equivalently.
As a result of the above, memory usage per subsystem is hard to measure and enforce because you have no way to categorize it.
As a result of the above, memory allocations often get haphazardly strewn across the memory space, resulting in poor locality and cache misses.
Fragmentation occurs more easily because you have no knowledge of temporal allocation locality.
You need to protect the global heap from thread collisions and false sharing. Solutions such as Hoard help with this but at an often unacceptable wasted memory overhead.
Allocation speed is slowed by systems having to deal with other systems' free blocks. System X allocates a lot of size N, system Y allocates a lot of size M. Mix these together in free pools and it takes longer to wade through them.
The user of a given application subsystem can't tune the allocator behavior for its needs, as anything that is done to it modifies the behavior globally for all subsytems.
```
