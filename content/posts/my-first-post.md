---
title: "CRUD API with Go & Postgres"
subtitle: ""
date: 2023-05-20T15:58:26+08:00
lastmod: 2023-05-20T15:58:26+08:00
summary: "A basic REST API made with Golang and Postgres"

draft: true 
author: "Carlos Gomez"
description: "Golang API"
license: "MIT"
images: []

tags: ["API", "Go", "Postgres", "Database", "Persistence"]
categories: ["Development"]

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

# Go REST API with Postgres

One of my favorite things to work with are APIs and Go makes it super easy to design them. Here we'll be designing a simple CRUD API for storing users and user information. The API will consist of 4 simple routes for *creating*, *retrieving*, *deleting*, and *updating* the users.

## Endpoints & Designs
The endpoints we'll be using are going to be: 

**POST** `/user`: creates a new user

**GET** `/users`: retrieves all users

**DELETE** `/user/:id`: deletes the user with the specified ID

**PUT** `/user/:id`: updates the user with the specified ID  


Now that we've defined the endpoints let's explain how we'll go about designing the API. 


On the grand scheme of things, each endpoint and each HTTP method will be routed to a handler and the handlers will make a call to the database (in this case, Postgres). Now that that's out of the way, let's get started. 

## Requirements 
1. Go installed 
2. Postgres docker container

*Create the folder structure for the project*
```Bash 
# Make the directory  
mkdir user-api

# Navigate to the directory
cd user-api

# Run go mod init
go mod init github.com/<username>/<project-name>

# Download dependencies
go get github.com/gofiber/fiber/v2
go get -u github.com/gofiber/fiber/v2/middleware/cors
go get -u github.com/gofiber/fiber/v2/middleware/logger
go get -u github.com/gofiber/fiber/v2/middleware/recover
go get github.com/lib/pq
```

## Step One (Defining our User Data):
### Models
Our first step would be to start defining how our *User data* should look like. We can do this by using a struct in Go.

