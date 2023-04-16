# REST API

## What is REST?

REST is an acronym for **RE**presentational **S**tate **T**ransfer. It is an architectural style for **distributed hypermedia systems** first introduced by Roy Fielding in 2000 in his famous [dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm). At its core there are [6 guiding principles](./GuidingPrinciples.md).

## Resource

The central key concept of REST is the **resource**. Anything that can be named is a resource: a document, a forecast, a physical object (e.g. a person) and so on. REST uses **resource identifiers** (e.g. URI) to identify resources involved in a certain interaction.

> **Note**: a **resource** is not state of something at certain moment in time. A **resource** is the mapping that exists between a resource identifier (e.g. URI) and a set of **entities**. 
<br>
<br>
That means:
<br>
<br>
`https://api.example.com/user-management/users`  show that is URI is linked to a collection of users and not what that collection would be in certain moment in time. 

Certain guidelines must be followed when **naming resources**, you can find them in the [resource naming guidelines document](./ResourceNamingGuidelines.md)

## Resource Methods

Another key concept in REST is *resource methods* to be used to perform the desired state transition. People wrongly assume that **resource methods** are the same as **HTTP** **GET/PUT/POST/DELETE** methods.

Roy Fielding has never mentioned any recommendation around which method to be used in which condition. All he emphasizes is that it should be uniform interface. If you decide HTTP POST will be used for updating a resource – rather than most people recommend HTTP PUT – it’s alright and application interface will be RESTful.

> **NOTE**: However, we use largely accepted recommandations when developing REST APIs. You can find them in the [resource methods guideline document](./ResourceMethodsGuidelines.md)

## JSON REST API

We use JSON when developing REST APIs. You can find more information in the [json rest api document](./JsonRestApi.md)