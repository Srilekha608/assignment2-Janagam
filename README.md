# assignment2-janagam

# Srilekha Janagam

###### Bali Indonesia

 Bali has been hosting travelers, tourists, **adventure seekers** and beach bums for a long time. The small island has seemingly found the perfect medium between holding strong to its local culture/customs, while catering nonchalantly to outsiders tastes. Visitors will be awestruck by the stunning landscapes, ancient temples and **traditional practices**; yet they will gladly accept the comforts of modern cafés, boutique shopping spots, western eateries and fun, lively bars. Bali is an excellent place to experience all the draws and **attractions of Asia**, without battling through culture shock along the way.The **adventures and stunning landscapes caught my insterest!!!**.

---
# Directions to Bali from kansas

1. Book online tickets to Indonesia international airport from kansas city airport through airlines website
2. go to airport atleast two hours before.
3. If your not done with web-check go to respective flight Checkin counter and do inperson checkin.
4. Go to secuity check and have a seat in flight.
5. After long journey and some turbulence you'll reach to the city  **Bali**.

# Items list to be carried

* Passort ,ticket confirmation
* Sufficient money for travel.
* Camera to capture moments.
* Clothes based on the climatic conditions.
* Snacks

[About me](./AboutMe.md)

---
# Favorite Food

I would like to recommend 4 food items which i'm really intrested.

| Item | Place | Price |
| -----| ----- | ----- |
| Bisibelebath  | Mysur | Rs.350 |
| Dosa | Ballari | Rs.300 |
| Popcorn Biriyani | Hubli | Rs.250 |
| Samosa | Mangalore | Rs.300 |

---
---

# Motivational Quotes

> "The secret of getting ahead is getting started." -*Srilekha Janagam*

> "Opportunities don’t happen, you create them." -*Chris tim*

***

***
> Spanning tree has n-1 edges, where n is the number of nodes (vertices).

> From a complete graph, by removing maximum e - n + 1 edges, we can construct a spanning tree.

> A complete graph can have maximum nn-2 number of spanning trees.

> Thus, we can conclude that spanning trees are a subset of connected Graph G and   disconnected graphs do not have spanning tree.

[Link of content](https://www.tutorialspoint.com/data_structures_algorithms/spanning_tree.htm)


```
struct edge {
    int s, e, w, id;
    bool operator<(const struct edge& other) { return w < other.w; }
};
typedef struct edge Edge;

const int N = 2e5 + 5;
long long res = 0, ans = 1e18;
int n, m, a, b, w, id, l = 21;
vector<Edge> edges;
vector<int> h(N, 0), parent(N, -1), size(N, 0), present(N, 0);
vector<vector<pair<int, int>>> adj(N), dp(N, vector<pair<int, int>>(l));
vector<vector<int>> up(N, vector<int>(l, -1));

pair<int, int> combine(pair<int, int> a, pair<int, int> b) {
    vector<int> v = {a.first, a.second, b.first, b.second};
    int topTwo = -3, topOne = -2;
    for (int c : v) {
        if (c > topOne) {
            topTwo = topOne;
            topOne = c;
        } else if (c > topTwo && c < topOne) {
            topTwo = c;
        }
    }
    return {topOne, topTwo};
}

void dfs(int u, int par, int d) {
    h[u] = 1 + h[par];
    up[u][0] = par;
    dp[u][0] = {d, -1};
    for (auto v : adj[u]) {
        if (v.first != par) {
            dfs(v.first, u, v.second);
        }
    }
}

pair<int, int> lca(int u, int v) {
    pair<int, int> ans = {-2, -3};
    if (h[u] < h[v]) {
        swap(u, v);
    }
    for (int i = l - 1; i >= 0; i--) {
        if (h[u] - h[v] >= (1 << i)) {
            ans = combine(ans, dp[u][i]);
            u = up[u][i];
        }
    }
    if (u == v) {
        return ans;
    }
    for (int i = l - 1; i >= 0; i--) {
        if (up[u][i] != -1 && up[v][i] != -1 && up[u][i] != up[v][i]) {
            ans = combine(ans, combine(dp[u][i], dp[v][i]));
            u = up[u][i];
            v = up[v][i];
        }
    }
    ans = combine(ans, combine(dp[u][0], dp[v][0]));
    return ans;
}

int main(void) {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        parent[i] = i;
        size[i] = 1;
    }
    for (int i = 1; i <= m; i++) {
        cin >> a >> b >> w; // 1-indexed
        edges.push_back({a, b, w, i - 1});
    }
    sort(edges.begin(), edges.end());
    for (int i = 0; i <= m - 1; i++) {
        a = edges[i].s;
        b = edges[i].e;
        w = edges[i].w;
        id = edges[i].id;
        if (unite_set(a, b)) { 
            adj[a].emplace_back(b, w);
            adj[b].emplace_back(a, w);
            present[id] = 1;
            res += w;
        }
    }
    dfs(1, 0, 0);
    for (int i = 1; i <= l - 1; i++) {
        for (int j = 1; j <= n; ++j) {
            if (up[j][i - 1] != -1) {
                int v = up[j][i - 1];
                up[j][i] = up[v][i - 1];
                dp[j][i] = combine(dp[j][i - 1], dp[v][i - 1]);
            }
        }
    }
    for (int i = 0; i <= m - 1; i++) {
        id = edges[i].id;
        w = edges[i].w;
        if (!present[id]) {
            auto rem = lca(edges[i].s, edges[i].e);
            if (rem.first != w) {
                if (ans > res + w - rem.first) {
                    ans = res + w - rem.first;
                }
            } else if (rem.second != -1) {
                if (ans > res + w - rem.second) {
                    ans = res + w - rem.second;
                }
            }
        }
    }
    cout << ans << "\n";
    return 0;
}
```

[Source of Algorithm](https://cp-algorithms.com/graph/second_best_mst.html)