---
layout: post
title: "Implement Singly Linked List in Python"
description: ""
category: Algorithm
tags: [python, data structure, algorithm, linked list]
---
{% include JB/setup %}

A linked list is one of the basic types of data structure in computer science. Conceptually, a linked list is a 
collection of nodes connected by links. This post describes a simple implementation of singly linked list (a node only
knows the next node, but not the previous) in python.

## Implement the Node

A node is single data point in the linked list. It not only holds the data stored, but also has a pointer that can tell 
which the next node is. 

Technically, you can implement the whole linked list only using node. As long as you know which node is the head, you 
can do everything with the whole list, such as add, search, delete, etc.

Here is the implementation
    
    class Node(object):
    
        def __init__(self, data=None, next=None):
            self.data = data
            self.next = next
    
        def get_data(self):
            return self.data
    
        def get_next(self):
            return self.next
    
        def set_next(self, new_next):
            self.next = new_next

## Implement the Linked List

The linked list should include the head of the list, and the api has the following methods:

- size(): return the number of nodes in the list
- insert(data): insert data at the head of the list
- search(data): return the node that has the data, returns None if not found
- delete(data): delete a node with the data, return the node if found, None if not found
- print(): print the whole LinkedList, throw ValueError if data not found


### Size

Size() will return the numbers of the nodes in the list. It iterates through the list from the head and keeps track
of node count.

    def size(self):
        current = self.head
        count = 0
        while current:
            count += 1
            current = current.get_next()
        return count


### Insert

Insert(data) will create a new Node with the provided data, set it as new header, and set its next to the old header. 
In this way, newer data is always at the head.

    def insert(self, data):
        new_node = Node(data)
        new_node.set_next(self.head)
        self.head = new_node

### Search

Search(data) iterates through the list from the head and check the data of each node visited. If the data is found,
the node is returned.

    def search(self, data):
        current = self.head
        while current:
            if current.get_data() == data:
                return current
            else:
                current = current.get_next()
        return None

### Delete

Delete(data) is similar to Search, which iterates through the list and found the node with the provided data.
But it has extra efforts to delete that node and returns it. To achieve this, it keeps track of the previous 
node. If the node to be deleted is found, we set the next of its previous node to its next node, and thus bypassing
the node to be deleted.

    def delete(self, data):
        current = self.head
        prev = None
        while current:
            if current.get_data() == data:
                if current == self.head:
                    self.head = current.get_next()
                else:
                    prev.set_next(current.get_next())
                return current
            prev = current
            current = current.get_next()
        return None

### Print

The Print() is a helper function to display the whole list

    def print(self):
        lst = []
        current = self.head
        while current:
            lst.append(str(current.get_data()))
            current = current.get_next()
        print('->'.join(lst))

----
The complete implementation can be found at <br/>
<https://github.com/ZachLiuGIS/Algorithm-Enthusiasts/blob/master/algorithms/data_structures/LinkedList.py>
