# Essential Greedy/Graph Templates (C++)

## 1. Interval Scheduling — Max Non-Overlapping Tasks
Sort by **end time**; greedily pick if start >= last end.

```cpp
int maxNonOverlapping(vector<pair<int,int>>& intervals) {
    sort(intervals.begin(), intervals.end(),
         [](auto& a, auto& b){ return a.second < b.second; });

    int count = 0, lastEnd = INT_MIN;
    for (auto& [s, e] : intervals) {
        if (s >= lastEnd) {
            count++;
            lastEnd = e;
        }
    }
    return count;
}
```
*Use for:* Activity Selection, Non-overlapping Intervals, Max meetings in one room.

---

## 2. Interval Partitioning — Min Resources/Rooms
Sort by **start time**; use a min-heap of end times to reuse freed resources.

```cpp
int minRooms(vector<pair<int,int>>& intervals) {
    sort(intervals.begin(), intervals.end());
    priority_queue<int, vector<int>, greater<int>> minHeap; // end times

    for (auto& [s, e] : intervals) {
        if (!minHeap.empty() && minHeap.top() <= s) {
            minHeap.pop(); // reuse room
        }
        minHeap.push(e);
    }
    return minHeap.size();
}
```
*Use for:* Meeting Rooms II, Minimum Platforms, CPU task scheduling.

---

## 3. Lexicographical Reduction — Monotonic Stack
Drop larger previous elements while it's still beneficial (limited by k removals or a "keep smaller" rule).

```cpp
string removeKdigits(string num, int k) {
    string ans;
    for (char c : num) {
        while (!ans.empty() && k > 0 && ans.back() > c) {
            ans.pop_back();
            k--;
        }
        ans.push_back(c);
    }
    while (k-- > 0 && !ans.empty()) ans.pop_back();

    int start = 0;
    while (start < (int)ans.size() - 1 && ans[start] == '0') start++;
    ans = ans.substr(start);
    return ans.empty() ? "0" : ans;
}
```
*Use for:* Remove K Digits, Smallest Subsequence of Distinct Characters, Remove Duplicate Letters.

---

## 4. Regret Greedy / Job Sequencing — Min-Heap Swap
Take every job greedily; if a later job is more profitable but you're out of slots, evict the worst one taken so far.

```cpp
// Jobs sorted by deadline; each unit-time slot used once.
int maxProfitJobScheduling(vector<pair<int,int>>& jobs) { // {deadline, profit}
    sort(jobs.begin(), jobs.end());
    priority_queue<int, vector<int>, greater<int>> minHeap; // profits taken
    int time = 0, totalProfit = 0;

    for (auto& [deadline, profit] : jobs) {
        if ((int)minHeap.size() < deadline) {
            minHeap.push(profit);
            totalProfit += profit;
        } else if (!minHeap.empty() && minHeap.top() < profit) {
            totalProfit += profit - minHeap.top();
            minHeap.pop();
            minHeap.push(profit);
        }
    }
    return totalProfit;
}
```
*Use for:* Job Sequencing with Deadlines, IPO (LeetCode 502), Maximum Profit in Job Scheduling variants.

---

## 5. Exchange Argument Sorting — Custom Comparator
Prove `a` should come before `b` if `a+b < b+a` (or similar swap condition), then sort by that rule.

```cpp
// Classic example: arrange numbers to form the smallest/largest number
string arrangeSmallest(vector<int>& nums) {
    vector<string> strs;
    for (int n : nums) strs.push_back(to_string(n));

    sort(strs.begin(), strs.end(), [](const string& a, const string& b) {
        return a + b < b + a; // exchange argument
    });

    string result;
    for (auto& s : strs) result += s;

    // strip leading zeros if needed
    int i = 0;
    while (i < (int)result.size() - 1 && result[i] == '0') i++;
    return result.substr(i);
}
```
*Use for:* Smallest/Largest Number formed from array, Minimize sum of two numbers, Queue reconstruction by height.

---

## 6. Minimum Spanning Tree (Kruskal's) — Sort Edges + DSU
Sort edges by weight; use Union-Find to avoid cycles.

```cpp
class DSU {
    vector<int> parent, rank_;
public:
    DSU(int n) : parent(n), rank_(n, 0) {
        iota(parent.begin(), parent.end(), 0);
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    bool unite(int x, int y) {
        int rx = find(x), ry = find(y);
        if (rx == ry) return false;
        if (rank_[rx] < rank_[ry]) swap(rx, ry);
        parent[ry] = rx;
        if (rank_[rx] == rank_[ry]) rank_[rx]++;
        return true;
    }
};

int kruskalMST(int n, vector<vector<int>>& edges) { // {weight, u, v}
    sort(edges.begin(), edges.end());
    DSU dsu(n);
    int totalWeight = 0, edgesUsed = 0;

    for (auto& e : edges) {
        int w = e[0], u = e[1], v = e[2];
        if (dsu.unite(u, v)) {
            totalWeight += w;
            edgesUsed++;
            if (edgesUsed == n - 1) break;
        }
    }
    return totalWeight; // check edgesUsed == n-1 for connectivity
}
```
*Use for:* Min Cost to Connect All Points, Connecting Cities With Minimum Cost, Network graph optimization.

---

## 7. Two-Pointer Matchmaking — Sort Both Arrays
Sort both sequences; greedily match with two pointers to satisfy constraints.

```cpp
int assignCookies(vector<int>& greed, vector<int>& cookies) {
    sort(greed.begin(), greed.end());
    sort(cookies.begin(), cookies.end());

    int i = 0, j = 0, satisfied = 0;
    while (i < (int)greed.size() && j < (int)cookies.size()) {
        if (cookies[j] >= greed[i]) {
            satisfied++;
            i++;
        }
        j++;
    }
    return satisfied;
}
```
*Use for:* Assign Cookies, Boats to Save People, Advantage Shuffle.

---

## Difficulty Ratings Per Problem

| Template | Problem | LeetCode Difficulty | Interview Toughness (1-10) |
|---|---|---|---|
| Interval Scheduling | Non-overlapping Intervals | Medium | 4 |
| Interval Scheduling | Activity Selection (GeeksforGeeks classic) | Medium | 3 |
| Interval Partitioning | Meeting Rooms II | Medium | 5 |
| Interval Partitioning | Minimum Platforms | Medium | 5 |
| Lexicographical Reduction | Remove K Digits | Medium | 6 |
| Lexicographical Reduction | Remove Duplicate Letters | Medium | 7 |
| Lexicographical Reduction | Smallest Subsequence of Distinct Characters | Medium | 7 |
| Regret Greedy / Job Sequencing | Job Sequencing with Deadlines | Medium | 6 |
| Regret Greedy / Job Sequencing | IPO (LC 502) | Hard | 7 |
| Exchange Argument Sorting | Largest Number (LC 179) | Medium | 6 |
| Exchange Argument Sorting | Queue Reconstruction by Height (LC 406) | Medium | 7 |
| Kruskal's MST | Min Cost to Connect All Points (LC 1584) | Medium | 6 |
| Kruskal's MST | Connecting Cities With Minimum Cost (LC 1135) | Medium | 5 |
| Two-Pointer Matchmaking | Assign Cookies (LC 455) | Easy | 2 |
| Two-Pointer Matchmaking | Boats to Save People (LC 881) | Medium | 4 |
| Two-Pointer Matchmaking | Advantage Shuffle (LC 870) | Medium | 6 |

**How to read "Interview Toughness":**
- **1-3:** Pattern is obvious once you know the template; low execution risk.
- **4-6:** Pattern recognition takes a moment; a few edge cases (empty arrays, ties, off-by-one) can trip you up.
- **7-8:** Requires proving the greedy choice is correct (exchange argument) or combining two techniques; easy to get a "greedy but wrong" solution that passes some tests.
- **9-10:** Rare for pure greedy; usually greedy is only half the solution (paired with DP, binary search, or a non-obvious invariant).

If you want, I can add ratings for the Part 2 (Advanced/Hybrid Greedy) problems too, once you decide if you want that section added.

---

## Quick Pattern Recognition Cheat Sheet

| Signal in Problem | Likely Template |
|---|---|
| "max number of non-overlapping" | Interval Scheduling |
| "min rooms/resources needed" | Interval Partitioning |
| "smallest number after removing k digits" | Lexicographical Reduction |
| "deadlines + profits, maximize" | Regret Greedy / Job Sequencing |
| "arrange to form smallest/largest" | Exchange Argument Sorting |
| "connect all nodes at min cost" | Kruskal's MST |
| "match two groups under constraints" | Two-Pointer Matchmaking |
