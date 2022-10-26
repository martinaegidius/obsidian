
<h1>Overfitting phase:</h1>
<b>One layer, one attention-head. Hidden_dim = 128 </b> 
Settings for perfect overfitting on single-sample:
torch.manual.seed(2): signal length: 2 fixations
Participant: [1]
LR = 0.01
Hidden_dim = 128 
Dropout = 0
Positional encoding dropout = 0 
beta = 1.0
noOfEpochs: 300
Gradient clipping = 0.5


Number of parameters in model: 688
![[Pasted image 20221021102636.png]]


Result:
IOU = 1.0 

<b>One layer, one attention-head. Hidden_dim = 2048 </b>
Settings for perfect overfitting on single-sample:
torch.manual.seed(2): signal length: 2 fixations
Participant: [1]
LR = 0.01
Hidden_dim = 2048
Dropout = 0
Positional encoding dropout = 0 
beta = 1.0
noOfEpochs: 250
Gradient clipping = 0.5 (tried without - no influence)


Number of parameters in model: 10288
![[Pasted image 20221021103340.png]]

Result:
IOU = 0.999



Q for prof.: 
- Would you advice me to use validation set? 
	- training-set only consists of 80 samples 

A: 
Compare geometric model to figure 6 in POET paper (green part) (position + time)
try embedding: 32 or 46 for nn.embed

Q: validation set?
For now: 
use test-error 
- later on it would be nice with validation

For debugging positional-encoding: try removing it for one experiment. It should work worse, but not horribly. Thus, you can see if dimensionality breaks the model 
- Performance appx
- the same (almost no difference) 
![[Pasted image 20221024085326.png]]
^ no pos encoding

Checking all dimensions of model: 
- 

Plan: 
- train one model per class. If outputs are same for any input still: 
	- Run through dimensionality of models (batch first)
	- Check that norm is calculated correctly in layer norm layers
	- Check that relu is actually implemented correctly
	- Try multiple participants 
	- Try nn.embed (32 or 46)
		- It is a bit perverse to use two cls-tokens for 4 values 

- Move on to ViT-extraction 




I think your layernorm is wrong. 
Solution: 
BEFORE YOU DO THIS: https://www.youtube.com/watch?v=WpEE26clmqA watch with focus. 
Right now you have nn.LayerNorm((2,)), which seems to not layernorm but feature-norm
https://wandb.ai/wandb_fc/LayerNorm/reports/Layer-Normalization-in-Pytorch-With-Examples---VmlldzoxMjk5MTk1
- Dont use transformerencoderlayer 
- Instead build multiheadattention and define FFN yourself as two functions 
- Build a forward-pass through these, where you yourself define the layernorm

