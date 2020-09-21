---
title: Python Parallelism and Concurrency
date: 2020-09-18 23:31:17
tags:
---


## What is asyncio?

* asyncio is a library to write concurrent code using the async/await syntax 
* asyncio is often a perfect fit for IO-bound and high-level structured network code  --> good for lots of IO work
* if your code is CPU-bound consider using multiprocessing library instead --> for example, finding primes

concurrent --> task, workitems that can potentially run in parallel, but not necessarily well
  
effectively doing one thing at a time. 


## Async and Sync Calls 

1. Synchronous:
    If an API call is synchronous, it means that code execution will block (or wait) for the API call to return before continuing. This means that until a response is returned by the API, your application will not execute any further, which could be perceived by the user as latency or performance lag in your app. Making an API call synchronously can be beneficial, however, if there if code in your app that will only execute properly once the API response is received.

2. Asynchronous:
    Asynchronous calls do not block (or wait) for the API call to return from the server. Execution continues on in your program, and when the call returns from the server, a "callback" function is executed.

