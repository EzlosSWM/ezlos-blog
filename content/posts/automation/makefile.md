---
title: "Makefiles"
subtitle: ""
date: 2023-06-14T15:58:26+08:00
lastmod: 2023-06-14T15:58:26+08:00
summary: "Makefiles, what are they and how to use them?"

draft: false
author: "Carlos Gomez"
description: "Makefiles, what are they and how to use them?"
license: "MIT"
images: []

tags: ["makefile", "make", "automation"]
categories: ["Automation"]

resources: 
- name: ""
  src: ""

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
# Makefiles
## What are Makefiles & how to use them?
Makefiles is a handy automation tool that can run and compile your code.
*

## Why use Makefiles?
I like to think of Makefiles as remote controls. Each button is a programmed command that would result in a different action. Makefiles consist of a set of commands that can automate the specified tasks.

*Note: `make` and Makefiles only work in the current directory and its children.*

As a Go programmer, I enjoy using Makefile for testing and compiling multiple binaries. Generally, I'd have 4-5 commands:
1. Create a temp folder for the binary. 
2. Navigate to the created folder and run the binary.
3. Delete the temp folder 
4. Runs unit tests

## Hands-on example 
Before we start building our Makefile, let's get acquainted with one.

Before getting started. 

Ensure you have `make` installed. 
```Makefile
# Debian
sudo pacman -S make

# Arch
sudo apt-get install make
```

Go ahead and create a Makefile 

```Shell
touch Makefile
```

Let's start with a simple "Hello, world!" command:

```Makefile 
hello: 
    echo "Hello, world!"
``` 

We preface the echo command with the caller *hello*. Let's see how we can use it: 
```Shell
make hello 

# Output 
"Hello, world!"
```

> *Note: Makefiles are indent-sensitive.* 

Makefiles can also include multiple commands:
```Makefile 
hello: 
    echo "Hello, world!"

bye: 
    echo "See ya later!"
```


```Shell 
make hello
# Output: 
Hello, world!

make bye 
# Output: 
See ya later!
```

Now that we have the basics out of the way, let's get started. 

## Real world example 
Let's set up a simple Go application as an example. 
```Go 
package main 

import (
    "fmt"
)

func main() {
    fmt.Println("Hello, world!")
}
```

As previously mentioned, I have specific commands I enjoy automating, so let's build them. 

1. Compiling the binaries into a bin folder. 
```Makefile 
build: 
   @go build -o bin/app ./main.go
```

2. cd into bin folder and run comand 
```Makefile 
run: 
   ./bin/app
```

3. Delete the bin folder to save space. 
```Makefile 
clean: 
    rm -rf bin/
```

4. Testing
```Makefile 
test: 
    @go test -v ./...
```

Now that we know what commands to run, we can automate them:
```Makefile 
.PHONY: all clean build run test

all: clean build run

build: 
   @go build -o bin/app ./main.go
 
run: 
   ./bin/app

clean: 
    rm -rf bin/

test: 
    @go test -v ./...
```

Notice `all` command. It runs each of the commands synchronously.

.PHONY specifies the name of a command, not a file

Cleaning up
As programmers, we like our code to be clean and flexible. 
```Makefile 
APP_NAME=app
FLD_PATH=./bin
ENTRY_POINT=./main.go

.PHONY: all clean build run test

all: clean build run

build: 
   @go build -o ${FLD_PATH}/${APP_NAME} ${ENTRY_POINT}
 
run: 
  @${FLD_PATH}.//${APP_NAME}

clean: 
    rm -rf ${FLD_PATH}

test: 
    @go test -v ./...
```

There we go, nice simple and easy to use. To call these we would use: 
```Shell
make build 

make run 

make clean 

make test 

make all
```


## Conclusion
*Makefiles* assist with automating the simple yet cumbersome tasks of compiling and running your applications.

## References
[Makefile PDF](https://www3.nd.edu/~zxu2/acms60212-40212/Makefile.pdf)



