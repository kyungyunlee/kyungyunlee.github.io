---
layout: post
title:  Clustered vs. non-clustered indexes
date: 2021-01-06
description: 
categories: study
tags: CS
---

Indexes in database are used to increase the query performance from the table. 

Without indexes, when you want to find a specific entry in a large table, you would have to scan through the entire table from beginning to end until you find what you want (Linear searching time). 

There are two major types of indexes in SQL server: Clustered and non-clustered indexes.

#### Analogy 

**Clustered index** - alphabetical sorting of last name and first name in a telephone book.

**Non-clustered index** - index of chapters at the beginning of a textbook, index of terms at the end of the textbook. 

![main-qimg-a578a13aeca2112760f11b2084f00065](https://tva1.sinaimg.cn/large/0081Kckwgy1gmdset2pnfj30gq05tgm6.jpg)



This diagram helped me understand what "clustered" meant in database. "Clustered" refers to whether the actual rows in the table are close together in the physical table or not. In case of non-clustered index, index pointers are all over the table, while for clustered index, pointers are in order (there can be exceptions once you start inserting lots of rows in the table; will need to perform re-sorting). 



#### Clustered index

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gmds9rdtnqj31750u041l.jpg" alt="Data Engineering-4 2" style="zoom: 33%;" />

Clustered index is the same as the ordering of the physical table. Therefore, there can be **only one** clustered index per table. Also, since it corresponds directly to the physical table, retrieval is faster than non-clustered table. 

**Pros**

* Direct / single access to the table
* Good for sequential retrieval 
* Minimize page transfer and maximize cache hit 

**Cons**

* Modification (insertion, updates, deletes) require extra work, such as re-ordering of the tables so that entries are in order.



#### Non-clustered index

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gmds9fycxwj31it0u0aer.jpg" alt="Data Engineering-5" style="zoom:33%;" />

Non-clustered index does not correspond to actual orderings in the physical table. It has pointers to the actual entry in the table and is stored in a separately from the table. 



**Pros**

* Can have multiple non-clustered indexes
* Insertion into the physical table does not affect performance

**Cons**

* Lookup is costly (No locality, cache misses)
* Slower data access compared to clustered index
* Need extra space to store the index 



#### References 

* https://www.youtube.com/watch?v=HauoKagBM_c&list=PLzzVuDSjP25QCp7S4_8b-rX9vytv6OqsZ&index=5

* https://www.guru99.com/clustered-vs-non-clustered-index.html



