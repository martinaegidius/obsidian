
<h1><b>Dataprep</b></h1>
<b>DataLoaders</b>
batch_size for dataloaders: 
	same as chunking data. Feedthrough ie. 10 items (batch_size=10) at each time. Common sizes = [8,60]
<br>
	Batching prevents overfitting, as it forces machine to learn generalizable weights
<br>
	Shuffle shuffles data randomly. e.g. MNIST-data: sorted after label. Thus, static-learning when not shuffling. Would first learn "everything is 1", followed by "everything is 2" (overfitting by sequence)
<br>
<b>Balanced datasets</b>
In unbalanced datasets, models would favour weights which too often predict over-represented class, not related to input-data