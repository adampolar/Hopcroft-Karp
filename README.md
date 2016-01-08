# Hopcroft-Karp
A worst case O(E*sqrt(V)) (where E is the number of edges and V is the number of Vertices) solution for finding the maximum cardinality matching for a bipartite graph with the condition no edges share an endpoint. http://en.wikipedia.org/wiki/Hopcroft%E2%80%93Karp_algorithm

```C#
/**
 ** C# Program to Implement Hopcroft Algorithm
 **/



using System;
using System.Collections;
/** Class Hopcroft **/
using System.Collections.Generic;
public class HopCarp
{    
    private const int NIL = 0;
    private const int INF = int.MaxValue;
    private ArrayList[] Adj; 
    private int[] Pair;
    private int[] Dist;
    private int cx, cy;
 
     /** Function BFS **/
    public bool BFS() 
    {
        Queue<int> queue = new Queue<int>();
        for (int v = 1; v <= cx; ++v) 
            if (Pair[v] == NIL) 
            { 
                Dist[v] = 0; 
                queue.Enqueue(v); 
            }
            else 
                Dist[v] = INF;
 
        Dist[NIL] = INF;
 
        while (!(queue.Count == 0)) 
        {
            int v = queue.Dequeue();
            if (Dist[v] < Dist[NIL]) 
                foreach (int u in Adj[v]) 
                    if (Dist[Pair[u]] == INF) 
                    {
                        Dist[Pair[u]] = Dist[v] + 1;
                        queue.Enqueue(Pair[u]);
                    }           
        }
        return Dist[NIL] != INF;
    }    
     /** Function DFS **/
    public bool DFS(int v) 
    {
        if (v != NIL) 
        {
            foreach (int u in Adj[v]) 
                if (Dist[Pair[u]] == Dist[v] + 1)
                    if (DFS(Pair[u])) 
                    {
                        Pair[u] = v;
                        Pair[v] = u;
                        return true;
                    }               
 
            Dist[v] = INF;
            return false;
        }
        return true;
    }
     /** Function to get maximum matching **/
    public int HopcroftKarp() 
    {
        Pair = new int[cx + cy + 1];
        Dist = new int[cx + cy + 1];
        int matching = 0;
        while (BFS())
            for (int v = 1; v <= cx; ++v)
                if (Pair[v] == NIL)
                    if (DFS(v))
                        matching = matching + 1;
        return matching;
    }
    /** Function to make graph with vertices x , y **/
    public void makeGraph(int[] x, int[] y, int E)
    {
        Adj = new ArrayList[cx + cy + 1];
        for (int i = 0; i < Adj.Length; ++i)
            Adj[i] = new ArrayList();        
        /** adding edges **/    
        for (int i = 0; i < E; ++i) 
            addEdge(x[i] + 1, y[i] + 1);    
    }
    /** Function to add a edge **/
    public void addEdge(int u, int v) 
    {
        Adj[u].Add(cx + v);
        Adj[cx + v].Add(u);
    }    
    /** Main Method **/
    public static void Main (string[] args) 
    {
        System.Console.WriteLine("Hopcroft Algorithm Test\n");
        /** Make an object of Hopcroft class **/
        HopCarp hc = new HopCarp();
 
        /** Accept number of edges **/
        System.Console.WriteLine("Enter number of edges\n");
        int E = int.Parse(System.Console.ReadLine());
        int[] x = new int[E];
        int[] y = new int[E];
        hc.cx = 0;
        hc.cy = 0;
 
        System.Console.WriteLine("Enter "+ E +" x, y coordinates ");
        for (int i = 0; i < E; i++)
        {
            var temp = System.Console.ReadLine();
            x[i] = int.Parse(temp[0].ToString());
            y[i] = int.Parse(temp[1].ToString());
            hc.cx = Math.Max(hc.cx, x[i]);
            hc.cy = Math.Max(hc.cy, y[i]);
        }
        hc.cx += 1;
        hc.cy += 1;
 
        /** make graph with vertices **/
        hc.makeGraph(x, y, E);            
 
        System.Console.WriteLine("\nMatches : "+ hc.HopcroftKarp());
        System.Console.WriteLine("1 -> " + hc.Pair[1]);
        System.Console.WriteLine("2 -> " + hc.Pair[2]);
        System.Console.WriteLine("3 -> " + hc.Pair[3]);
        System.Console.WriteLine("4 -> " + hc.Pair[4]);
        System.Console.WriteLine("5 -> " + hc.Pair[5]); 
    }    
}
```
