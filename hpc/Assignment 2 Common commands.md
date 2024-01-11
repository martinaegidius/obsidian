md5sum binary_1 binary_2 - have to be equal 
cmp binary_1 binary_2 -> no output = identical


input args: grid-size max_iter tolerance start_t output type
- Output type 0: nothing
- Output type 3: binary
	- Naming: poisson_res_gauss_gridSZ.bin and poisson_res_jacobi_gridSZ.bin
- Output type 4: vtk

run jacobi: 
./poisson_j 23 10000 1e-8 5 3

run gauss-seidel
./poisson_gs 23 10000 1e-8 5 3
