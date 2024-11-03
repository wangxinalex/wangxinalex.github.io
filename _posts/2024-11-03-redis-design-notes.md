---
layout: post
title: "Notes of Redis Design"
date: 2024-11-03 12:28:18 +0800
categories: Redis
---

## Simple Dynamic String (SDS)

```C
struct sdshdr {
    int len;
    int free;
    char buf[];
}
```

### Binary safe

Uses `len` instead of `\0` to judge the endig of a string.

## Linked List

```C
typedef struct listNode {
    struct listNode* prev;
    struct listNode* next;
    void* value;
}listNode;
```

```C
typedef struct list{
    listNode* head;
    listNode* tail;
    unsigned long len;
    void *(*dup) (void* ptr);
    void *(*free) (void* ptr);
    int *(*match) (void* ptr);
}list;
```
