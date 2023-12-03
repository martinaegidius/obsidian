
#question regarding task:
It seems depending on the batch-size the network learns to favour globular/beta prediction. Ie. larger batch-size: better globular and beta accuracy, vs. smaller batch-size: better TM quality.


My understanding is that in general we are most interested in predicting whether proteins are TM and predicting their topology, signal peptide is just a nice addition they found. Thus, I reckon that models which perform well on the TM task are to be preferred. Is this completely misunderstanding the assignment? There are only 62 samples of SP+TM in the training set, and I presume that a lower accuracy on this is expected. If we look at the distribution of residues, it seems expected that it will be biased to predicting substituting S with I: 
![[Pasted image 20231130121357.png]]

#answer 
- Balanced sampling. Maybe mentioned supplementary material. 
- Check how many samples contain B, and how many samples contain P. Check if the number is the same.



#question I've added in early stopping now with validation-loss criterion, but it seems to actually worsen performance. My thoughts on two reasons for this: 
1. My thought is that the splits have been quite nicely made and the nature of the data is quite "similar", ie. we should not fear overfitting too much, and just prefer long training. 
2. Validation loss does not translate directly to the task

#question When changing the protein-type-from-label function from using P in the sequence to using B in the sequence for classifying beta, the results change marginally (1% bit better on pure SP task for both topology and label). I noticed your other student uses P to classify as beta-barrel. As far as I remember, they should be equivalent? My understanding is that using B is most correct.  



#question My model is performing all-rightish, compared to their results, depending on the task we focus on: https://docs.google.com/spreadsheets/d/1b6nPzIgEzaFJERisLXt54tnC86n-jDUAldvwCDwXSyk/edit#gid=0
![[Pasted image 20231130110510.png]]

They are notably better at SP+TM, whereas we are better at TM and beta. 
What I'm wondering is if I should spend the last time:
- trying to make it better on all metrics
- comparing on more metrics (I save the SOV mentioned in their supplementary paper, but can't readily read which threshold they used). Currently I have ROC-curves, AUROC-scores and accuracies, per-residue confusion-matrices, per-type confusion-matrices. 
- Fitting some sort of baseline model on simply the amino-acid sequence for comparison, but I think their baseline should suffice 
- Stop doing more and focus on my exam on monday ;) 



#conclusion 
You are doing well. 
- You can see why it is performing badly on TM + SP by taking per-residue labels and predictions for only this class and looking at the confusion. Probably the S is somehow substituted to O or P. You should have the data in the prediction-dictionary
- Try a balanced sampler first to ensure uniformness of batches. This should help alleviate the issue. 
- Going into structure space seems to hold so much information that overfitting is not an issue. Plenty of data to fit the model, and the task becomes really easy for the decoder stem -> Don't worry about overfitting. If you really want to early stop, you should do that based on accuracies more than on validation loss, as it is an imperfect metric
- Ensure that number of P is equal to number of B  <- IT IS NOT. B is the only predictor for B barrel. P is no predictor at all. You were correct. 


Status: 
- Efforts with weighted sampling: good, but now we get worse performance on TM. Upscale the sampling rate for TM-proteins and redo experiment. Keep batch-size 32. Dropout does not affect performance beneficially. Consider even removing weight-decay