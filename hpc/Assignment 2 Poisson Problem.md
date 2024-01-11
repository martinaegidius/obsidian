Is 3D because processors have become too strong 
- Note: just one added loop. Can look at old solution

You solve second order differential equation in 3 dims using jacobi-iterations
- Equation describes e.g. steady state heat distribution in a medium with constant heat capacity

You make it using Jacobi iterations
You need two arrays to do it (saving old and new values)
- Swap the matrices after computing ie. save to "old" for new values each time to save memory
- Num ops is O(N^2 log(N))
	- At worst-case
	- But in practice after 1000s-10000s you have already hit floating point precision

Gauss-seidel: double as fast, in-place
"Lexicographical ordering": row-major
- But: hard to parallelize compared to Jacobi. Jacobi has better independence. 

Use Frobenius norm (faster compute)
You should always scale problems between -1 and 1, as the floating point is most precise here

You should allocate boundary-points as arrays. This is faster than if-statements in the inner loop -> only need table lookup. 

Numerical performance is FLOP/s. Algorithmic performance is O(...)

Memory bound problem - FLOP/s is not the best metric 

`[i][j][k]`: k is the "next" element in physical memory. Ensure quickest loop along k. 
- You need to choose the best idx for x,y,z for peak algorithmic performance

<b> Bernd Tips</b>
- Memory band-width will be a problem. The number of updates per second (lattice updates per seconds, lup/s, number of updated gridpoints/second)
	- Divide by number of memory operations -> gives you a number for how well you utilize memory bandwidth, will plateau at some point
	- From lup/s you can get mflop/s, mops/s -> bytes/s (and compare to the processor sheet, are you hitting peak bytes/s?)
- Speed-up curves: S(P) as function of p. If you're lucky, it follows linear and drops off /plateau at the memory-bandwidth constraint. 
- 2D example: he always get's this question
	- Does a corner boundary have 20 or 0? 
		- Doesn't matter, you never hit the corner point. It is never checked. 
		- Bernd puts an insanely large number there to catch bugs (9999 - then if you have bug you will see heat dissipation from corner)


<h4>Further points 10th of january: </h4>
Seems like it mostly regards jacobi: 
- You stop based on iter_max and threshold-norm
- When you compare two algorithms, you need to stop only on the threshold, not iter_max, ie. iter_max = infinity
	- For sequential 
- Parallel jacobi should be easy, parallel gauss seidel harder (you SHOULD use doacross loops)
- When you have parallel results, you should compare to sequential ones. To check quality of results:
	- Run a small problem through threshold criterion. To check if they are equal, you should hit through the threshold at the SAME number of iterations. Check this
	- When you compare the speed-up to the sequential, you run to a fixed iter_max, disregarding the threshold (ie. threshold = 0.0)
		- Choose a sane number of iter_max:
			- Choose iter-max so sequential time is approximately 2 minutes. The speedup-curve is constant anyways disregarding the run-time. So you want a small sequential run-time. 

- <b> batch-constraints </b> Batch-system job-time is 1 hour for hpcintro. Number of cores per user is one. 
- A good idea for speed-up measurement is to ensure you get a FULL node 
	- bsub -n X: X cores for your threads. Should not do this, because you are not sure which cores you get
	- Use bsub -n 24 instead (ask for full machine)
		- Inside batch-job loop over number of threads t= 1, 2, 4, 8, 12 , 16, 24 
			- OMP_NUM_THREADS=$t 
		- In this way you are sure you get the full machine, which allows you to play with the affinity. IT HAS EFFECT 
			- 12 cores / socket 0 with MEM0, 12 cores / socket 1 MEM1. 
				- IE: IT IS A NUMA-SYSTEM. So affinity is relevant
- In assignment he asks you to do <b>a simple jacobi parallelization</b>. It has only a single pragma. This should be your baseline. It should not be perfect. It should be "naive" parallelization.


Think about optimizing your sequential versions. In both algorithms, you have grid-points <b>u</b>, and radiator <b>f</b>. You should implement <b>f</b> as a 3D array, so that you do not branch inside of loops. The branches will slow you down considerably in parallel-versions
- Radiator = f = "source term"
- Bernd says compilers in general are smart enough to optimize array-indexation


All openMP inside jacobi and gauss-seidel. The main should only alloc and initialize data. Don't use 
openMP in the main. 

You should hand-in 4 files - sequential and openMP compiled code. 


Checking Gauss-Seidel:
Bernd has given you routines which you can dump. You can visualize with paraView. Run a checksum (e.g. MD5) on binaries. If it works you get the same checksum on binary files independent of the number of threads.

It is a memory-bound problem. You should look at the memory-bandwidth utilized, and play with the affinity to see if it becomes better. 

DONT WASTE TOO MUCH TIME DOING TOO MANY EXPERIMENTS. But consider problem-sizes, and if they fit in the cache. 

Function calls are expensive. Stencil-updates should be done directly in the triple-loop, not in a seperate function.

NEVER initialize parallel environment inside loop. This gives you huge overhead. 






<h1> Question you should ask </h1>



























