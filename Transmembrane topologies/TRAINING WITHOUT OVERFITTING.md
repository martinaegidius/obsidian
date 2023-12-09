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



