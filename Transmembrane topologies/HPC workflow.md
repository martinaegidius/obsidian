<h1>Sending files to blackhole</h1>
$BLACKHOLE  = /dtu/blackhole/0c/147212
rsync -av your_folder s194119@transfer.gbar.dtu.dk:/dtu/blackhole/0c/147212

<h1> Receiving new log-files </h1>
rsync -av s194119@transfer.gbar.dtu.dk:/dtu/blackhole/0c/147212/ExperimentalResults /home/max/Documents/s194119/fall2023/deep_learning_project/Results

implemented in fetchResults.sh in deep_learning_project

<h1> Paths </h1>
Data-directory: /dtu/blackhole/0c/147212/data/graphein_downloads/{processed,raw}/


<h1> Environment </h1>
module load python3/3.10.12
source transmembrane/bin/activate



<h1> Resource inspection </h1>
https://www.hpc.dtu.dk/?page_id=4976


