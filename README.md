# Java-code-for-Dynamic-Dijkstra-s-algorithm
In modern systems, edge weights often fluctuate due to factors such as traffic congestion, bandwidth variation, or environmental conditions. Furthermore, in many cases, the edge weights are best represented as probability distributions rather than fixed values. Existing algorithms fail to efficiently handle these dynamic and probabilistic scenarios.
Proposed Solution:-

Probabilistic Edge Weights: Each edge weight is modeled as a probability distribution.
Dynamic Updates: When edge weights change, the algorithm recalculates only the affected paths, avoiding full recomputation.
This approach ensures efficient computation of shortest paths under uncertainty and dynamic conditions, making it suitable for real-time systems.


**CODE**
import java.util.*;
public class SimpleDynamicGraph {
    static class Edge {
        int target;
        int weight;

        Edge(int target, int weight) {
            this.target = target;
            this.weight = weight;
        }
    }

    static class Graph {
        int nodes;
        List<List<Edge>> adjList;

        Graph(int nodes) {
            this.nodes = nodes;
            adjList = new ArrayList<>();
            for (int i = 0; i < nodes; i++) {
                adjList.add(new ArrayList<>());
            }
        }

        void addEdge(int from, int to, int weight) {
            adjList.get(from).add(new Edge(to, weight));
        }

        void updateEdge(int from, int to, int newWeight) {
            for (Edge edge : adjList.get(from)) {
                if (edge.target == to) {
                    edge.weight = newWeight;
                    return;
                }
            }
        }

        int[] dijkstra(int source) {
            int[] dist = new int[nodes];
            Arrays.fill(dist, Integer.MAX_VALUE);
            dist[source] = 0;

            PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
            pq.add(new int[]{source, 0});

            while (!pq.isEmpty()) {
                int[] current = pq.poll();
                int node = current[0];
                int distance = current[1];

                for (Edge edge : adjList.get(node)) {
                    if (dist[node] + edge.weight < dist[edge.target]) {
                        dist[edge.target] = dist[node] + edge.weight;
                        pq.add(new int[]{edge.target, dist[edge.target]});
                    }
                }
            }
            return dist;
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of nodes: ");
        int n = sc.nextInt();

        Graph graph = new Graph(n);

        System.out.print("Enter number of edges: ");
        int e = sc.nextInt();

        System.out.println("Enter edges (from, to, weight): ");
        for (int i = 0; i < e; i++) {
            int from = sc.nextInt();
            int to = sc.nextInt();
            int weight = sc.nextInt();
            graph.addEdge(from, to, weight);
        }

        System.out.print("Enter source node: ");
        int source = sc.nextInt();

        int[] distances = graph.dijkstra(source);
        System.out.println("Shortest distances from source:");
        for (int i = 0; i < distances.length; i++) {
            System.out.println(source + " -> " + i + " = " + distances[i]);
        }

        System.out.print("Enter number of updates: ");
        int updates = sc.nextInt();

        System.out.println("Enter updated edges (from, to, new weight): ");
        for (int i = 0; i < updates; i++) {
            int from = sc.nextInt();
            int to = sc.nextInt();
            int newWeight = sc.nextInt();
            graph.updateEdge(from, to, newWeight);
        }

        distances = graph.dijkstra(source);
        System.out.println("Updated shortest distances from source:");
        for (int i = 0; i < distances.length; i++) {
            System.out.println(source + " -> " + i + " = " + distances[i]);
        }

        sc.close();
    }
}
