# Print Names of all Tensors

In order to inspect a tensorflow graph, it is useful to know the names of the tensors in the graph. 

```python
    [(op.name, op.values()) for op in sess.graph.get_operations()]
```

