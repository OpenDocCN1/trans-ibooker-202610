Yep, we observe reads from chromosomes 1 through 22, X and Y, along with some other chromosomal chunks that are not part of the “main” chromosomes or whose locations are unknown. Let’s analyze the code a little more closely:

```
readsData = ac.loadAlignments("data/HG00103") ①
readsDataDF = readsData.toDF() ②
unique_chr = readsDataDF.select('referenceName').distinct(). \
collect() ④
```

+   1. AlignmentDataset: an ADAM type that contains all our data.
+   2. DataFrame: the underlying Spark DataFrame.
+   3. this will aggregate all the distinct contig names; will be small.
+   4. this triggers the computation and brings the data in the DataFrame back to the client app (the shell).

For a more clinical example, say we are testing an individual’s genome to check whether they carry any gene variants that put them at risk for having a child with cystic fibrosis (CF). Our genetic test uses next-generation DNA sequencing to generate reads from multiple relevant genes, such as the CFTR gene (whose mutations can cause CF). After running our data through our genotyping pipeline, we determine that the CFTR gene appears to have a premature stop codon that destroys its function. However, this mutation has never been reported before in HGMD, nor is it in the Sickkids CFTR database, which aggregates CF gene variants. We want to go back to the raw sequencing data to see if the potentially deleterious genotype call is a false positive. To do so, we need to manually analyze all the reads that map to that variant locus, say, chromosome 7 at 117149189 (see Figure 9-1):

```
cftr_reads = readsDataDF.where("referenceName" == "7").\
where(fun.col("start") <=
117149189).where(fun.col("end") > 117149189)
cftr_reads.count()
...
```