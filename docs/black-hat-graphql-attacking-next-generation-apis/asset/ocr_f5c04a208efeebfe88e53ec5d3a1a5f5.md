argument tells Clairvoyance where it should output the schema it generates during runtime:

```
# cd ~/clairvoyance
# python3 -m clairvoyance http://localhost:5013/graphql -w ~/high-frequency-vocabulary/30k.txt -o clairvoyance-dvga-schema.json
```

Depending on the size of the wordlist, Clairvoyance may take a few minutes to finish executing. Upon completion, you should see a new file in the clairvoyance directory called clairvoyance-dvga-schema.json.

Let’s test the efficiency of our wordlist by comparing the schema Clairvoyance gave us with the schema generated from an introspection query. To best represent these differences, we can leverage GraphQL Voyager, located at http://lab.blackhatgraphql.com:9000 or https://ivangoncharov.github.io/graphql-voyager, and upload both schemas. Figure 6-1 shows the DVGA’s schema, and Figure 6-2 shows the reconstruction of the schema by Clairvoyance.

As you can see, Clairvoyance was able to recover almost every field and operation of the DVGA schema! For an application that doesn’t have introspection enabled, this isn’t half bad.

Another good option is to generate our own wordlists. As mentioned, tools like Clairvoyance are only as strong as the wordlists we provide them. We can add to our list by making informed guesses, or by extracting keywords from HTTP traffic, static files, and other resources collected during the information-gathering phase.

![](bbox=[0.1, 0.52, 0.92, 0.85])

Figure 6-1: The original DVGA schema