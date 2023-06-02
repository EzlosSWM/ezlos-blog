---
title: "Builder Design Pattern in Go"
subtitle: ""
date: 2023-06-01T15:58:26+08:00
lastmod: 2023-05-20T15:58:26+08:00
summary: "A short explanation of the builder design pattern with an example using Go."

draft: false
author: "Carlos Gomez"
description: "Notes for the builder design pattern implemented in Go."
license: "MIT"
images: []

tags: ["design", "pattern", "go", "notes"]
categories: ["Design Patterns"]

resources: 
- name:  
  src: 

hiddenFromHomePage: false
hiddenFromSearch: false
lightgallery: true  
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  maxShownLines: 50
share:
  enable: true
comment:
  enable: true
---
# Builder Pattern in Go

## Quick backstory
### The Problem
The problem I am facing is that I have a particular workflow for writing Go code, which gets repetitive. Even when working on starting a new project, a general idea, or testing a quick theory, the setup is a bit cumbersome for me. Here is an example of the project structure I typically start with:
```Shell
└── folder1
    ├── main.go 
    ├── go.mod
    ├── README.md
    └── Makefile
```

It felt like a lot to run go mod init github.com/EzlosSWM/<new-project> and touch main.go README.md Makefile every time - let alone boilerplate content for each file. So that is when I decided, in typical programmer fashion, to write a script for generating the project structure. 

*Note: the script and the breakdown will come after.*

I managed to write a simple enough script in Go to get it out of the way, but I knew; I could do better. That’s when I started brainstorming ways to make it scaleable & easier to maintain. I wanted a CLI tool that generates boilerplate Go code for a generic project and eventually scales up to complete projects with different architectural patterns.    

### The Solution
After deciding what I wanted, it was time to do some research on how to go about achieving it. That's when I ran into a video on YouTube titled [10 Design Patterns Explained in 10 Minutes](https://www.youtube.com/watch?v=tv-_1er1mWI&pp=ygUROCBkZXNpZ24gcGF0dGVybnM%3D) by Fireship. I found it interesting how he explained the builder pattern so I did more focused research on it. That's when I decided I was going to use the pattern to build my CLI tool. 

## What is the builder pattern?
Essentially, a builder is a creational design pattern that allows you to create objects (in Go’s case, structs). Encapsulating the logic for creation allows for cleaner and easier-to-maintain code while being flexible. It defines a predetermined sequence of steps to promote consistency and reliability.

Imagine having to build *n* users with different clearance levels. It would be much simpler to have a checklist of data a user needs to have. For example: 
1. setAge()
2. setPhone()
3. setAdminStatus()

### Go example (easy)
1. Initializing the project 
```Go 
// main.go
package main 

import (
    "fmt"
)

func main() {
    fmt.Println("Hello, world!")
}
```

Now just run `go run main.go` to get: 
> $ Hello, world!

2. Defining our interface 

Under our import section, we want to define our builder interface to house our API.
```Go
type Builder interface {
    setAge(int) User
    setPhone(int) User
    setAdminStatus(bool) User
}
```

3. Define our User struct 
```Go 
type User struct {
    name    string 
    age     int 
    phone   int
    isAdmin bool
}
```

4. Define our functions to instantiate our User struct & function
```Go 
// instantiates our User struct
func newUser(name string) *User {
    return &User{
        name: name,
    }
}

// attaches our methods to the User struct 
func (u *User) setAge(age int) *User {
	u.age = age
	return u
}

func (u *User) setPhone(phone int) *User {
	u.phone = phone
	return u
}

func (u *User) setAdminStatus(status bool) *User {
	u.isAdmin = status
	return u
}
```

4. Building our users 

Now that we’ve done most of the heavy lifting, we can start by using our builder to create users quickly and effectively.
```Go
func main() {
	regularUser := newUser("foo").setAge(18).setPhone(123456789)
	fmt.Println(regularUser)

    adminUser := newUser("bar").setAge(20).setAdminStatus(true)
    fmt.Println(adminUser)
}
```

Output: 
```Shell
&{foo 18 123456789 false}
&{bar 20 0 true}
```

Hurray! We just made a User builder.

### Full code example 
```Go 
package main

import "fmt"

type Builder interface {
	setAge(int) User
	setPhone(int) User
	setAdminStatus(bool) User
}

type User struct {
	name    string
	age     int
	phone   int
	isAdmin bool
}

func newUser(name string) *User {
	return &User{
		name: name,
	}
}

func (u *User) setAge(age int) *User {
	u.age = age
	return u
}

func (u *User) setPhone(phone int) *User {
	u.phone = phone
	return u
}

func (u *User) setAdminStatus(status bool) *User {
	u.isAdmin = status
	return u
}

func main() {
	regularUser := newUser("foo").setAge(18).setPhone(123456789)
	fmt.Println(regularUser)

	adminUser := newUser("bar").setAge(20).setAdminStatus(true)
	fmt.Println(adminUser)
}

// Output 
&{foo 18 123456789 false}
&{bar 20 0 true}
```

## Conclusion
The builder pattern is an easy-to-implement creational design pattern, and if implemented correctly - scales easily. In this scenario, we have a builder that creates regular and admin users, but there are multiple scenarios where this would be useful. 

## Reading Material 
[Refactoring Guru - Builder](https://refactoring.guru/design-patterns/builder)

[Refactoring Guru - Builder in Go](https://refactoring.guru/design-patterns/builder/go/example)

[Wikipedia](https://en.wikipedia.org/wiki/Builder_pattern)
