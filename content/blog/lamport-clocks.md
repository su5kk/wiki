---
title: Time, Clocks and the Ordering of Events in a Distributed System by Leslie Lamport.
date: 2025-07-24
authors:
  - name: Tim
    link: https://su5kk.github.io/wiki/about/
    image: https://github.com/su5kk.png
tags:
  - Papers
  - Distributed systems
---

In this paper, Leslie Lamport introduces a distributed algorithm for
synchronizing logical and physical clocks and demonstrates how the clocks can be used for
establishing a total order of the events within the distributed system.
<!--more-->

## Total order
In simple terms, a total order means that for any two events in a system, we can always definitively say which one happened first. There are no ambiguities, ties, or "we don't know" answers. It's like taking all the events happening across multiple machines and arranging them into a single, unambiguous timeline.

This concept is crucial in distributed systems because it creates a shared source of truth. When different nodes act concurrently, they need a consistent way to agree on the sequence of operations. Without a total order, a system could grant two users the last available item or process transactions in a conflicting sequence. By establishing a single, predictable timeline, we can design algorithms that are reliable and behave correctly, as if all operations were happening in one place.

## Logical Clock synchronization

TLDR:
1. Nodes share their local logical time by sending timestamps with the messages.
2. Upon receiving, a node compares its local time with the received time and
   sets its local time to `max(local, received) + 1`
3. This ensures that logical ticks are synchronized across the nodes.

One of the problems with distributed systems is that the nodes in this system do
not know the state of one another by default. This includes the local time on
the node (logical or physical). Knowing the time is fundamental to any
distributed algorithm, because it is the most important characteristic actors
can rely on, when making decisions.

For example: if a node wants to acquire an exclusive access to a shared
resource, it must send a `REQUEST` message and then get `ACK`s from all other participants. However, just getting
acknowledgements is not enough, the node also has to know that these `ACK`s
happened **after** it sent the request. The only way to know that is to make all
nodes within the system aware of time in some form.

## Physical Clock synchronization

For synchronizing physical clocks, we have to establish a few conditions:
1. Unless reset, the time in physical clock is **always moving forward**.
2. Assume there is a **minimum possible travel time** for messages
   `travel_delay`.

Then, the synchronization part is similar to that of a logical clock:
1. If a node sends a message `m` at physical time `t`, then `m` contains a timestamp `T_m`
2. Upon receiving a message `m` at time `t'`, a node sets its local time to equal to `max(local_time, T_m + travel_delay)`.

## References

- [Original paper](https://lamport.azurewebsites.net/pubs/time-clocks.pdf)
