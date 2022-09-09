# Tools Visualize

[_Documentation generated by Documatic_](https://www.documatic.com)

<!---Documatic-section-Codebase Structure-start--->
## Codebase Structure

<!---Documatic-block-system_architecture-start--->
```mermaid
None
```
<!---Documatic-block-system_architecture-end--->

# #
<!---Documatic-section-Codebase Structure-end--->

<!---Documatic-section-tools.visualize.make_dot-start--->
## [tools.visualize.make_dot](4-tools_visualize.md#tools.visualize.make_dot)

<!---Documatic-section-make_dot-start--->
<!---Documatic-block-tools.visualize.make_dot-start--->
<details>
	<summary><code>tools.visualize.make_dot</code> code snippet</summary>

```python
def make_dot(var, params=None):
    if params is not None:
        assert all((isinstance(p, Variable) for p in params.values()))
        param_map = {id(v): k for (k, v) in params.items()}
    node_attr = dict(style='filled', shape='box', align='left', fontsize='12', ranksep='0.1', height='0.2')
    dot = Digraph(node_attr=node_attr, graph_attr=dict(size='12,12'))
    seen = set()

    def size_to_str(size):
        return '(' + ', '.join(['%d' % v for v in size]) + ')'

    def add_nodes(var):
        if var not in seen:
            if torch.is_tensor(var):
                dot.node(str(id(var)), size_to_str(var.size()), fillcolor='orange')
            elif hasattr(var, 'variable'):
                u = var.variable
                name = param_map[id(u)] if params is not None else ''
                node_name = '%s\n %s' % (name, size_to_str(u.size()))
                dot.node(str(id(var)), node_name, fillcolor='lightblue')
            else:
                dot.node(str(id(var)), str(type(var).__name__))
            seen.add(var)
            if hasattr(var, 'next_functions'):
                for u in var.next_functions:
                    if u[0] is not None:
                        dot.edge(str(id(u[0])), str(id(var)))
                        add_nodes(u[0])
            if hasattr(var, 'saved_tensors'):
                for t in var.saved_tensors:
                    dot.edge(str(id(t)), str(id(var)))
                    add_nodes(t)
    add_nodes(var.grad_fn)
    return dot
```
</details>
<!---Documatic-block-tools.visualize.make_dot-end--->
<!---Documatic-section-make_dot-end--->

# #
<!---Documatic-section-tools.visualize.make_dot-end--->

[_Documentation generated by Documatic_](https://www.documatic.com)