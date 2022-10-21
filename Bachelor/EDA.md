log 9/11: 
Loaded images. 
Plotting with eye-data seems to work fine. 
Test-case: 

number for "choice" = 314
f: sofa_2009_004180.jpg
fix = [fixL;fixR] =   
  439.8000  489.1000
  361.6000  372.7000
  297.2000  337.8000
  416.0000  504.3000
  321.4000  390.3000
  261.5000  348.5000

Seems to work fine. 
Green person (number 3) makes a bit funny looks. 

# Next step: 
Statistics 
Mean image 

todo: 
- Check if scaling affects measured eye-pos
	- Status: scaling does not affect measured eye-positions. Plots cross-compared to Dimitrios implementation, and plots are exactly the same. Scales have probably been fixed on the fly. 


Mean box on train set 
torch.mean(boxes,0)  
>>> tensor([0.1559, 0.2902, 0.8477, 0.8151])


Overfitting baseline-model converges towards constant bounding-box: 
[0.1764, 0.2950, 0.8254, 0.7864]

Average box on complete training set tensor([0.1559, 0.2902, 0.8477, 0.8151])
['aeroplane_2008_003575.jpg']
['sofa_2011_002359.jpg']
['sofa_2009_004983.jpg']
['aeroplane_2009_003066.jpg']
['sofa_2008_008029.jpg']
Average box first five sampels torch.manual.seed(1):  tensor([0.0821, 0.1908, 0.9772, 0.8919])


Fine idea, but data-loader is actually just going through dataset ie the break-statmement does not reset the generator. Need to fix this. 


