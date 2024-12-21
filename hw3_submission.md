A) What were you able to get working, and why do you think you couldn't achieve the final objective?

- I managed to build the secure memory with the addition of the metadata cache, but I encountered what seems to be a deadlock. When verifying the metadata from memory, the process never reaches the root packet and hangs, even though a memory request was issued. I suspect that I may not be handling the cache port events correctly, or there could be a deadlock threshold configuration that I was unable to locate or modify. I observed that some ports trigger a panic alert if no response occurs within a specified number of cycles. When I ran the build in the codespace, it failed with a deadlock panic alert, but locally, it simply hangs without further output.
B) Describe the experiment you would run if everything were working.

- If everything were working, I would run several workloads under various combinations of cache sizes and replacement policies, comparing the statistics between them. The goal would be to determine whether these changes significantly impact performance. I would also compare the performance of the secure memory with the metadata cache against the secure memory without the cache. I would have paid attention to the hit ratio of the metadata cache, the simulated time and IPC of each configuration.

C) Run some applications (e.g., the getting started suite or matrix multiplication) using the baseline, and discuss the potential impact of the secure memory changes.

- I believe that secure memory with the metadata cache would improve performance compared to secure memory without it. However, I suspect that cache size and replacement policies would have only a minor impact on performance on the simulated seconds, with certain policies potentially performing worse than the default.
