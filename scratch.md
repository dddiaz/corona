
Search DB
```python
Entrez.email = "<your-email-here>@gmail.com"  # Always tell NCBI who you are
search_term = "SARS-CoV2[orgn] AND complete genome[title]"
handle = Entrez.esearch(db="nucleotide", term=search_term)
search_results = Entrez.read(handle)
genome_ids = search_results['IdList']
```

Download Ref Genome
```python
######################################
# Download Ref Genome
######################################
Entrez.email = "daniel.delvin.diaz+ncbi@gmail.com"  # Always tell NCBI who you are
search_term = "NC_045512[locus] AND complete genome[title]"
handle = Entrez.esearch(db="nucleotide", term=search_term)
search_results = Entrez.read(handle)
ref_genome_id = search_results['IdList'][0]
record = Entrez.efetch(db="nucleotide", id=ref_genome_id, rettype="gb", retmode="text")
# print(record.read())
filename = f'{os.path.abspath(".")}/ref_genome/genBankRecord_ref.gb'
with open(filename, 'w') as f:
    content = record.read()
    # print(content)
    f.write(content)
print('File Written:{}'.format(filename))
# Note I have noticed a weird behavior with pycharm + jupyter notebook where you wont see the
# file locally unless you click out of pycharm then back in.
```


```python

def get_one_hot_genome_encoding(gb_file_path):
    """
    :arg gb_file_path: Load a GB file
    Get the genome from that file
    One hot encode that genome
    :returns A one hot encode genome
    """
    for seq_record in SeqIO.parse(gb_file_path, "genbank"):
        # print(f"SARS-CoV-2 Genome: {seq_record.seq}")
        genome_seq = str(seq_record.seq)
        # Lets assume theres one genome per file
        seq_hot = one_hot_encoder(string_to_array(genome_seq))
        return seq_hot
```