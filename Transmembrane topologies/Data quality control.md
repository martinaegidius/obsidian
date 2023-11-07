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

