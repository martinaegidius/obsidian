Status: untill now best results have been obtained using 2 LSTM layers. 
It seems that performance is dependent on batch-size. 

Topology metrics: 

| Batchsz      | TM ACC | SPTM acc| beta acc |
| ----------- | ----------- | ----------- | ----------- |
| 32      | +  | + | + |
| 64   |  -        | - | - |

Type metrics: in general a bit better using batch 64

Best experiments in last run: format GCSize x LSTMS x num layers
Batch size 32: good-grass-15, 128x128, 2 layers
Batch size 64: stoic-frog-20, 128 x 512, 2 layers

Lowest val loss: 
stoic-frog 
best general test metrics:
stoic-frog 


Diagnostics: it seems they may have underfitted a bit. There is at least no indication of the val-loss increasing at any point -> run for longer. Added experiment with more LSTM layers, smaller GCSize, and dropout for longer epochs. This will remake results, but also add in additional cases with more lstm-layers

Experimenting with more layers and dropout. In general, more layers make the gradients smaller. They still show the spikes they did earlier. 

#TODO If 4 layers perform worse than 2, use 2. Then try training across a subset of different learning rates, weight-decays and dropout.


Best ones with layers = 1, 2 are currently 

devoted-butterfly-24: 
epochs: 80
Batch: 64
GCS: 512
LSTM: 128, 2 layers
dropout = 0.0

good-durian-26: 
epochs: 80 
Batch: 64 
GCS 512
LSTM: 512, 1 layer
dropout = 0.0


logical-bird-30: started overfitting after 80 it seems
epochs: 
Batch: 32
GCS: 64 
LSTM: 128, 


Looking at curves, logical-bird is the better one.

Waiting for results for 4 layers.
Untill now 4 layers seem promising, but we loose some SP accuracy. 


Diagnostics
Batch:
We choose batch size 32 - it seems that larger batch-sizez favour globular accuracy and lower tm accuracies (probably due to the inbalanced dataset, ie. fewer tm samples in each batch -> TM gradients become diluted)
Dropout:
There may be slight indication that dropout is a good idea if we control validation more often. But overfitting is not too big of an issue - sequences are quite similar in train and test-split.

Gradients: seem on average more well-behaved using smaller LSTM size, GCs64 LSTM128

Following configurations show promise both regarding val loss and val overlap
GCSize: 128, LSTMSize: 512 (was stopped early but learned well, already within 100 epochs comparable val loss and accuracy, lower overlap accuracy)
GCSize: 64, LSTMSize: 512 (winner after 150 epochs)	
GCSize: 64, LSTMSize: 128 (second place)


Num-layers: seems that likely-brook is best on all performance metrics of interest. 
	Ie. 64x128x4, batch_sz = 32 



<h1> Conclusion: we choose likely-brook-33 architecture </h1>

