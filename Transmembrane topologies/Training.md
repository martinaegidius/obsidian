
- [x] Overfit a single sample: lr = 0.001, hn = 32, 30 epochs
- [ ] Overfit four samples: batch_sz = 2, lr = 0.001, hn = 64, 100 epochs, accuracy 98.2, last 5 points avg. loss 0.199
	- [x] Trying lr = 0.0001 for increased gradient stability -> became unstable 
	- [ ] Trying batch_sz = 1 as batch size 2 is a bit odd for so small dset  -> overfit possible, e-05 at epoch 75. dset acc 100%
- [ ] Increase size to 16, batch_sz = 4 - after 87 epochs accuracy 0.84, mean loss = 0.47. 
	- Note: execution very slow, need to run on HPC
- Added LSTM support, it seems bidirectional is superior. Slow convergence at current settings. 


#todo: Next step: fire up on HPC/colab to finetune for larger batches and trainingset sizes 

- [x] uploaded files to colab
- [x] Built pipeline support in colab
	- [x] Need cuda-tensor support, probably will throw an error


setup tracking pipe BUT found issue 
- [x] Proteins which have mismatches are not excluded from the dset 
- [ ] Trying to fix it currently on colab - run assure loop for all loaders when finished regenerating dset
- [x] Check that there are no overlaps of ids in loaders

To-do today: 
- [x] Fix above issues
- [ ] Figure out why we get loss spikes 
- [ ] Modularize the shit out of it and make it runnable on HPC 
- [ ] Increase overfitting set up to full set in increments of 400 or the likes. 
	- [x] Check how batch impacts it - in small overfitting experiments it fucked up 
- [x] Change accuracy metric


90 epochs are enough for overfitting 64 samples approximately. After 250 epochs, we have loss < 1/1000. 


HPC workflow: 
1. conda activate ...
2. batch-sub 
3. scp experiment results
