
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


Training 1000 EPOCHS, beta = 0.33, lr = 0.001
Filename: ['aeroplane_2011_001858.jpg']
 Target: tensor([[0.0360, 0.2682, 0.9960, 0.8697]], dtype=torch.float64)
 Prediction: tensor([[0.0799, 0.1534, 1.0179, 0.8485]])
 Loss: 0.006071662602999837

IOU:  [tensor([[0.7619]], dtype=torch.float64)]
Filename: ['aeroplane_2008_001719.jpg']
 Target: tensor([[0.0980, 0.0871, 0.7520, 0.9129]], dtype=torch.float64)
 Prediction: tensor([[0.0872, 0.0813, 0.7643, 0.9089]])
 Loss: 0.00012109170858366993

IOU:  [tensor([[0.9545]], dtype=torch.float64)]
Filename: ['aeroplane_2008_000585.jpg']
 Target: tensor([[0.0020, 0.0133, 0.9980, 1.0000]], dtype=torch.float64)
 Prediction: tensor([[-0.0081,  0.1126,  0.9598,  1.0083]])
 Loss: 0.0043525304183497995

IOU:  [tensor([[0.8502]], dtype=torch.float64)]
Filename: ['aeroplane_2010_004557.jpg']
 Target: tensor([[0.3080, 0.1682, 0.9860, 0.5015]], dtype=torch.float64)
 Prediction: tensor([[0.3175, 0.2065, 0.9796, 0.4726]])
 Loss: 0.000922767882294459

IOU:  [tensor([[0.7795]], dtype=torch.float64)]
Overfitting evaluation finished. 
Transformer accuracy with PASCAL-criterium on overfit set: 4/4, percentage: 1.0
Mean model accuracy with PASCAL-criterium on overfit set: 4/4, percentage: 1.0
L1-loss on every over batch 0:100: 0.05900789591704009

L1-loss on every over batch 100:200: 0.06858376114569496

L1-loss on every over batch 200:300: 0.0655124392801061

L1-loss on every over batch 300:400: 0.06683054435130882

L1-loss on every over batch 400:500: 0.05915770704035768

L1-loss on every over batch 500:600: 0.05317119672962023

Testing finished. 
Transformer accuracy with PASCAL-criterium: 270/619, percentage: 0.43618739903069464
Mean model accuracy with PASCAL-criterium: 344/619, percentage: 0.555735056542811
Median model accuracy with PASCAL-criterium: 288/619, percentage: 0.46526655896607433

![[Pasted image 20221031112315.png]]


1000 epochs, 4 img train: 
![[Pasted image 20221101092259.png]]

10000 epochs, 12 img train: 
![[Pasted image 20221101094526.png]]

Definitely, model over time learns to differ from mean and median


Ideas for better results: 
- Make minibatch possible
- Add-in some dropout
- Try different number of encoder-layers paired with different learning-rates
- hidden-dimension in encoder-layer should be tuned.
- Beta has influence on gradient-calculation. Should be tuned aswell
- Plan: run model on same split 3 times, with three different hidden-dimensions


Overfit DSET, EPOCHS = 986, LR = 0.001, NLAYERS = 1, dropout everywhere = 0.1, hidden_dim = 2048

![[Pasted image 20221101163359.png]]

Results: 
Transformer accuracy with PASCAL-criterium: 309/619, percentage: 0.4991922455573506
Mean model accuracy with PASCAL-criterium: 344/619, percentage: 0.555735056542811
Median model accuracy with PASCAL-criterium: 288/619, percentage: 0.46526655896607433

End-performance: 
complete over-epoch val loss:  0.043005528013982636
epoch loss 0.008010193739179243
epoch val loss 0.043005528013982636
epoch val acc 0.09090909090909091

for recreating plot: 
![[train_loss_987_epochs_val_25.pt]]

![[valloss_987_epochs_val_25.pt]]


Something weird is wrong: 
- Your model gives different performance during testing and evaluation even though it is the same datasample 
- Pitfall: maybe pascalACC and getIOU dont behave the same? 
	- pascalACC used in eval 
	- getIOU used in train 
- Or: something is wrong with the eval/training-loop. Losses should definitely be the same. Maybe one of them is a mean over something? 
	- Check tomorrow 
- Seems not to be a problem without dropout and with higher LR. 


Prøv faktor ti mindre LR 
Overvej schedule for LR 
Prøv at fjern attention og se om du kan overfitte igennem de lineære FFN 
	- Ekstra lag til at "ignorere" pad

Hvis ikke det ender med at virke på 6: 
- gå tilbage til 4, slut færdig 
- Skriv metodeafsnit til denne her dreng
	- Ca. 3 dage [all inclusive]

laeroplane_2010_002460
aeroplane_2011_000276
aeroplane_2008_008507



CHECK: 
- IOU 
-  Maybe implement self 
- ablation study - nheads, nlayers, hidden_dim
- Class-agnostic vs. class-specific ablation study 


Dim has a baseline of appx 33% on all classes 
If time fix multiple bboxes 


Debugging IOU
- Seems correct. 
- 