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
