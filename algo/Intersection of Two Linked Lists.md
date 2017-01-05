## Description
>Write a program to find the node at which the intersection of two singly linked lists begins.
>
>For example, the following two linked lists:
> >A:        a1 → a2
> >                   ↘
> >                     c1 → c2 → c3
> >                   ↗            
> >B:     b1 → b2 → b3
>begin to intersect at node c1.
>Notes:
>
> * If the two linked lists have no intersection at all, return null.
> * The linked lists must retain their original structure after the function returns.
> * You may assume there are no cycles anywhere in the entire linked structure.
> * Your code should preferably run in O(n) time and use only O(1) memory.

[Leetcode Link](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Methodology
A straight-forward method is story every pointer of *A* in a Hashset. Then traverse List *B* and find the pointer in *A's* Hashset. The time complex of this method is O(n). However, the space complexity is O(n) also, which does not meet the O(1) memory request.

A tricky method is to convert the the problem into list cycle problem. We can reverse the pointer of List *A*, and let the head node(before reverse) of list *A* point to head node of list *B*. Then the two list merged into one list start with *A's* tail(before reverse) represented by *C*. If list *C* exists a cycle, it means list *A* and *B* have intersection and the first node of the cycle is the intersection node. Otherwise, list *A* and *B* have no intersection. However, this method change the structure of list *A*. Of course, we can reverse list *A* again which will restore list *A's* structure.

 Can we find the intersection without change the structure of the two lists? Of course we can. We can count the size of list *A* and *B* as *N* and *M*. Suppose *N > M*, we move *A* *N-M* steps forward. Then we move *A* and *B* forward together once a step util they point to the same element(the intersection) or *NULL*(no intersection).

### Steps

1. Count List *A's* length as *N*.
2. Count List *B's* length as *M*.
3. Assign pointer *pa = A*, pointer *pb = B*.
4. If *N > M*, move *pa* forward *N-M* steps. Otherwise, move *pb* forward *M-N* steps.
5. Move *pa* and *pb* together util *pa* equals to *pb*.
6. Return *pa*(or *pb*) as result. If *pa==pb==NULL*, it means they have no intersection. Ohterwise, means *pa==pb!=NULL* is the intersection node.
