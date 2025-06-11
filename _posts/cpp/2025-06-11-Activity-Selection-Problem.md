---
title: "Activity Selection Problem"
date: 2025-06-11 00:00:00 +0800
categories: [C++]
tags: [Greedy, Algorithm, C++]
---

## What is the Activity Selection Problem?

The **Activity Selection Problem** is a fundamental example of a **Greedy Algorithm**.  
Given a list of activities, each with a start and finish time, the task is to select the maximum number of activities that **don't overlap** — meaning no two activities can be scheduled at the same time.

This problem commonly arises in **scheduling**, where resources (such as people, rooms, or processors) are limited, and we must choose the best combination of activities to maximize utilization.

---

## Problem Statement

You are given `n` activities with their **start and end times**. Select the maximum number of activities that can be performed by a single person, assuming that a person can only work on a single activity at a time.

### Example:
Given activities with time intervals:
```
(1, 3), (2, 4), (0, 6), (5, 7), (8, 9), (5, 9)
```

We want to choose the most non-overlapping activities.

---

## Greedy Strategy

To solve this efficiently, we use a **greedy approach**:
1. **Sort** all activities based on their **ending time**.
2. Always pick the activity that **finishes earliest** and doesn't overlap with the previously selected one.
3. Repeat until all activities are checked.

This works because **choosing the activity that ends soonest** gives us the largest remaining time to accommodate other activities.

---

## Python Implementation

Here’s a simple Python program that demonstrates this strategy:

```python
def activity_selection(activities):
    # Sort activities by end time
    sorted_activities = sorted(activities, key=lambda x: x[1])

    selected = [sorted_activities[0]]
    last_end_time = sorted_activities[0][1]

    for start, end in sorted_activities[1:]:
        if start >= last_end_time:
            selected.append((start, end))
            last_end_time = end

    return selected

# List of (start, end) time pairs
activities = [(1, 3), (2, 4), (0, 6), (5, 7), (8, 9), (5, 9)]

# Select optimal activities
selected = activity_selection(activities)

print("Selected Activities:")
for act in selected:
    print(act)
```

### Output:
```
Selected Activities:
(1, 3)
(5, 7)
(8, 9)
```

---

## Why Greedy Works?

The greedy algorithm works optimally for this problem because **locally optimal decisions** (choosing the soonest finishing activity) lead to a **globally optimal solution**. By always picking the next activity that ends the earliest, we leave room for more activities afterward.

---

## Real-Life Applications

- **Conference Room Scheduling** – Booking sessions without overlapping.
- **Task Scheduling** – Assigning jobs to a single processor or worker.
- **Class Timetabling** – Selecting lectures that don’t overlap.

---

## Related Topics

- Greedy Algorithms
- Interval Scheduling
- Dynamic Programming (for extended variants)
- Job Sequencing Problem

---

*Posted by Naailah Mazaya  