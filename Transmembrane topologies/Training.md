
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
	- [ ] Need cuda-tensor support, probably will throw an error