# DFS

Start typing here...
```Java
package com.wayfair.ancestor;

import java.util.*;
import java.util.HashSet;
import java.util.Set;

public class Main {

    private void dfsGraph(Map<Integer, ArrayList<Integer>> map, int parent, int curr, Map<Integer, ArrayList<Integer>> result, Set<Integer> visit) {
        visit.add(curr);
        for (int dest : map.get(curr)) {
            if (!visit.contains(dest)) {
                result.get(dest).add(parent);
                dfsGraph(map, parent, dest, result, visit);
            }
        }
    }

    public Map<Integer, ArrayList<Integer>> getAncestors(int[][] edges) {
        Set<Integer> nodes = new HashSet<>();
        for (int[] edge : edges) {
            nodes.add(edge[0]);
            nodes.add(edge[1]);
        }
        Map<Integer, ArrayList<Integer>> graph = new HashMap<>();
        Map<Integer, ArrayList<Integer>> result = new HashMap<>();
        for (int node: nodes) {
            result.put(node, new ArrayList<>());
            graph.put(node, new ArrayList<>());
        }
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
        }
        for (int node: graph.keySet()) {
            dfsGraph(graph, node, node, result, new HashSet<>());
        }
        return result;
    }

    public boolean hasSharedAncestor(int[][] parentChildPairs, int node1, int node2) {
        Map<Integer, ArrayList<Integer>> ancestors = getAncestors(parentChildPairs);
        List<Integer> node1Ancestors = ancestors.get(node1);
        List<Integer> node2Ancestors = ancestors.get(node2);
        for (int ancestor1: node1Ancestors) {
            for (int ancestor2: node2Ancestors) {
                if (ancestor1 == ancestor2) {
                    return true;
                }
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[][] parentChildPairs = {
                {1, 3}, {2, 3}, {3, 6},
                {5, 6}, {5, 7}, {4, 5},
                {4, 8}, {8, 9}, {10, 2}
        };
        Main ob = new Main();
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 3, 8));
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 5, 8));
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 6, 8));
    }
}

```
