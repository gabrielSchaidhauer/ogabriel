---
layout: post
title: "Key-Value Databases"
description: "Understanding what they are where they live and what they feed on"
date: 21/01/2021
feature_image: images/kv-database/lockers.jpg
language: en-US
tags: [data%en-US%, nosql%en-US%]
---
Key-Value database is a term that defines several NoSQL databases with similar characteristics, but not all of them work the same way. Here I will explain some of these characteristics without going too deep on any specific feature.
<!--more-->

If you want to know a little more about NoSQL in general, the distinction between them and relational databases, the CAP theorem, etc. I recommend reading a great book called [NoSQL Distilled](https://www.amazon.com/NoSQL-Distilled-Emerging-Polyglot-Persistence-ebook/dp/B0090J3SYW/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr=). This book covers initial concepts around NoSQL and is a good starting point.

## What do you mean by key-value?
A key-value database is a database containing a structure to store data similarly to a Map or a Dictionary, where the structure holds a key, and this key is an identifier that points to a single record.
Using a simple analogy, this structure is like a hotel reception. When you get to the hotel a key is handed to you, this key contains your room number and gives you the ability to go into the room. In possession of this key, we can always go straight to our room.

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

Related to this definition we can point some software:
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

When we talk about key-value databases the reading of data is even more optimized by the way data is stored. Using a structure similar to a [HashMap](https://www.w3schools.com/java/java_hashmap.asp), using the key we have direct access to the stored information, usually, for this operation, we have O(1) complexity what is optimal performance.

As we say [there ain't no such thing as a free lunch](https://en.wikipedia.org/w/index.php?title=There_ain%27t_no_such_thing_as_a_free_lunch&redirect=no) so there are also downsides to this approach.

Since all the data is in memory and RAM is volatile if there is any kind of failure like a power outage or any other the data could be entirely lost.

Another important aspect is the amount of memory available on servers. Usually, we have a lot less memory than disk storage, and memory is not that easy to scale. So, when using this kind of approach we get limited by the amount of memory available.

### Hybrid Storage

{% include image_caption.html imageurl="/images/kv-database/bookshelf.jpg" title="Bookshelf" caption="" %}

These databases usually don't work exclusively with disk storage. Focusing on the performance they commonly choose a hybrid approach. This works by storing all the information on disk but keeping some loaded for fast access.

What is or is not kept in memory depends on the database configuration and, usually, can be customized accordingly to each use case. It can be configured to keep only more recent records or more accessed records in memory.

Using this kind of storage we are not limited by memory anymore, having a lot more space to store data. Also, when using disk storage we don't need to worry about data loss due to outages, if an outage happens the server can reload the data from disk to the memory based on his policies. The main downside of this approach is that 
when the use case doesn't match the configured conditions the time to load the data from disk could be added to the operation.

Excluding specific use cases where you are willing to lose data, the hybrid approach is preferred. In the last couple of years, the price and performance of disk storage have increased, especially with the advent of SSD technologies, which makes the downsides of the hybrid approach get less and less compelling.  

## Modeling

{% include image_caption.html imageurl="/images/kv-database/modeling.jpg" title="Modeling" caption="" %}

If you already know relational databases you usually end up modeling the data at first and then you think on the queries after, as they are needed. It happens because on relational databases we have an infinity of ways of searching and fetching data, which does not happen on most NoSQL databases.

This fact is especially true to key-value databases. Its ideal model is when we can have previous knowledge of the key, without having to search for it. On most key-value databases, we cannot search for data on the stored value. To better understand let's see an example. Imagine a library where you can always find a book if you know it's bookcase and shelf.

```json
{
  "library": {
      "A10:3": [
          {
              "title": "Harry Potter and the Order of the Phoenix",
              "author": "J.K. Rowling"
          },
          {
              "title": "Harry Potter and the Philosopher's Stone",
              "author": "J.K. Rowling"
          }
      ],
      "A10:12": [
          {
              "title": "The Hobbit",
              "author": "J.R.R. Tolkien"
          },
          {
              "title": "The Lord of the Rings: The Fellowship of the Ring",
              "author": "J.R.R. Tolkien"
          }
      ]
  }
}
```

If we know the bookcase and the shelf we can locate all the Harry Potter books available. However, we cannot find on which shelf the books of J.R.R Tolkien are.

This shows us that using this kind of database we need to think carefully about the modeling of our data already thinking about how the data will be queried. We must define very well our keys since we must have the ability to easily find them.

Another important aspect of the modeling is the definition of the aggregate, but, since this is not a specific aspect of key-value databases it will not be explained in this post. If you want to know more about it you can read [here](https://arleypadua.medium.com/why-keeping-aggregates-with-nosql-9e1d9f9920f2).

## When you should use it

As we have seen, key-value databases, in general, have an access pattern where you should know the key that leads you to your data. This access pattern creates situations where key-value databases are not the best fit. 

Imagine listing products, but to list the products we would end up needing to fetch from the database each of the product details, this would be a good fit? It seems the combination of bad modeling and a use case that doesn't match the database.

Commonly, this kind of database should be used to store data that fit as a unity, such as user session, user preferences, shopping carts, etc.

Obviously, it is relative to your business area and also your system modeling, in your system it may be for example that a product list would be associated with a specific key, but we need to be always aware of how we need to fetch the data and the database limitations.

## Conclusion

Key-Value databases are powerful tools and won't be I who will escape from the cliche of saying that with great power comes great responsibility. Working with this kind of database we need to evaluate carefully if this is the right choice for our use case, what kind of storage we are going to use, if it offers more than one if we know how our modeling will be, etc. Even with all the limitations, these databases provide great advantages in terms of performance and scalability, so if you still haven't it is worth a try.

If you want to leave a comment telling me what you think, let's chat.

Happy Coding!
 
 
 

 

