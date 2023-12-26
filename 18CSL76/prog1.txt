def aStarAlgo(start_node, stop_node):
    open_set, closed_set, g, parents = {start_node}, set(), {start_node: 0}, {start_node: start_node}

    while open_set:
        n = min(open_set, key=lambda x: g[x] + heuristic(x))

        if n == stop_node or not Graph_nodes[n]:
            break

        for m, weight in get_neighbors(n):
            if m not in open_set and m not in closed_set:
                open_set.add(m)
                parents[m], g[m] = n, g[n] + weight
            elif g[m] > g[n] + weight:
                g[m], parents[m] = g[n] + weight, n

        open_set.discard(n)
        closed_set.add(n)

    path = [n]
    while parents[n] != n:
        path.append(parents[n])
        n = parents[n]
    path.reverse()
    print('Path found: {}'.format(path))
    return path if n == stop_node else None

def get_neighbors(v):
    return Graph_nodes[v] if v in Graph_nodes else None

def heuristic(n):
    H_dist = {'A': 10, 'B': 8, 'C': 5, 'D': 7, 'E': 3, 'F': 6, 'G': 5, 'H': 3, 'I': 1, 'J': 0}
    return H_dist[n]

Graph_nodes = {'A': [('B', 6), ('F', 3)], 'B': [('C', 3), ('D', 2)], 'C': [('D', 1), ('E', 5)],
               'D': [('C', 1), ('E', 8)], 'E': [('I', 5), ('J', 5)], 'F': [('G', 1), ('H', 7)],
               'G': [('I', 3)], 'H': [('I', 2)], 'I': [('E', 5), ('J', 3)]}

aStarAlgo('A', 'J')
