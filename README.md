# DAPI
DAPI is a proposal of style implementation for REST API.

* [Introduction](#introduction)
* [URL](#url)
* [Methods](#methods)
* [Resource](#resource)
* [Errors](#errors)
* [List system](#list-system)
* [Batch](#batch)
* [Search](#search)

# Introduction
A REST API is an API organized around resource. A ressource could be every kind of data. Their is two kinds of ressource : Resource which represents an unique item, and resource which represents list of items.

# URL

# Methods

## Introduction
A REST API is based on HTTP standards Methods : GET, POST, PUT, PATCH, DELETE, OPTIONS, HEAD... 

The method have a standard utilization :
* GET : Read a resource.
* POST : Create a resource.
* PUT : Create or replace a resource.
* DELETE : Delete a resource.
* PATCH : Edit a resource.


Detailled utilisation bellow :

## GET
The main et only utilization of a GET is to read a resource. The only two utilizations of a GET resource is detailled bellow :
* GET /resources : Get a list of items resources
* GET /resources/__id__ : Get the item resources which have the targeted __id__

A GET is idempotent : "the property of certain operations in computer science that can be applied multiple times without changing the result beyond the initial application." (Wikipedia). A GET query does not change the resource state. If the query is applie multiple times, the result do not evolve (only if another method change the resource state of course). 

## POST
This method is used to create a new resource. This method is not idempotent. The main utilization of a POST is to create a resource :
* POST /resources : Create new resources.

## PUT
This method is used to create or replace a resource. This method is idempotent. The two utilizations of a PUT is :
* PUT /resources : Create OR replace a list of items resources. *Not mendatory*. Could  be dangerous.
* PUT /resources/__id__ : Create the resource __id__ OR edit the content of the resource __id__ (replace it).
For small edition, prefer the utilization of the PATCH method.

## DELETE
This method is used to delete a resource. This method is idempotent. The two utilizations of a DELETE is :
* DELETE /resources : Delete a list of items resources. *Not mendatory*. Could be dangerous.
* DELETE /resources/__id__ : Delete the resource __id__.

## PATCH
The patch method is not idempotent. His one the hardest method to implement. The two utilization of a PATCH is :
* PATCH /resources : Edit a list of items resources. *Not mendatory*
* PATCH /resources/__id__ : Edit the resource __id__. *Not mendatory*

# Resource
A resource could have differents formats. The only mendatory format is json. But you could add new format for you resource : YAML, XML, XLS, Raw txt...

For the generic explanation, we use the JSON format.

## Simple resource
A resource is JSON object, with an _id field for identifie it :
```
{
  _id: ItemUniqueID,
  anAttribute: DATA,
  anObject: {
    anotherAttribute: 33,
    anArray: [3, 4, 5]
  }
}
```

## Envelope
A resource could be in an envelope. And envelope have two mendatory fields : metadata and result. The result field contains the item, the metadata contains the envelope informations :
```
{
  metadata: {
    DATA ABOUT THE QUERY, THE RESOURCE, THE CONTEXT...
  },
  result: THE_RESOURCE
}
```

## List resource
The main utilization of envelope is about list resource. A list is an array, but by convention we never answer with an array. It's easier for the client to always read JSON object.

The metadata mendatory fields in a standard query list is : 
* status : Status of the request (utilized in error context : SUCCESS or ERROR).
* skip : Number of element ignored (for pagination system).
* limit : Number of element to get in the list (for pagination system).
* total : Total number of element in the list.

```
{
  metadata: {
    status: 'SUCCESS',
    skip: 0,
    limit: 2,
    total: 40
  },
  result: [
    {
      _id: item1
    },
    {
      _id: item2
    }
  ]
}
```

# Errors
An error have a code HTTP and a body which contains a resource item.

The standard error have the same code errors of http (40x or 50x).

Errors 40x are caused by the client.

Errors 50x are caused by the server.

## Basic error
A basic errors have three mendatory fields : 
* error : The name of the error (In uppercase, could be standard HTTP errors : NOT_FOUND... Or custom errors : COULD_NOT_DO_THAT).
* techMsg : A technical message for help a support member of the team to resolve the problem.
* clientMsg : A client message which can be displayed to the user (in english !)

```
{
  error: 'NOT_FOUND',
  techMsg: 'Resource 4343 does not exists',
  clientMsg: 'We currently can not reach the resource 4343, try again later'
}
```

## List error
In a list context, the status field of the metadata field could be used :
```
{
  metadata: {
    status: 'ERROR'
  },
  result: {
    error: 'BAD_REQUEST',
    techMsg: 'Could not do that',
    clientMsg: 'We have a problem with your request, try again later'
  }
}
```

# List system

# Batch

# Search


