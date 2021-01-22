---
layout: post
title: "Key-Value Databases"
description: "Understanding what they are where they live and what they feed on"
date: 01/01/1970
feature_image: images/kv-database/lockers.jpg
language: en-US
tags: [data%en-US%, nosql%en-US%]
---

Key-Value database is a term that defines a number of NoSQL of databases with similar characteristics, but not all of them work the same way. Here I will explain some of these characteristics without going too deep on any specific feature.

<!--more-->

If you want to know a little more about NoSQL in general, the distinction between them and relational databases, the CAP theorem among other stuff I recommend reading of a great book called [NoSQL Distilled](https://www.amazon.com/NoSQL-Distilled-Emerging-Polyglot-Persistence-ebook/dp/B0090J3SYW/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr=). This book cover initial concepts around NoSQL and is a good starting point.

## What do you mean by key-value?

A key-value database is a database containing a structure to store data similarly to a Map or a Dictionary, where the structure holds a key and this key is an identifier that points to a single record.

Using a simple analogy, this structure is like a hotel reception. When you get to the hotel a key is handed to you, this key contains your room number, and gives you the ability to go into the room. In possession of this key we can always go straight to our room.

{% include image_caption.html imageurl="/images/kv-database/reception.jpg" title="Hotel Reception" caption="" %}

Taking this to the technical field we could think of it as a *[JSON](https://wikipedia.org/wiki/JSON)*:

```json
{
    "rooms": [
        {"306": {"beds": 2, "tv": "LCD"}},
        {"206": {"beds": 1, "tv": "LCD"}}
    ]
}
```

## Ok, but where the database goes in?

If we got here you already understood how key-values work, even may have dealt with Maps or Dictionaries using any programming language, but you could be asking yourself, Where the hell all of this are related to databases?

Well, the answer is that is all related.

According to [Oracle](https://www.oracle.com/database/what-is-database/):

>A database is an organized collection of structured information, or data, typically stored electronically in a computer system

Related to this definition we can point some softwares:

- [Redis](https://redis.io)
- [DynamoDB](https://aws.amazon.com/dynamodb/)
- [Voldemort](https://www.project-voldemort.com/voldemort/)

These are just 3 examples. This is not an exhausting list, if you want to know more implementations of key-value databases you can check this [post](https://en.wikipedia.org/wiki/Key–value_database).

Besides some of these databases are not exclusively key-value, all of them perform this role very well.

Key-Value databases can store data in two ways, In memory or Hybrid.

### In-memory databases

{% include image_caption.html imageurl="/images/kv-database/memory.jpg" title="Memory" caption="" %}

A database configured to operate in-memory stores all of its information on the RAM. Everything in software engineering has tradeoffs and this situation is not an exception.

The main benefit of using in-memory storage is speed. Databases storing data exclusively in-memory don't need to write on disk and memory access is faster than disk access so these are databases with great reading and writing performance.

When we talk about key-value databases the reading of data is even more optimized by the way data is stored. Using a structure similar to a [HashMap](https://www.w3schools.com/java/java_hashmap.asp), using the key we have direct access to the stored information, usually for this operation we have O(1) complexity what is an optimal performance.

