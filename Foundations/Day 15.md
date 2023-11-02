DFSForest is DFS implemented to multiple source nodes/nodes that do not follow from the first node chosen.

You iterate over all the children on the selected node and when the node no longer has any descendants left unchecked then it is pushed to the stack.

