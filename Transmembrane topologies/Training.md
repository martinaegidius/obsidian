
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


- Debugged single accuracy: the denominator is correct also for batches
- Checking if colab output is the same as HPC for 40 epochs, batch sz 2, num_samples = 30 (train), with test and val subsets (0,2) and batch_sz = 2, shuffle=True
	- It is 

1. Trying to tune optimizer on one sample
	1. LSTMB: 1e-05 definitely seems too low 
	2. LSTMB 64 64 with 1e-04 was fine - but the sample was very easy
	3. Seed = 4 (protein: very simple)
2. Two samples
	1. LSTMB 64 64 with 1e-04 was fine, but both samples are simply I (seed=4)
	2. 
3. 8 samples
	1. LSTMB 512 512 1e-04 perfect
4. 512
	1. LSTMB 512 512 1e-04 was perfect after 300 epochs
5. Scaling up to 1024 600 epochs, batch_sz = 1 - was cancelled (error in is_topologies_equal), but seemed promising. Overlap acc 88% at epoch 90. 
	1. If that works go for 3072 
	2. Running the same with batch_size = 32, 600 epochs, to see if batching works
	3. REDOING 1024 600 epochs, batch_sz = 1 WITH SAME SEED TO SEE IF ERROR HAPPENS AGAIN, CHECK AT 17 O'CLOCK
		1. batchjob ID 19510976
		2. Happened again
		3. Hopefully fixed indexing for edge-case, set over same experiment again 
			1. 19513466 - this worked fine, acc 1 
		4. Setting over singleton overfit [2048,3072] for [400,600] epochs 
			1. 19513470
		5. Setting over batch-overfitting experiment for 1024, 2048 samples, batch_sz = 32
			1. 19513471
				1. Batches seem to fit way worse 
		6. Setting over batch-overfitting experiment for 1024, batch_sz = 16 for comparison
			1. 19513473

			Set up overfitting experiment for 2800 samples, 450 epochs
			

Valid seeds: 
1934
210253 (better)
72102537 (best, used at this moment)


HPC workflow: 
1. conda activate ...
2. batch-sub 
3. scp experiment results



#QUESTION 
You are getting this error: It happens rarely, but during each training. I find their code quite hard-to-read. I don't understand why it works for them (I presume it has something to do with the amino-acid sequence, there must be some biological argument for this, for it seems completely odd to go idx + 1)
Traceback (most recent call last):
  File "/zhome/ba/f/147212/transmembrane_project/transmembrane-topology/train.py", line 201, in <module>
    train_acc,train_acc_overlap = tmu.dataset_accuracy(model,trainloader,type2key)
  File "/zhome/ba/f/147212/transmembrane_project/transmembrane-topology/transmembraneUtils.py", line 158, in dataset_accuracy
    match_tmp = is_topologies_equal(preds_top,label_top)
  File "/zhome/ba/f/147212/transmembrane_project/transmembrane-topology/transmembraneUtils.py", line 234, in is_topologies_equal
    overlap_segment_end = min(topology_a[idx + 1][0], topology_b[idx + 1][0])
IndexError: list index out of range


- Possibly two reasons: 
	- 1. is_topologies_equal needs (label,pred) but you gave(pred,label) in dataset_accuracy (their code is hard to read) -> fixed this, checking if it is an issue again, I think this may fix it
	- Else: 
		- Need to introduce that it does not index idx+1, as this of course is not in list (possibly they have added zero-padding)
		- [ x ] Implemented, let it run overnight, let's see  



<h1>possible tips</h1>
Gradient clipping - see if we measure explosions first
Proteinworkshop look at pretraining-trick, they offer weights for pretrained models maybe.
Get performance for the prototype split using their model 
Metrics: 
- AUC
- ROC 
- AUPRC


Added in batch-support with linearly scaled learning rate. Convergence is much better now for batch-size e.g. 16. But ran it for 32, and it seems gradients died??? Tried scaling with sqrt(k) instead for batches :) -> current experiment



#TODO add the accuracy results to the training-script for quicker comparisons
Currently it is working fine for type prediction, but it is having a really hard time getting the overlap accuracy to be good! How can we solve this? 
- Trying with more features, as given in https://openreview.net/pdf?id=sTYuRVrdK3 the "INTERMEDIATE" type should work better (active)
- Trying to download pre-trained weights for the inverse-folding task to see if that helps (need to see how these weights translate to SchNet)
- Using dimenet instead should be easy - same batch-attributes, consider this if above still doesn't work 