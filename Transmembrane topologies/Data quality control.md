Using version = 2 (default arg), we still missed 59 entries. 

Thus, trying with version 4 instead, we see how many are missing. We still need to check the sequence 
#todo Status: still downloading, check when finished

Using version 4 we only miss 32 proteins. Better.

Try finding rest using other versions -> they dont exist. Saved missing proteins in graphein_data/train/missing


#done simply save list of bad entries, and work on without these


#done Parse pdb-file aminoacid sequence using biopython, check that residue-sequences align

#done saved quality-control dictionary to scratch. Three proteins have different sequences. 

#todo Create graph dataset
#todo create pytorch dataloader 
#todo inspect graphein.protein.graphs.read_pdb_to_dataframe

#todo inspect graphein.protein.utils.save_graph_to_pdb


#question Should I generate AF predictions for the last 32 proteins? I think I know how to do it. 

**Felix message**: 
*i.e. what I would do using vanilla pytorch is.  provided*
- [x] 1.) get all the alphafold pdb files downloaded in a directory  
- [x] 2.) write a torch.utils.data.Dataset class that consumes the train data json file + the pdb file directory,  +
- [ ] and returns protein graphs + labels (this should leverage graphein and be written in a way that it's compatible with proteins.sh encoders)  
- [ ] 3.) Define a nn.module prediction model that takes protein graphs as input and returns topology label sequences as output. the encoder would be a submodule of that nn.module  
- [ ] 4.) Write a train loop that uses your dataset + the module + the metrics that are provided




How they generate dsets in proteins.sh: 
https://proteins.sh/_modules/proteinworkshop/datasets/base.html#ProteinDataModule.train_dataset
ctrl + f: def process(self):
they use protein_to_pyg and save tensors 

from graphein.protein.tensor.io import protein_to_pyg


Return protein graphs and labels 
https://colab.research.google.com/github/a-r-j/graphein/blob/master/notebooks/protein_tensors.ipynb
~~~
data = gpt.io.protein_to_pyg(pdb_code="4hhb", # Can alternatively pass a path or a uniprot ID (for AF2) with path=... and uniprot_id=...
chain_selection=["A", "B", "C", "D"], # Select all 4 chains
deprotonate=True, # Deprotonate the structure
keep_insertions=False, # Remove insertions
keep_hets=[], # Remove HETATMs
model_index=1, # Select the first model
# Can select a subset of atoms with atom_types=...
)
~~~

Inspecting gearnet:  https://github.com/a-r-j/ProteinWorkshop/blob/main/proteinworkshop/models/graph_encoders/gear_net.py
`    def required_batch_attributes(self) -> Set[str]:
        - ``x`` Positions (shape ``[num_nodes, 3]``)
        - ``edge_index`` Edge indices (shape ``[2, num_edges]``)
        - ``edge_type`` Edge types (shape ``[num_edges]``)
    GearNet    - ``edge_attr`` Edge attributes (shape ``[num_edges, num_edge_features]``)
        - ``num_nodes`` Number of nodes (int)
        - ``batch`` Batch indices (shape ``[num_nodes]``)

        :return: Set of required batch attributes.
        :rtype: Set[str]`

So we need to:
1. install Pytorch geometric - demo: https://colab.research.google.com/drive/17VFsRZeZa7rgtrQ39-RMiYIE0ePrSlgN?authuser=1
2. Test how graphein.protein.tensor.io.protein_to_pyg works (can't find docs)
	1. When we have it: try to use protein_to_pyg
		1. Check what it returns - do we get the features which graphein module needs? 

#question: I want to use gearnet. I am having a really hard time figuring out how the data should be formatted using graphein as the documentation is so sparse. 
#question Should we generate predictions for missing/wrongly sequenced proteins? 



#meetingNotes:
- Graphein and protein-workshop is written by same guy. So ProteinBatch should be the same as in Graphein ("Why would he make life harder for himself")
- You were right; get_edge_features accesses edges of a graph. Therefore, at some point beforehand a featurization-scheme has been called beforehand
- All models are a type of BenchmarkModel - that is the idea behind protein-workshop, as it is so modular: https://github.com/a-r-j/ProteinWorkshop/blob/main/proteinworkshop/models/base.py. It can be accessed in models/base.py
	- Here there is a function featuriser, and a class BenchmarkModel of which GearNet will be a subtype 
- Felix idea: download the package, use a downstream task on a pretrained model, his suggestion: try with the following configuration
	- https://proteins.sh/quickstart_component/downstream.html
![[Screenshot from 2023-11-09 13-28-37.png]]
Use breakpoints/hooks to figure out at which point the data looks in which way to figure out the feature-scheme

- Note on missing data: absolutely no problem, just exclude them 
- Note about calculating edge features during each forward: absolutely no problem, is quite cheap computationally compared to maintaining modularity when using different edge-features etc.
- Regarding HPC: use it when you have a script etc., but beware that geometric can be a pain in the ass - segmentation faults etc. 
	- Felix instantiated a new conda environment, installed in that environment (even though they tell you not to ;) ) 
#todo: install protein-workshop pip install proteinworkshop --no-cache-dir. Beware installation instructions are built for linux nvidia cuda, so you probably need to run with GFX enabled. 



- it seems it works now with SchNet
	- I will proceed with this, just to get something done
- [x] Need to make a pipe which reads a protein from pdb, throws to protein-tensor and followingly can be converted to a Proteinbatch() with neccessary batch-attributes
- [x] Fix that process shouldn't be called for already processed proteins
- [ ] Read up on graph encoder/decoder 
- [ ] Decoders: multiclass node classification present in https://proteins.sh/configs/task.html 
- [x] Begin installing environment on HPC 
- [ ] Check that environment works 


#question When using batches, as far as I understand torch geometric treats a batch as a large disconnected graph, ref: https://pytorch-geometric.readthedocs.io/en/latest/generated/torch_geometric.data.Batch.html
- How should I understand this when decoding?

#meeting-notes
- Linear decoder is no issue. Remember vector-product
- See if you can fit using a simple linear decoder
- Need larger hidden dimension - impossible to infer 5 values from 1 value
- When running train / val / test splits, just use the val for early stopping - other params stay constant from prototype training split 


- [x] Add Batch-functionality, and train on a small batched subsample (16) 
	- [x] Issues arose due to the lazy layers of the SchNet model. Before, output was biased toward classes 0 and 5, which is highly improbable at random. Added xavier_init for dense decoder and forwarded an example batch through encoder-part to initialize the weights. Still, they are very biased towards 5 for some reason.  
	- [x] Fixed by using xavier normal init

- [x] Overfit a single sample: lr = 0.001, hn = 32, 30 epochs
- [ ] Overfit four samples: batch_sz = 2, lr = 0.001, hn = 64, 100 epochs, accuracy 98.2, last 5 points avg. loss 0.199
	- [x] Trying lr = 0.0001 for increased gradient stability -> became unstable 
	- [ ] Trying batch_sz = 1 as batch size 2 is a bit odd for so small dset  -> overfit possible, e-05 at epoch 75. dset acc 100%
- [ ] Increase size to 16, batch_sz = 4 - after 87 epochs accuracy 0.84, mean loss = 0.47. 
	- Note: execution very slow, need to run on HPC
- Added LSTM support, it seems bidirectional is superior. Slow convergence at current settings. 


#todo: Next step: fire up on HPC/colab to finetune for larger batches and trainingset sizes 