---
layout: post
title:  json-size-explorer helps you discover what contributes to JSON document size
date: 2018-07-08 19:00:00
---

When trying to improve the performance of an application, one rule is to do less.
When working with JSON documents, doing less means reducing their size. By being smaller, they are faster to serialize, and faster to deserialize. They take less memory, and less time to be transferred.

Sadly, reducing the size means changing the structure, which also means changing the document's consumer code. 
For example, you can shorten the keys you are using, or try to reduce duplication.

But how do you know what is duplicated? How do you know which key to rename, and what the potential win would be? To answer these questions, I wrote a small CLI tool: [json-size-explorer](json-size-explorer).

Feed it a JSON file, and it will print a bunch of statistic about it:

- number of keys
- size taken by the keys
- most frequent key
- key taking the most space
- most frequent value
- biggest value
- duplicates

The current output is kind of ugly:
![json-size-explorer-1](/img/2018-07-08/json-size-explorer-1.png)
![json-size-explorer-1](/img/2018-07-08/json-size-explorer-2.png)

But you can see that for this 3+mb file, more than 30% of it are just keys and one specific key/value is repeated 3,116 times, taking more than 16% of the total file size.

Pull requests are welcome.
