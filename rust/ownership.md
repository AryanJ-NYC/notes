---
description: >-
  Rust’s central feature is ownership. Although the feature is straightforward
  to explain, it has deep implications for the rest of the language.
---

# Ownership

In Rust, memory is managed through a system of ownership with a set of rules the compiler checks at compile time.

## Stack vs. Heap

#### Stack

* all data stored on the stack must have a known, fixed size.

#### Heap

* data with an unknown size at compile time or a size that might change must be stored on the heap
* the heap is less organized
  * when you put data on the heap, you request a certain amount of space
  * the OS finds an empty spot in the heap that is big enough, marks it as being in use and returns a _pointer_ \(the address of that location\)
    * this process is called _allocating on the heap_
  * since the pointer is of a known, fixed size, you can store the pointer on the stack
    * when you want the actual data, you must follow the pointer

#### Writing Data

* pushing to the stack is faster than allocating on the heap because the OS never has to search for a place to store new data; that location is always at the top of the stack
* comparatively, allocating space on the heap requires more work
  * the OS must first find a big enough space to hold the data then perform bookkeeping to prepare for the next allocation

#### Reading Data

* accessing data on the heap is slower than accessing data on the stack because you have to follow a pointer to get there

{% hint style="info" %}
Managing heap data is why ownership exists
{% endhint %}

## Ownership Rules

* each value in Rust has a variable that's called its _owner_
* there can only be one owner at a time
* when the owner goes out of scope, the value will be dropped



