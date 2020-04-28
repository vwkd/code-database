---
title: Graph databases
author: vwkd
index: 3
tags:
---

## Introduction

- stores data in a graph, more like in real world
- relationships are first-class citizens, just as much as entities

- like ER diagram in Introduction

- can visualise data better, pattern, anomalies, etc.
no two nodes need to have the same attributes and relationships, not like in table where columns are fixed, can add over time as learns them
however in the ER model it's fixed

- can easily find meaning in relationships, patterns, etc.
- entity is a node, relationship is an edge, i.e. database stores multiple instances of abstract types, ER model shows structure

- keep attributes on nodes small, put attributes into own nodes, e.g. person with single attribute name, relationships lives in city, has telephone number etc.
-> can't sort graph by attributes, but can visualise how many customers live in city
everything that wants to be able to aggregate by should be an entity, instead of an attribute
use nodes for entities with identity
attributes for properties of the entities
relationships to connect entities
attributes for relationship to specify relationship
label nodes to give roles, group entities

Problems
- When attributes of a node or separate node connected to node
e.g. address is property of User or separate node connected to user?
-> if attribute value is a complex value type pull out into separate node with attributes, e.g. address
-> if want to group, by this attribute, start traversing graph at this attribute, e.g. tags, skills
-> if attribute is in certain relationship with node, e.g. `award` attribute to which wants to add date, etc.
- How to implement time in the graph, versions of the graph at points in time, e.g. prices a week ago, number of friends a year ago, etc.
need to separate state from structure, need to be able to version independently from each other, e.g. structure node changes or state node changes
purely additive, never delete
use "entity nodes" to link to first and last version of that node, each intermediary node links to next,
add `to` and `from` properties to every relationship to limit validness, use EPOCH time, use max EPOCH for `from` even if unlimited to store it together with other attribute on disk
alternatively attach linked list to structure node with versions, attach next state node with timestamp
beware: much more data, need to add every change, queries become more complex
add state node for every attribute except id

- every relationship can have only one start and end node
- use intermediary nodes if wants to connect more than two nodes, e.g. add details to relationship, n-ary relationship, etc.
e.g. `User works at Company` and attach `as Role`, or `Customer buys Product` and attach `for Price` and `using Payment`


- make intermediary nodes if relationship contains standalone information, can use intermediary nodes to build more relationships, e.g. instead of `User reviews Product` use `User writes Review reviews Product`, can add review-specific details to `Review` node
link off from relationship to intermediary nodes

Labels are used to shape the domain by grouping nodes into sets where all nodes that have a certain label belongs to the same set.
A node can have zero to many labels. ????



## Resources

- [Wikipedia - Graph database](https://en.wikipedia.org/wiki/Graph_database)
- [David Bechberger - A Skeptics Guide to Graph Databases](https://www.youtube.com/watch?v=yOYodfN84N4)
- [Neo4j - Tips and Tricks for Graph Data Modeling](https://www.youtube.com/watch?v=78r0MgH0u0w)