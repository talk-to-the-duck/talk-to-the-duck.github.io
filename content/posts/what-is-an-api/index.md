+++
title = "Introduction to APIs"
slug = "what-is-an-api"
description = ""
date = 2023-07-06T20:23:00+02:00
featured = false
draft = false
comment = true
toc = true
reward = true
pinned = false
categories = [
"API"
]
tags = [
""
]
series = [
"Working with APIs"
]
images = []
+++

This is the first article about APIs, HTTP APIs and Open API.

I will start placing a common ground about what a generic API is, and why an API is important. Because an API is a complex artifact, it’s important to show its most important features.

Then, I will focus on HTTP APIs and how to be represented them, and then I will explain why instead of creating your own way of documenting an API, you want to use a standard way of representing HTTP APIs which is called Open API.

<!--more-->

# Introduction

This is the first article about APIs and Open API.

I will start placing a common ground about what an API is, and why an API is important. Because an API is a complex artifact, it’s important to show its most important features.

Then, an API has to be represented, and I will explain why instead of creating your own way of documenting it, you want to use a standard way of representing a http API which is called Open API.



# Basic concepts

To make it possible for communication between two distinct software components a well defined way has to be defined.

A service is a software program that makes its functionality available via a published technical interface, called service contract.

A service contract can be broken down into a set of service capabilities. Each service capability is a function offered to other software programs called service consumers.

Any piece of software that uses a service is called a service consumer. Also, the server side is called producer.

Components can be deployed in the same processes, or in different processes. If they are in different processes, they can run on the same machine or on separate ones.

The API we are interested in are those for components running in separate processes, separate machines. Our case of interest is distributed computing.


# What does API mean?

API is the acronym for Application Programming Interface.

There isn’t an official definition of API. Still, on the internet, we can find many definitions.
Some very important software enterprises give the API definition in their own terms.

Amazon Web Services says:
_APIs are mechanisms that enable two software components to communicate with each other using a set of definitions and protocols._

IBM says:
_An API, or application programming interface, is a set of defined rules that enable different applications to communicate with each other._

Mulesoft says:
_An API is a software intermediary that allows two applications to talk to each other._

Redhat says:
_An API is a set of definitions and protocols for building and integrating application software._
All the definitions agree on communicating between two pieces of software, and for a communication to become reality, computers need a protocol which is a set of well-defined rules.
Two applications that want to communicate have to establish a common language. Adhering to an API is the same thing as signing a contract: the clearer the contract, the better.

Today, the most popular APIs are REST and SOAP but you might be interested to the power of GraphQL as well.

If you are into Event-Driven Architectures (EDA) you should have a look at AsyncAPI.
AsyncAPI is an open source initiative that seeks to improve to make working with EDAs as easy as working with REST APIs.

# Why should we care about API?

APIs allow many systems to dialog. If the system is complex enough that it has to be distributed, then well defined APIs are key to simplifying service interconnections. Applications that dialog with other applications are the norm today.
Also, the big majority of all the monolithic applications doesn’t work in a standalone fashion. For example a small web application that needs to authenticate users, most of the time does that via a service called Identity Provider, and such component exposes an API.

Once that API is established both the producer and the consumer can be implemented in any language and independently the one from the other.


# Characteristics of an API
In no special order, an API should be:

**Secure** - Security is the foremost characteristic an API has to present. An API allows reading and/or modifying data from the data. You have to be sure that only authorized people or systems can do that. An API has to assure that data in transit cannot be read by third parties. Authentication, authorization, and encryption are key.

**Stateless** - each request has to contain all of the information necessary to be understood by the server. The server doesn’t store any details about the client session, and the client cannot make the assumption that a request depends on a previous request. The behavior is similar to the one of a function compared to the one of an object: a function called with the same set of arguments will always run with the same result. An object instead can execute the same thing every time.Robust - an API can potentially be used by many concurrent users. Performances shouldn’t vary too much with different loads.

**Well documented** - An API can be (and generally is) a complex artifact. Its use should be as easy as possible and developers should be able to understand how to use it simply by reading the documentation. Errors and their meaning should be well documented too.

**Resilient** - Errors can happen. When the backend incurs any error, the problem should be clearly explained to the caller, with well coded error messages so that, if the fault is the caller’s one, it can amend the call and try again.

**Interoperable** - an API should ease communication between two different systems. This entails that communication protocol and encoding should be clearly defined.

**Discoverable** - in an organization many APIs can be present. If an API cannot be found, it doesn’t exist. Because of this, the organization could start creating a new API (to find traces of the existing one along the way) or even give up the project.

**Consistent** - the endpoints of an API follow similar patterns. For example, the logic of reading a list of customers shouldn’t be that different from reading a list of articles.

# How to represent an API?
Nothing prevents you from creating your own API format, but you don’t want to do that because you want an API to make it possible and easy for two systems to communicate with each other.
Communicating is hard: systems are heterogeneous, parameters and payloads have to be passed back and forth, security is a big concern. There is a massive number of possible representations of an API. Expressing neatly all the aspects of an API requires a well-defined formalism, and it’s definitely a challenge. Thus, before embarking on the creation of a language to represent an API, you want to check if a standard exists.

As a matter of fact, for HTTP APIs such a standard exists, its name is Open API Specification.

What is the OpenAPI Specification? From the official site:

“The OpenAPI Specification (OAS) defines a standard, language-agnostic interface to HTTP APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection.”

Please note that OpenAPI covers http APIs in general, and it also includes support for REST APIs.

The OpenAPI Specification is not limited to represent an HTTP API, but also has a well established ecosystem.
Software tools can be used to translate an OpenAPI definition into browsable documentation, to generate code for the client side and the server side in many programming languages. Automatic tests can be generated as well.

Depending on the functional needs you are facing, and the technical environment you are working on, you have an array of choices of API standards. Some of today's most widely used API protocols other than REST are SOAP and MQTT. OpenAPI covers http APIs in general, and it also includes support for REST APIs.
OpenAPI is not the only way to model http APIs, RAML and API Blueprint are some alternatives.

# Conclusion
In this introductory article, I outlined some basic concepts about communication between software components.
To make such communication possible, an API has to be created. A good API should have some features such as security, interoperability and consistency. After this conceptual section, I introduced the problem of how to represent an API and suggested that for HTTP APIs, a standard such as OpenAPI is a good choice. _RAML_ and _API Blueprint_ exist too but are less known.

In the next article, I will dive into many more details about OpenAPI.

If you have any comments on the content, the form you can leave a comment... it is by exchanging that we progress.

Author: Daniele Cremonini
Reviewer: Emmanuel Quinton
