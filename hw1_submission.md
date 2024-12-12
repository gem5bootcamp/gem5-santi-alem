# Cache Coherence

## **Question 1**

For algorithm 1, does increasing the number of threads improve performance or hurt performance? Use data to back up your answer.

**Response**

For algorithm 1, on average it seems to be performing equal or worse than using only one thread. The following results are from averaging the execution time over 200 times. 

| Number of Threads | 1 | 2 | 4 | 8 |
| --- | --- | --- | --- | --- |
| **Average Time in ms** | 1,28894165 | 1,69460185 | 1,3573944 | 1,26359755 |

## **Question 2**

(a) For algorithm 6, does increasing the number of threads improve performance or hurt performance? Use data to back up your answer.

(b) What is the speedup when you use 2, 4, 8, and 16 threads (only answer with up to the number of cores on your system).

**Response**

For algorithm 6 it shows an improvement when adding cores with the exception of using 8 cores.

| Number of Threads | 1 | 2 | 4 | 8 |
| --- | --- | --- | --- | --- |
| **Average Time in ms** | 1,3183577 | 1,2004183 | 1,1845623 | 1,2397706 |
| Speedup | 1 | 1.0982 | 1.1129 | 1.0633 |

## **Question 3**

(a) Using the data for all 6 algorithms, what is the most important optimization, chunking the array, using different result addresses, or putting padding between the result addresses?

(b) Speculate how the hardware implementation is causing this result. What is it about the hardware that causes this optimization to be most important?

**Response**

| Algorithm | 1 Thread | 2 Threads | 4 Threads | 8 Threads |
| --- | --- | --- | --- | --- |
| 1 | 1.2514412ms (1.0) | 1.7194673 (0.7500) | 1.3612725 (0.9473) | 1.2910531 (0.9989) |
| 2 | 1.2563356 (1.0) | 1.6926515 (0.7619) | 1.3517052 (0.9541) | 1.2863202 (1.0025) |
| 3 | 1.2734703 (1.0) | 1.6964859 (0.7602) | 1.3536823 (0.9527) | 1.2938659 (0.9967) |
| 4 | 1.2849584 (1.0) | 1.6983620 (0.7593) | 1.4194049 (0.9085) | 1.2802978 (1.0073) |
| 5 | 1.2893826 (1.0) | 1.1704241 (1.1018) | 1.2023973 (1.0725) | 1.2889459 (1.0005) |
| 6 | 1.2896000 (1.0) | 1.1871590 (1.0863) | 1.1985390 (1.0760) | 1.2918683 (0.9982) |

[data.txt](Cache%20Coherence%2014bdd60ead83800e9c30f3a683797aa1/data.txt)

Padding the result addresses seemed to has the most speedup when incrementing threads, it dips when reaching 8 cores. This optimization keeps the threads from writing data to the same block in the cache, each one having its own block to write. 

## **Question 4**

(a) What is the speedup of algorithm 1 and speedup of algorithm 6 on *16 cores* as estimated by gem5?

(b) How does this compare to what you saw on the real system?

|  | 1 | 16 | speedup |
| --- | --- | --- | --- |
| Algorithm 1 | 0.000444 simSeconds | 0.000473 simSeconds |  0.9387 |
| Algorithm 6 | **0.00045 simSeconds** | **0.000088 simSeconds** | **5.1136** |

The speedup is a lot higher with all the optimizations than the tests on real hardware. 

## **Question 5**

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the *hit ratio?*

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

| **Algorithm** | **Hits** | **Misses** | **Hit Ratio** |
| --- | --- | --- | --- |
| 1 | 212080 | 34251 | 0.8609553812 |
| 2 | 298087  | 18703 | 0.9332676821196779 |
| 3 | 228005 | 34119 |  0.8698364133005753 |
| 4 | 293884 | 18513 | 0.9407388675307381 |
| 5 | 244528 | 17956 | 0.931592020846985 |
| 6 | 298087 | 2689 | 0.9910597920046812 |

When comparing  to the naive algorithm L1D cache hit ratio with the algorithm 2, 3 and 5. The one with the biggest impact was chunking the array (2). Padding the result addresses (5) has a similar impact in hit ratio. Just using different result addresses has almost no impact.

## **Question 6**

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the *read sharing?*

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

| **Algorithm** | L1D Read Sharing |
| --- | --- |
| 1 | 411     30.29%     30.29% |         784     57.77%     88.06% |          14      1.03%     89.09% |          11      0.81%     89.90% |          12      0.88%     90.79% |          11      0.81%     91.60% |          11      0.81%     92.41% |          11      0.81%     93.22% |          12      0.88%     94.10% |          11      0.81%     94.92% |          13      0.96%     95.87% |          12      0.88%     96.76% |          12      0.88%     97.64% |           9      0.66%     98.31% |          10      0.74%     99.04% |           6      0.44%     99.48% |           7      0.52%    100.00% |
| 2 | 408     68.34%     68.34% |          19      3.18%     71.52% |          15      2.51%     74.04% |          13      2.18%     76.21% |          14      2.35%     78.56% |          13      2.18%     80.74% |          13      2.18%     82.91% |          13      2.18%     85.09% |          13      2.18%     87.27% |          13      2.18%     89.45% |          14      2.35%     91.79% |          10      1.68%     93.47% |           7      1.17%     94.64% |          10      1.68%     96.31% |           8      1.34%     97.65% |           6      1.01%     98.66% |           8      1.34%    100.00% |
| 3 | 417     30.50%     30.50% |         785     57.43%     87.93% |          14      1.02%     88.95% |          11      0.80%     89.76% |          12      0.88%     90.64% |          11      0.80%     91.44% |          13      0.95%     92.39% |          11      0.80%     93.20% |          15      1.10%     94.29% |          12      0.88%     95.17% |          11      0.80%     95.98% |          11      0.80%     96.78% |          10      0.73%     97.51% |          13      0.95%     98.46% |           8      0.59%     99.05% |           7      0.51%     99.56% |           6      0.44%    100.00% |
| 4 | 415     67.26%     67.26% |          19      3.08%     70.34% |          15      2.43%     72.77% |          13      2.11%     74.88% |          14      2.27%     77.15% |          13      2.11%     79.25% |          15      2.43%     81.69% |          11      1.78%     83.47% |          16      2.59%     86.06% |          14      2.27%     88.33% |          13      2.11%     90.44% |          13      2.11%     92.54% |          12      1.94%     94.49% |          15      2.43%     96.92% |           8      1.30%     98.22% |           6      0.97%     99.19% |           5      0.81%    100.00%  |
| 5 | 413     29.88%     29.88% |         782     56.58%     86.47% |          11      0.80%     87.26% |          14      1.01%     88.28% |          15      1.09%     89.36% |          11      0.80%     90.16% |          11      0.80%     90.96% |          14      1.01%     91.97% |          14      1.01%     92.98% |          13      0.94%     93.92% |          12      0.87%     94.79% |          13      0.94%     95.73% |          13      0.94%     96.67% |          14      1.01%     97.68% |          12      0.87%     98.55% |          14      1.01%     99.57% |           6      0.43%    100.00% |
| 6 | 461     72.71%     72.71% |          28      4.42%     77.13% |           7      1.10%     78.23% |           6      0.95%     79.18% |           9      1.42%     80.60% |          11      1.74%     82.33% |          11      1.74%     84.07% |          11      1.74%     85.80% |          10      1.58%     87.38% |          11      1.74%     89.12% |          10      1.58%     90.69% |          10      1.58%     92.27% |          11      1.74%     94.01% |          12      1.89%     95.90% |          12      1.89%     97.79% |          10      1.58%     99.37% |           4      0.63%    100.00% |

If we ignore the first controller chunking the array (algorithm 2) has the biggest impact on read sharing comparing to algorithm 3 and 5, which have similar read sharing to the naive implementation. This makes sense since each thread is reading from a different portion of the array so this lowers the need for each controller to read from the same addresses.

## **Question 7**

Which optimization (chunking the array, using different result addresses, or putting padding between the result addresses) has the biggest impact on the *write sharing?*

Show the data you use to make this determination. Discuss which algorithms you are comparing and why.

| **Algorithm** | L1D Write Sharing |
| --- | --- |
| 1 | 41      0.25%      0.25% |         978      5.92%      6.17% |        1037      6.28%     12.45% |        1036      6.27%     18.73% |        1037      6.28%     25.01% |        1037      6.28%     31.29% |        1037      6.28%     37.57% |        1037      6.28%     43.85% |        1037      6.28%     50.13% |        1037      6.28%     56.41% |        1037      6.28%     62.70% |        1025      6.21%     68.90% |        1037      6.28%     75.18% |        1027      6.22%     81.41% |        1026      6.21%     87.62% |        1025      6.21%     93.83% |        1019      6.17%    100.00% |
| 2 | 41      0.25%      0.25% |         958      5.81%      6.06% |        1033      6.27%     12.33% |        1037      6.29%     18.62% |        1037      6.29%     24.91% |        1037      6.29%     31.20% |        1037      6.29%     37.49% |        1037      6.29%     43.78% |        1037      6.29%     50.07% |        1037      6.29%     56.36% |        1037      6.29%     62.65% |        1027      6.23%     68.88% |        1026      6.22%     75.10% |        1026      6.22%     81.33% |        1028      6.24%     87.56% |        1030      6.25%     93.81% |        1020      6.19%    100.00% |
| 3 | 40      0.24%      0.24% |         979      5.95%      6.20% |        1037      6.30%     12.50% |        1036      6.30%     18.80% |        1037      6.30%     25.10% |        1038      6.31%     31.41% |        1038      6.31%     37.72% |        1038      6.31%     44.04% |        1024      6.23%     50.26% |         974      5.92%     56.18% |        1035      6.29%     62.48% |        1037      6.30%     68.78% |        1038      6.31%     75.09% |        1037      6.30%     81.40% |        1025      6.23%     87.63% |        1023      6.22%     93.85% |        1012      6.15%    100.00% |
| 4 |  40      0.24%      0.24% |         958      5.86%      6.10% |        1032      6.31%     12.41% |        1037      6.34%     18.75% |        1037      6.34%     25.09% |        1038      6.35%     31.43% |        1038      6.35%     37.78% |        1024      6.26%     44.04% |        1022      6.25%     50.28% |         932      5.70%     55.98% |        1028      6.28%     62.27% |        1037      6.34%     68.60% |        1038      6.35%     74.95% |        1037      6.34%     81.29% |        1026      6.27%     87.56% |        1023      6.25%     93.81% |        1012      6.19%    100.00% |
| 5 |  44     19.73%     19.73% |          13      5.83%     25.56% |           9      4.04%     29.60% |           3      1.35%     30.94% |          14      6.28%     37.22% |           1      0.45%     37.67% |          16      7.17%     44.84% |          13      5.83%     50.67% |          13      5.83%     56.50% |          13      5.83%     62.33% |          13      5.83%     68.16% |          13      5.83%     73.99% |          14      6.28%     80.27% |          14      6.28%     86.55% |          14      6.28%     92.83% |          14      6.28%     99.10% |           2      0.90%    100.00% |
| 6 | 51     21.16%     21.16% |          16      6.64%     27.80% |           8      3.32%     31.12% |           6      2.49%     33.61% |           2      0.83%     34.44% |          15      6.22%     40.66% |          15      6.22%     46.89% |          14      5.81%     52.70% |          14      5.81%     58.51% |          14      5.81%     64.32% |          14      5.81%     70.12% |          14      5.81%     75.93% |          14      5.81%     81.74% |          14      5.81%     87.55% |          14      5.81%     93.36% |          14      5.81%     99.17% |           2      0.83%    100.00% |

If we ignore the first controller padding the result addresses (algorithm 5) has the biggest impact on write sharing comparing to algorithm 2 and 3. We can assume that since each thread is writing the result in a different cache block we lower the chance of them writing in the same address. 

## **Question 8**

Let's get back to the question we're trying to answer. From [question 3](https://file+.vscode-resource.vscode-cdn.net/e%3A/gem5-bootcamp/latin-america-2024-1/homework/cache-coherence/README.md#question-3) above, "What is it about the hardware that causes this optimization to be most important?"

So: (a) Out of the three characteristics we have looked at, the L1 hit ratio, the read sharing, or the write sharing, which is most important for determining performance? Use the average memory latency (and overall performance) to address this question.

Finally, you should have an idea of what optimizations have the biggest impact on the hit ratio, the read sharing performance, and the write sharing performance.

So: (b) Using data from the gem5 simulations, now answer what hardware characteristic *causes* the most important optimization to be the most important.

| **Algorithm** | Average Miss Latency | SpeedUp |
| --- | --- | --- |
| 1 | 498.844054 | 0.9386892177589852 |
| 2 | 835.772914 | 0.9426751592356688 |
| 3 | 244.90216 | 1.6204379562043796 |
| 4 | 407.315737 | 1.6727941176470589 |
| 5 | 64.969728 | 4.303921568627451 |
| 6 | 86.362826 | 5.113636363636363 |

The write sharing seems to indicate which optimization is the most determinant, since it aligns with the improvement in speedup and lower miss latency.

The MESI protocol for cache coherence allows for a cache block to be "read shared" between many different L1 caches. Padding the cache addresses mitigates invalidating cache by always writing the result in different blocks, so it does not invalidates the blocks being read by other threads. 

## **Question 9**

Run using a `xbar_latency` of 1 cycle and 25 cycles (in addition to the 10 cycles that you have already run).

As you increase the cache-to-cache latency, how does it affect the importance of the different optimizations?

You don't have to run all algorithms. You can probably get away with just running algorithm 1 and algorithm 6.

| Algorithm | 1 Cycle | 10 Cycle | 25 Cycle |
| --- | --- | --- | --- |
| 1 | 0.000188 | 0.000473 | 0.000898 |
| 2 | 0.000187 | 0.000471 | 0.000507 |
| 3 | 0.000124 | 0.000274 | 0.000503 |
| 4 | 0.000123  |  0.000272 | 0.000291 |
| 5 | 0.000082 | 0.000102 | 0.000135 |
| 6 | 0.000078 | 0.000088 | 0.000104 |

Increasing the cache-to-cache latency makes more evident the impact of algorithm 5 optimizations, this indicate that reducing write sharing might be an important in systems with more cache latency.