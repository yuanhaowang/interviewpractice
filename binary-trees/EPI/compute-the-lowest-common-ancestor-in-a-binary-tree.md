#### Compute the Lowest Common Ancestor in a Binary Tree

> Design an algorithm for computing the LCA of two nodes in a binary tree in which nodes do not have a parent field.

##### Code:

```py
def lca(tree, node0, node1):
    Status = collections.namedtuple('Status', ('num_target_nodes', 'ancestor'))

    # Returns an object consisting of an int and a node. The int field is 0, 1,
    # or 2 depending on how many of {node0, node1} are present in tree.
    # If both are present in tree, when ancestor is assigned to a non-null value, it is the lca

    def lca_helper(tree, node0, node1):
        if not tree:
            return Status(0, None)

        left_result = lca_helper(tree.left, node0, node1)
        if left_result.num_target_nodes == 2:
            # Found both nodes in left subtree
            return left_result
        right_result = lca_helper(tree.right, node0, node1)
        if right_result.num_target_nodes == 2:
            # Found both nodes in right subtree
            return right_result
        num_target_nodes = (left_result.num_target_nodes + right_result.num_target_nodes + (node0, node1).count(tree))
        return Status(num_target_nodes, tree)

    return lca_helper(tree, node0, node1).ancestor
```

##### Explanation:

A brute-force approach is to see if the nodes are in different immediate subtrees of the root, or if one of the nodes is the root. In this case, the root must be the LCA. If both nodes are in the left subtree of the root or the right subtree of the root, we recurse on that subtree. In the case of a skewed tree with two nodes at the bottom, our running time is $\small \mathcal O(n^{2})$

The trick to this problem is to put a bit more effort in the information we return when searching. Instead of simply returning a True/False indicating whether we found a node, we can return a Status item telling us exactly how many nodes we found, and what the possible ancestor root is. If we have found both nodes in either the left subtree or the right subtree, just return that immediately. Otherwise, check if the current root is either node and bubble up the result. The time and space complexity are now equivalent to that of a postorder traversal, $\small \mathcal O(n)$ and $\small \mathcal O(h)$, respectively. 

