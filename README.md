[# Designing-a-Simple-OS-Task-Manager](https://docs.google.com/document/d/1K9a2OJHBCRWh0Mt4Kgs7erHeaxNU3FBz34ctwfeCM9o/edit?usp=sharing)

# 🖥️ Operating Systems Checkpoint
**Process Scheduling, Synchronization, Memory & Disk Management**

---

## Part 1: Process Scheduling

### 🔹 Given Processes

| Process | Arrival Time | Burst Time |
|---------|--------------|------------|
| P1      | 0            | 5          |
| P2      | 1            | 3          |
| P3      | 2            | 1          |

---

### ✅ 1. FCFS (First Come First Served)

#### 🔸 Execution Order
Processes are executed in the order they arrive.

#### 🔸 Gantt Chart
```
|   P1   |   P2   | P3 |
0        5        8    9
```

#### 🔸 Waiting Time Calculation
- **P1:** 0 − 0 = 0
- **P2:** 5 − 1 = 4
- **P3:** 8 − 2 = 6

#### 🔸 Average Waiting Time
(0 + 4 + 6) / 3 = 10 / 3 = **3.33**

---

### ✅ 2. Round Robin (Quantum = 2)

#### 🔸 Execution Order
Time-sliced execution, each process gets 2 units per turn.

#### 🔸 Gantt Chart
```
| P1 | P2 | P1 | P3 | P2 | P1 |
0    2    4    6    7    8    9
```

#### 🔸 Completion Times
- **P1** completes at 9
- **P2** completes at 8
- **P3** completes at 7

#### 🔸 Waiting Time Calculation
- **P1:** (9 − 0 − 5) = 4
- **P2:** (8 − 1 − 3) = 4
- **P3:** (7 − 2 − 1) = 4

#### 🔸 Average Waiting Time
(4 + 4 + 4) / 3 = **4**

---

### 🔍 Comparison & Responsiveness

- **FCFS** has lower average waiting time
- **Round Robin** is more responsive, especially for interactive systems, because:
  - No process monopolizes the CPU
  - Shorter response time for later-arriving processes

**✅ Conclusion:** 👉 **Round Robin is more responsive**, while **FCFS is simpler but less fair**.

---

## Part 2: Process Synchronization

### 🔹 Scenario
Two processes increment a shared variable `counter` 100 times each.

### ❌ What Could Go Wrong Without Synchronization?

Without synchronization:
- Both processes may read the same value simultaneously
- Updates can be overwritten
- Final value may be less than 200
- This is called a **race condition**

---

### ✅ Suggested Synchronization Mechanism
**Mutex (Mutual Exclusion Lock)**

#### 🔸 Pseudocode with Mutex
```
mutex lock;

Process P1 or P2:
for i = 1 to 100 do
    lock(mutex)
    counter = counter + 1
    unlock(mutex)
end for
```

#### ✅ Why This Works
- Only one process can access `counter` at a time
- Prevents race conditions
- Ensures data consistency

---

## Part 3: Memory Management

### 🔹 Given
- **Page Frames:** 3
- **Page Reference String:** 1, 2, 3, 2, 4, 1, 5

---

### ✅ 1. FIFO Page Replacement

| Reference | Frames  | Fault |
|-----------|---------|-------|
| 1         | 1       | Yes   |
| 2         | 1 2     | Yes   |
| 3         | 1 2 3   | Yes   |
| 2         | 1 2 3   | No    |
| 4         | 4 2 3   | Yes   |
| 1         | 4 1 3   | Yes   |
| 5         | 4 1 5   | Yes   |

**🔸 Total Page Faults (FIFO): 6**

---

### ✅ 2. LRU (Least Recently Used)

| Reference | Frames  | Fault |
|-----------|---------|-------|
| 1         | 1       | Yes   |
| 2         | 1 2     | Yes   |
| 3         | 1 2 3   | Yes   |
| 2         | 1 2 3   | No    |
| 4         | 4 2 3   | Yes   |
| 1         | 4 2 1   | Yes   |
| 5         | 5 2 1   | Yes   |

**🔸 Total Page Faults (LRU): 6**

---

### 🔍 Comparison
- Both algorithms result in **6 page faults**
- **LRU generally performs better** because it uses recent usage patterns
- In this case, the reference string limits LRU's advantage

---

## Part 4: Disk Scheduling

### 🔹 Given
- **Initial Head Position:** 53
- **Requests:** 98, 183, 37, 122, 14, 124, 65, 67

---

### ✅ 1. FCFS Disk Scheduling

#### 🔸 Order
53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67

#### 🔸 Total Head Movement
```
|53-98|   = 45
|98-183|  = 85
|183-37|  = 146
|37-122|  = 85
|122-14|  = 108
|14-124|  = 110
|124-65|  = 59
|65-67|   = 2
```

**🔸 Total = 640 tracks**

---

### ✅ 2. SSTF (Shortest Seek Time First)

#### 🔸 Order
53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183

#### 🔸 Total Head Movement
```
|53-65|    = 12
|65-67|    = 2
|67-37|    = 30
|37-14|    = 23
|14-98|    = 84
|98-122|   = 24
|122-124|  = 2
|124-183|  = 59
```

**🔸 Total = 236 tracks**

---

### ✅ Conclusion
- **SSTF is far more efficient**
- Minimizes seek time
- Reduces total head movement significantly

---

## ✅ Final Summary

| Topic              | Best Approach                   |
|--------------------|---------------------------------|
| CPU Scheduling     | Round Robin (Responsiveness)    |
| Synchronization    | Mutex                           |
| Memory Management  | LRU (generally better)          |
| Disk Scheduling    | SSTF                            |

---
