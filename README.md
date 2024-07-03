```Java
public class Main {

    private void dfsGraph(Map<Integer, ArrayList<Integer>> adj, int parent, int curr,
    Map<Integer, ArrayList<Integer>> ancestors, Set<Integer> visit) {
        visit.add(curr);
        for (int child : adj.get(curr)) {
            if (!visit.contains(child)) {
                ancestors.get(child).add(parent);
                dfsGraph(adj, parent, child, ancestors, visit);
            }
        }
    }

    public Map<Integer, ArrayList<Integer>> getAncestors(int[][] edges) {
        Set<Integer> nodes = new HashSet<>();
        for (int[] edge : edges) {
            nodes.add(edge[0]);
            nodes.add(edge[1]);
        }
        Map<Integer, ArrayList<Integer>> adjacencyList = new HashMap<>();
        Map<Integer, ArrayList<Integer>> ancestors = new HashMap<>();
        for (int node: nodes) {
            ancestors.put(node, new ArrayList<>());
            adjacencyList.put(node, new ArrayList<>());
        }
        for (int[] edge : edges) {
            adjacencyList.get(edge[0]).add(edge[1]);
        }
        for (int node: adjacencyList.keySet()) {
            dfsGraph(adjacencyList, node, node, ancestors, new HashSet<>());
        }
        return ancestors;
    }

    public boolean hasSharedAncestor(int[][] parentChildPairs, int node1, int node2) {
        if (node1 == node2)
            return false;
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
                {1, 3}, {2, 3}, {3, 6}, {5, 6}, {5, 7}, {4, 5}, {4, 8}, {8, 9}, {10, 2}
        };
        Main ob = new Main();
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 3, 8));
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 5, 8));
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 6, 8));
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 6, 9));
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 8, 9));
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 2, 10));
        System.out.println(ob.hasSharedAncestor(parentChildPairs, 2, 2));
    }
}

```
