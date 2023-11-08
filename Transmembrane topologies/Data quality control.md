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
- [ ] 2.) write a torch.utils.data.Dataset class that consumes the train data json file + the pdb file directory, and returns protein graphs + labels (this should leverage graphein and be written in a way that it's compatible with proteins.sh encoders)  
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
        - ``edge_attr`` Edge attributes (shape ``[num_edges, num_edge_features]``)
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