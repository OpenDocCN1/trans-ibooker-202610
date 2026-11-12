```
mismatchingPositions | null
originalQualityScores | null
readGroupId | SRR062640
readGroupSampleId | HG00103
mateAlignmentStart | 21810734
mateReferenceName | hs37d5
insertSize | null
readInFragment | 1
attributes | RG:Z:SRR062640\tX...
only showing top 1 row
```

You may get a different read, because the partitioning of the data may be different on your system, so there is no guarantee which read will come back first.

Now we can interactively ask questions about our data set, all while executing the computations across a cluster in the background. How many reads do we have in this data set?

```
readsData.toDF().count()
...
160397565
```

Do the reads in this data set derive from all human chromosomes?

```
unique_chr =
readsDataDF.select('referenceName').distinct().collect()
unique_chr = [u.referenceName for u in unique_chr].sort()
unique_chr
...
1
10
11
12
[...]
GL000249.1
MT
NC_007605
X
Y
hs37d5
```