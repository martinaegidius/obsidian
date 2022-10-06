- D_model in transformer should probably be 2 (number of in-features in transformerencoderlayer)

- n_hidden is gonna be default, probably 2048 (and is the size of the output of the encoding layer!). It is a hyperparameter
- You must use positional embedding from the paired x-y-points 
- You should not mask the input (triangular). This would be interesting for predicting "next" fixation, but in your case you want to use all fixations for getting bounding box 
- You should use src_padding_mask for padding out 0's. Dimitrios sees the point with 0,0 being a problem - try -1 -1 instead for now. He says that there may be a problem when using ViT-features
- Start out with creating 2-3 encoding-layers and throwing a linear layer on top. 
- You MUST use Dimitrios' splits for comparing correctly to his baseline 
- In the image, your "embedding" will be the ViT-extracted featuremaps! Thus, no explicit embedding needed.