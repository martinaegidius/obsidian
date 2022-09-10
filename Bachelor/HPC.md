General ipynb-procedure
https://topfarm.pages.windenergy.dtu.dk/cuttingedge/pywake/pywake_ellipsys/master/exercises.html

1. connect through shell
2. module load python3/3.10.2
3. source bachelorenv/bin/activate
4. go to interactive node (voltash, sxm2sh, a100sh)
5. https://topfarm.pages.windenergy.dtu.dk/cuttingedge/pywake/pywake_ellipsys/master/exercises.html
6. Go: jupyter notebook --no-browser --port=40000 --ip=$HOSTNAME
7. open new terminal and 
8. ssh <user>@l1.hpc.dtu.dk -g -L8000:<hostname>:40000 -N
9. Open manjaro browser and go http://localhost:8000/
10. Enter token from jupyter output 

hostname is login-node user 
