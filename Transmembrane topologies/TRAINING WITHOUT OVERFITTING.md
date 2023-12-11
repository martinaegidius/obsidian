All are run for 160 epochs

Best results are: 
- Smooth-serentiy-15
- Worthy-sky-8
- Colorful-haze-21

- All have have weight-decay 
- GCSize 64, LSTMS 64
- In general, 2 layers perform better than 1. Still waiting to see if 4 layers perform better than 2 
- Batch-size is 32 or 64 
- Dropout is 0, for smooth-serenity-15 it is 0.1
	- Overall dropout 0 performs better!
	- LSTM size 64 seems actually more promising than 128 atm, but only three samples in LSTMsize 128 so wait and see
	- Have only run GCSize 64 so far, still waiting for others.

Awaiting results for GCSize>64, LSTMSize>64



Best results: 
batch-size 32
LSTMlayers = 2 or 4
- Fewer layers give better TM, but a worse over-all profile

Testing GCSize 64 vs 128, dropout vs no dropout and 2 vs 4 layers. 

<h1> Current status </h1>
clear-thunder-14, colorful-haze-21 and smooth-serenity-15 so far best
Waiting to see if anything better comes up 

>writing report while doing so 



BEST RESPLIT CONCLUSION: 
- GCSize 64 for all 
- LSTMS 128 seems to increase TM but lower classification in some cases.
	- 4 layers -> better classification, marginally lower tm acc, lower beta.
	- Let's use 128 with 4 layers, same architecture we converged towards last time. 
- Weight-decay only helps on TM / TMSP. Use decay = 0
- Dropout really helps SPTM, beta, overall type and beta. Minor impact on TM acc - keep dropout. 
- OVERFITTING SEEMS NOT TO BE AN ISSUE

>Running with weighted configurations of dset
>	Honest-glade-1: generally decent results, need more beta-performance
>	royal-sound: was good at 120 epochs, seems to fuck up last epochs
>	- It seems we are overfitting. Consider adding in weight-decay, early-stopping, and lower the number of epochs. 
>	
>	
>	Depending on results, try adding label-smoothing or class-penalties
>	




It seems grads are still a bit unstable. In addition, it seems longer training increases tm acc way more. In general it seems good to upsample tm, sptm learns from that. Best results were 1.5,1,1,1,1,1 or 2,1,1,1,1. We check these with different degrees of label-smoothing, batch-size and number of layers. (runs 11/12 after 09:50)


