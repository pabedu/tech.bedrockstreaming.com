---
layout: post
title: "GopherCon UK 2023"
description: My feedbacks from GopherCon UK 2023
author: pa_bedu
tags: [conference, london, tech, golang, go]
color: rgb(251,87,66)
thumbnail: /images/posts/gopherconuk2023/F6317813-48CF-4DA1-AA50-F26BD7093F35.jpg
---

Hello there! My name is Pierre-Alain, I'm a senior developer in the backend teams at Bedrock and I had the great opportunity to go in London for the GopherCon UK.

I travelled by train from Lyon to London, it was a good way to reduce my carbon footprint for this trip but not a good way to reduce cost, thanks Bedrock for allowing me to do that <3

## What is GopherCon UK ?

GopherCon UK is a three day event with one workshop day (16th August) and two multi-track conference days (17th and 18th), held at The Brewery, in the heart of the City of London.

With over 500 delegates, speakers, and sponsors, GopherCon UK aims to deliver fantastic up-to-date content about Go programming and related technologies in a comfortable and professional setting. There are countless networking opportunities to engage with international speakers and delegates, making the event one you won't want to miss.

## Workshop
It was my first time participating to a workshop in an event like GopherCon.
### Practical GO for developers
because of unicode you can't len, use RuneCountInString

`fmt.PrintF("%#v, #vi, (%T)", a, b, b)` for debugging

"" -> string !
`` -> raw string ! useful when \

concurency
https://twitter.com/jordancurve/status/1108475342468120576?lang=en

```go
	for i := 0; i < 5; i++ {
		/* BUG i = 5 all the time, will be fixed in the future by go team
		go func() {
			fmt.Printf("gr:%d\n", i)
		}()
		*/

		//fix 1
		/*go func(n int) {
			fmt.Printf("gr:%d\n", n)
		}(i)*/

		// fix 2
		i := i
		go func() {
			fmt.Printf("gr:%d\n", i)
		}()
	}
```
Goroutine semantics
- send/receive will block until opposite operation (*)
    - buffered channel of cap n has n non-blocking sends
- receive from a closed channel will return the zero value without blocking
  - you can use `val,ok := <- ch` as second left variable to know if value comes from channel or not
- send or close to a closed channel will panic
- send or receive on a nil channel will block forever...

doc.go
used to add documentation to packages
https://gitlab.com/353workshops/shipt22/-/blob/main/nlp/doc.go?ref_type=heads
Readme.md
give instructions to devs to use this project

example_test.go
give example tests to other developers

```go
package nlp_test

import (
  "fmt"

  "github.com/shipt/nlp"
)

func ExampleTokenize() {
  text := "Who's on first?"
  tokens := nlp.Tokenize(text)
  fmt.Println(tokens)

  // Output:
  // [who on first]
}
```

nlp_test.go

`testify/require`
`testify/mock`
`testify/suites`

## Day one
### Memory Management in Go: The good, the bad and the ugly

### Scaling Coffee with Goroutines (2 hour tutorial)
step by step tutorial !
- a lot of coffee
- a lot a people
- as fast as possible

### coffee actions
- take payment
- steam milk
- make espresso

1 customer to serve -> 2s for each action => 6sec
3 customers to serve -> 18sec

## scaling for speed !

### use goroutine

```go
 go MakeCoffee()
 ```

3 customers to serve -> 49microseconds

### use waitGroup

```go
 sync.WaitGroup()

defer wg.Done()

wg.wait()
 ```
but we still have to wait for each coffee to be made

3 customers to serve -> 6s

we can do better, we still do each action to make a coffee one after the other

### more goroutines

each action can become a goroutine

3 customers to serve -> 2s \o/

scaling for load !
docker container / kubernetes deployment

300 customers => OMM killed if too many customers

vertically => more resources !

300000000 customers => OMM killed

we could keep adding resources, but it will cost money !

horizontally => more pods/containers

2 pods => 3000 customers each => 2sec each

split responsibilities in different pod (payment, milk, espresso)

### Logic Programming in Go

Epic games, Verse for meta verse
Erlang
The reasoned schemer

### The 7 Deadly Sins for Gophers
Lush
Dont rush things, going to quick in production. With concurrency, simply adding go keyword
Use go routines only when needed. 

Wrath
Using panic instead of returning error
« Using a wall to stop a car instead of the brakes »
Defensive code
« Must » method must be used outside of user runtime, use them at start up

Greed
Future proofing  everything !
Don’t over engineering things 

Sloth
Not commenting the why not the what it does
Returning error, add context to your errors. Semantic context. 
Fmt.errorf(« this did not work : %w », err)

Gluttony
Using framework when not needed !
Vulnerability can happen. 
Start simple
Only libraries with good support  

Envy
Dont use pattern if not needed
Interfaces are not needed all the time !
Effective Go !

Pride
Assuming you know what’s best !
Dont artificially restrict access to your API

Last 8th sin… Complexity
You tried to do your best. 


### Zero Trust Security for your APIs
Rome was not built in a day
Zero trust security 
Trust no one and always verify. 
Least privilege and deny by default
Full visibility and inspection
Centralised management system

Perimeter based approach -> badge at the door
Zts -> badge at every doors



---
### PARTY !

---

## Day two
### State of the Go Nation!
Product manager
07 start
09 opensource
12 1.0

Go 1 program will contnue to work for all Go 1.*

2015 refinement 1.5
Compiler in go
New garbage collector
Semi-annual release

2018 ecosystem
Go modules -> go module caching and checksum. Secure platform. 

When will go 2 happen ?
Never ! 

What about today ?
Go users has grown *4 since 2018
Go users are satisfied

What’s next ?
Loop var fix
Onboarding
Vulnerability management

CI
Govulncheck

### Efficient Debugging and Logging with OpenTelemetry in Go

How to debug ? 
Logs all the things !
Locally or remotely 
Metrics, logs and traces
Logs are for your future self left by your past self :)

Logs
Never have the good tags, never there when you need them
Not ideal for understanding end to end processing. 

Distributed tracing
A view of lifespan of a request
Good tool to debug production issues
End ti end 

Open telemetry
Unified approach for metrics, loggings and tracings

In Go

### The Hacker's Guide to JWT Security
Caveat of JWT
How to prononce JWT ? Jot in RFC 7519
JWT are only encoded not encrypted 

#1 none algorithm
Backend must check algorithm before checking claims
RFC problem, implementation problem, application dev problem.
How to solve ~> verify algorithm

#2 HS256 password/key cracking
Only one token needed
Offline ~> no communication with server. We cant know. 

#3 man in the middle
Unsecured internal network

#4 xss to steal a Token
XSS deal with it !
Use cookies instead of localstorage

### How NOT to Write a Test in Go
Simple dont write test ! Done !

￼
Why testing ?
Find bugs
Increase trust, avoid regression
Documentation in go

Testing in go 
Fixtures should be placed in  testdata directory

# structured test
Table driven test
￼

Parallel test
T.parallel()
Careful about go gotcha loop

Test suites
testing.M
TestMain method,
t.cleanup(callable) ~> works on panic

Categorinzing tests
Build tags //
Or ENV better cause you know you skipped some tests

-race on go test 

Mocks
Embedding interfaces

Randomising test executions 
Shuffle flag ~> seed value to reproduce error on CI

Benchmarks
testing.B
Exclude by default -bench to add them
Reset timer after setup :)

Test automation

Code coverage

### Make developers fly; Principles for platform engineering
picture

### Incident Management - Talk the Talk, Walk the Walk
Set of procedure to solve this project 

Everything fails all the time 

Mindset
Think about business 

Incident flow
During incident
On call traits
Being proactive


War room conduct