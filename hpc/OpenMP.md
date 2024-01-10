

<h3> Debugging: Works for OpenMP up to version 4.0 which does not include doacross loops </h3>
suncc -g -fast -xopenmp -xinstrument=datarace \ -o output.o dependency.c
OMP_NUM_THREADS=4 collect -r on ./output.o

er_print -races tha.1.er
- As soon as you find a race, stop experiment, check output, fix data-race, because the experiment is really slow, especially when you have data-races


<b>Never use -xinstrument-compiled code as production, because it is so slow. </b>


<h3>Scheduling</h3>
- pragma omp for schedule(static[,chunk])
	- Default setting
	- Divide iterations into pieces of size chunk and statically assign to the threads
	- If not specified, work N is equally divided among threads (P) with chunk N/P. 
		- If chunks are less than N/P, you statically assign chunks from the start untill N work is done. Ie. no change if "hanging"
- pragma omp for schedule(dynamic[,chunk])
	- Still chunked work, but default chunk-size 1. When a thread finishes one chunk, it is assigned a new chunk
- pragma omp for schedule(guided[,chunk])
	- Chunk size is exponentially reduced with each chunk that gets dynamically assigned to the threads 
		- Not usually used. Bernd usually uses static or dynamic. It may make sense for triangular matrix-solving
- pragma omp for schedule(runtime[,chunk])
	- Controlled by runtime variable OMP_SCHEDULE=type,chunk
	- Bernd likes this, it is very practical 
	- Note: When this is used, no setting of the OMP_SCHEDULE will default to DYNAMIC,1 scheduling (kinda breaking the specification requirements..)
	- 


![[Pasted image 20240109103223.png]]


omp_get_num_threads() gives current open threads (1 outside parallel region, n inside region)
omp_get_max_threads() gives the sys flag OMP_NUM_THREADS
to get the time in seconds 
	double ts = omp_get_wtime()
	double te = omp_get_wtime() 
	double time = te-ts;

environ vars
- OMP_STACKSIZE: stack-size allocated for each worker 16Mb per worker by default probably
- OMP_NUM_THREADS
- OMP_SCHEDULE
- OMP_WAIT_POLICY
	- Sleeping threads may be bad (passive), as cpu may move them to another core etc -> bad data access when waking up 
	- "active": spinlock (don't allow moving around threads)
- PROC_BIND [true|false|close|spread]
	- Controls binding of threads to cores 
	- true: worker thread will stay on core which it was started on (ie. cache access to data remains even when at sleep)
	- packed/spread: packed: neighbouring cores, spread: as distance as possible (multiple sockets, e.g.)



<b> Mandelbrot exercise </b>
- Create a version of the code that uses orphaning. What is the advantage (or  disadvantage) of using orphaning here?
	- By default all variables are shared. We can't make default(none), ie. there is larger chance of bugs 
	- We have to compile using openMP, thus adding more dependencies. 
	- 