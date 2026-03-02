# Greedy Algorithms - Interview Notes

## 🎯 Core Idea of Greedy

At each step, choose the **locally optimal option** (the "best-looking" move right now).  
Hope that these local choices add up to a **global optimum**.

**Key Requirement:** Works only when the problem has:
- ✅ **Greedy-choice property** + **Optimal substructure**

---

## 🔍 Problem-Solving Checklist

When faced with a problem, ask yourself:

| Question | Purpose |
|----------|---------|
| **Prioritization/Sorting** | Is there an opportunity to sort or prioritize elements? |
| **Future Impact** | Does the locally "best" option preserve future possibilities? |
| **Optimality Proof** | Can you prove the greedy choice gives globally optimal solution? |
| **Counterexample** | Find one case where greedy fails? If yes → use **DP instead** |

---

## 📋 Common Greedy Patterns At A Glance

| Pattern | Example | Approach |
|---------|---------|----------|
| **Interval Scheduling** | Activity Selection | Pick earliest finishing time |
| **MST** | Kruskal's/Prim's | Pick smallest edge, avoid cycles |
| **Shortest Path** | Dijkstra's | Always pick shortest distance node |
| **Huffman Encoding** | Frequency Merging | Pick 2 smallest frequencies |
| **Sorting-based** | Fractional Knapsack | Sort by ratio, then pick |
| **String/Array** | Jump Game | Lexicographically smallest |

---

## 🧩 5 Core Greedy Coding Patterns

> **Key Insight:** Greedy problems follow patterns, just like sliding window or two pointers. Once you recognize the shape, the solution almost always follows one of these skeletons.

### 1️⃣ Sort + Pick (Most Common)

**When:** Sort elements (by value, end time, ratio, etc.), then greedily pick.

**Used in:**
- Activity Selection / Meeting Rooms → sort by end time
- Fractional Knapsack → sort by value/weight ratio
- Job Sequencing with Deadlines → sort by profit

**Code Skeleton:**
```java
Arrays.sort(arr, (a, b) -> a[1] - b[1]); // sort by end time
for (int[] interval : arr) {
    if (interval[0] >= lastEnd) {
        // pick this interval
        lastEnd = interval[1];
    }
}
```

---

### 2️⃣ Greedy with Heap / Priority Queue

**When:** You need to always pick min/max element dynamically.

**Used in:**
- Huffman Encoding → repeatedly merge smallest 2
- Connect Ropes → always pick 2 smallest ropes
- Meeting Rooms II → track room availability with min-heap of end times

**Code Skeleton:**
```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
for (int num : arr) pq.add(num);

while (pq.size() > 1) {
    int a = pq.poll();
    int b = pq.poll();
    int cost = a + b;
    pq.add(cost);
}
```

---

### 3️⃣ Greedy with Running Variables (Jumping / Tracking)

**When:** No sorting, no heap — just track the furthest/current best option as you iterate.

**Used in:**
- Jump Game (LC 55/45) → keep `maxReach`
- Gas Station → keep running sum + reset if negative

**Code Skeleton:**
```java
int maxReach = 0;
for (int i = 0; i < nums.length; i++) {
    if (i > maxReach) return false; // stuck
    maxReach = Math.max(maxReach, i + nums[i]);
}
return true;
```

---

### 4️⃣ Greedy by Frequency / Count

**When:** Problem is about choosing based on counts or balancing.

**Used in:**
- Rearrange String (no adjacent same char)
- Task Scheduler (LC 621)
- Top-K Frequencies

**Code Skeleton:**
```java
Map<Character, Integer> freq = new HashMap<>();
for (char c : s.toCharArray()) 
    freq.put(c, freq.getOrDefault(c, 0) + 1);

PriorityQueue<Map.Entry<Character, Integer>> pq =
    new PriorityQueue<>((a, b) -> b.getValue() - a.getValue());

pq.addAll(freq.entrySet());
// greedily pick most frequent first
```

---

### 5️⃣ Greedy with Two Pointers

**When:** You need to pair things optimally.

**Used in:**
- Assign Cookies (LC 455) → match smallest child with smallest cookie
- Boat to Save People (LC 881) → match heaviest with lightest

**Code Skeleton:**
```java
Arrays.sort(arr);
int i = 0, j = arr.length - 1, boats = 0;
while (i <= j) {
    if (arr[i] + arr[j] <= limit) i++;
    j--;
    boats++;
}
```

---

## ✅ Quick Reference: The 5 Patterns

1. **Sort + Pick** — Most versatile
2. **Heap/PriorityQueue** — Dynamic selection
3. **Running Variable Tracking** — Simple iteration
4. **Frequency/Counting** — Balancing problems
5. **Two Pointers** — Pairing problems
