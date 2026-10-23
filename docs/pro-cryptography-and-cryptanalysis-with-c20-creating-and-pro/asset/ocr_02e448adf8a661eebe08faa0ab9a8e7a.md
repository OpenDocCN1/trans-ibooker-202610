```
struct merkle_tree {
    tree_node* tree_root;
    merkle_tree(vector<tree_node*> vector_nodes);
    ~merkle_tree();
    void print_merkle_tree(tree_node *node, int index);
    void delete_merkle_tree(tree_node *node);
};

#endif
```

# Listing 15-4. The Content of the tree.cpp File

```
#include <iostream>
#include <iomanip>
#include "tree.h"

using namespace std;

merkle_tree::merkle_tree(vector<tree_node*> vector_nodes)
{
    vector<tree_node*> aux_nodes;
    while (vector_nodes.size() != 1)
    {
        print_hash_values(vector_nodes);
        for (int i = 0, n = 0; i < vector_nodes.size(); i = i + 2, n++) {
            if (i != vector_nodes.size() - 1) // check if there is a neighbour block
            {
                // merges the neighbour nodes and computes the hash value
                // of the new node
                aux_nodes.push_back(new tree_node(compute_sha256(vector_nodes[i]->hash_value + vector_nodes[i + 1]->hash_value)));
                // link the new node with the left neighbour and the right neighbour
                aux_nodes[n]->l_neighbour = vector_nodes[i];
                aux_nodes[n]->r_neighbour = vector_nodes[i + 1];
            } else
```