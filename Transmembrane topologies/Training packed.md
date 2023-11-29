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
