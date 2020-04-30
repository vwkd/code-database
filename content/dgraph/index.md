---
title: Dgraph
author: vwkd
index: 4
tags:
---

<!-- ToDo: Finish -->

## Introduction

- simple graph database
- no attributes for relationships
- no labels for nodes



## Implementation

- the database is a simple key-value store, `ID:predicate` - `value`
- each entity is given a unique ID
- each attribute is a predicate of the form `has_noun` with a simple value as value
- each relationship is a predicate of the form `verb` with an ID as value or array thereof
- both attributes and relationships are predicates, except attributes have simple values while relationships have IDs as values
- an entity can only exist with at least one attribute or relationship, otherwise it can't be created in the key-value store
- following relationships is very cheap, since deals only with integers (IDs)
- but needs to build index of nodes by some identifying attribute, e.g. "label", because user doesn't identify nodes by their IDs



## GraphQL+-

- query language modeled after GraphQL
- problems:
  - relationships are represented by nested objects, unintuitive, nested data structure to represent non-nested one
  - selecting nodes to query is in single line at beginning, doesn't scale, should have been integrated with selecting nodes for result via object properties, like setting object properties with the selection parameters
  - single line at beginning is a freely named function call, selection is done in argument to `func` parameter, very unintuitive

### Mutation

- if `set`s using an existing `uid` updates existing entry instead of creating new one
- `uid` is a reserved attribute

```javascript
// creation
{
  "set":[
    {
      attributes
      relationship {
        attributes
        relationship {
          attributes
        }
      }
    }
  ]
}

// update
{
  "set":[
    {
      "uid": "0x1",
      attributes / relationship
    }
  ]
}

// delete
{
  delete {
    <0x1> <age> * .
  }
}
```

### Query

```javascript
// simple
{
  myFuncName(func: funcName(arguments)) {
    attributes
    relationship {
      attributes
    }
  }
}

// recursive
{
  find_follower(func: uid("0x1")) @recurse(depth: 4) {
    name 
    age
    follows
  }
}
```

- if wants to query by attribute, needs to add index for it