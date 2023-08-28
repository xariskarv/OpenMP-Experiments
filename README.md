# OpenMP-Experiments

### Explanation
This project tries to parallellize common algorithms and it shows the speedup that might exist compared with the sequential form of these algorithms. The algorithms which will be used are :
 * Countsort
 * Backsub (Backward substitution) . Backward substitution is the method which is used after Gaussian Elimination, to solve large linear systems.
 * String matching (Calculates how many times a specific word can be found in a large text file).

In this project the file that is being used for the string matching algorithm is the **enwik8.zip** can be downloaded from here: https://mattmahoney.net/dc/textdata.html 

**_Important note!_** The parallel algorithms have been implemented using the **Intel(R) Core(TM) i3-3220 CPU @ 3.30GHz 3.30 GHz** which has only 2 cores. If a CPU with more cores will be used, there might be a bigger speedup as the upper limit of number of threads which can be used will be increased.

### Parallel results
1)**Countsort**

The Countsort algorithm can be parallelized easily with a simple **#pragma omp parallel for** outside of the outer loop. The algorithm doesn't contain any loop dependencies or critical sections which must be protected. Its thread uses its own private variables and the only shared variables are the matrices (x,y) and the size of the array which will be sorted (n). Below the graph shows the execution speed for 1,2,4,8 and 16 threads for n=10.000:

![Countsort Efficiency](https://github.com/xariskarv/OpenMP-Experiments/assets/125483991/2a50bd9e-0283-4f3a-bd8f-b72ac53a0cff)

As the graph shows, if the countsort algorithm runs sequentially it needs aproximatelly 0.656 seconds to be executed. With parallel implementation, using 8 threads it can be executed in 0.187 seconds so it can create x3.5 speedup.

2)**String Matching**

The string matching algorithm uses a brute force strategy to find a specific word in a lrage text document. This algorithm also is pretty simple to be parallalelized. Below is the graph which shows the performance again for 1,2,4,8,16 threads:

![String_matching_Efficiency](https://github.com/xariskarv/OpenMP-Experiments/assets/125483991/2527c175-5409-4313-a45f-ef83440cbfcf)

As the graph shows, the best performance can be gained with 4 or 8 threads. Parallel execution best time: 0,135 seconds. Sequential execution time: 0.253 seconds. Speedup = 1.87

3)**Backsub**

The backward substitution algorithm cant be parallelized efficiently with the given code. The outer loop uses a sum variable which must be private in order to parallelize the outer loop. The problem is that every next outer loop iteration must be calculated from the previous one. So by default the outer loop must be executed sequentally so the next x values will not be 0. The only part that can be executed with parallelization is the inner loop with a #pragma omp parallel and a reduction clause for the sum variable, which does not give any efficiency, in contrast it makes the time worse. Below is the graph for 1,2,4,8,16 threads and a 10000x10000 linear system.

![Backsub_Efficiency](https://github.com/xariskarv/OpenMP-Experiments/assets/125483991/f76a1a15-99fb-45d8-96a5-3fedc33d3d6a)

It is clear that the parallel execution gives worse time and efficiency as the number of threads is being increased.


