---
num: "Lecture 15"
desc: ""
ready: false
lecture_date: 2025-05-27 15:30:00.00-7:00
---

* [Slides folder]({{ site.slides_url }}){:target="_blank"}

---

---

## Final Exam Logistics

**Question:** 
On the final exam will we have to memorize things like which one is case 1 case 2 and case 3?

**Answer:** 
I will label them as such but there will be the starter code for each so you can recognize which is which.

## Merge sort

Merge sort uses extra memory space, unlike quicksort. The quicksort algorithm is easier to implement by selecting the first element as the pivot and then partitioning the list based on whether each element is greater or less than the pivot.

## Binary Search Tree class

**Question:**  
How common is it to use `self` without any attribute attached, and what does it mean in the context of the `TreeNode` class?

**Answer:** 
In this case, when we refer to `self`, we're primarily working within the context of the `TreeNode` class. The expression `self` without any attribute simply refers to the current instance of the class — in this case, a particular tree node.

For example, when evaluating a condition like `self.parent.left == self`, we're checking whether the left child of the current node's parent is the node itself. This is a common pattern when working with trees: we often want to determine whether a node is the left or right child of its parent.

This kind of comparison works because both `self` and `self.parent.left` refer to instances of the same class (`TreeNode`). Since Python compares objects by identity unless we override the equality operator (`__eq__`), this comparison will return `True` only if both references point to the exact same object in memory. This is known as **shallow equality** and is standard in many tree-based algorithms.

However, if you tried to compare instances of different classes — for example, a `TreeNode` and a `BinarySearchTree` — the comparison would return `False` unless the `__eq__` method was explicitly overloaded to define how those objects should be considered equal.

One point of confusion may come from the lab, where the `TreeNode` constructor only includes two attributes. In practice, attributes like `left`, `right`, and `parent` are often declared within the constructor or initialized later. Even if they’re not part of the constructor’s parameters, they can still be added as attributes of the object afterward.

This pattern — using `self`, comparing nodes by identity, and setting additional attributes like `left`, `right`, and `parent` — is very common in tree implementations.

In the lab, we were asked to include two attributes that do not normally belong to a basic `TreeNode` class — namely, `key` and `payload`. These represent the data stored in the node but are not what makes something a "tree node" in structure.

What fundamentally defines a tree node are the attributes:
- `left` (left child)
- `right` (right child)
- `parent` (reference to parent node)

These are structural components and must be part of the `TreeNode` class. In this lab, it is assumed that those attributes (`left`, `right`, `parent`) already exist, even if we don't see them explicitly initialized in the constructor.

## Initializing with Default Values
We typically initialize `left`, `right`, and `parent` to `None` in the constructor using default parameters or by explicitly assigning them inside `__init__`. For example:

```python
class TreeNode:
    def __init__(self, key, payload):
        self.key = key
        self.payload = payload
        self.left = None
        self.right = None
        self.parent = None
```

**Question:** 
Why are attributes like `left`, `right`, and `parent` in the `TreeNode` class treated like boolean values in conditionals?

**Answer:**  
In Python, when we write something like `if self.left:`, we're checking whether `self.left` refers to an actual object or not. If `self.left` is `None`, the condition is considered false and won’t run the block underneath. If it holds a valid object — meaning there is a left child — then it counts as true, and the code will execute.

This kind of behavior works with more than just tree nodes. Any value in Python that is considered "empty" — like `None`, an empty list `[]`, or an empty dictionary `{}` — is treated as false when used in an `if` statement. So if an attribute like `left` or `right` hasn't been assigned anything (and is still `None`), it will be treated as false.

Example: `isLeftChild` method

Let's say we have a method like this:

```python
def isLeftChild(self):
    return self.parent and self.parent.left == self
```
---

**Question:**
When we say "the closest largest value" in the context of a tree or search algorithm, do we mean the closest in terms of numerical value or position in the tree?

**Answer:**
We mean the closest numerically, not position-wise.

For example, if we are searching for the closest larger value to 12 in a binary search tree, and the tree contains the values 5, 10, 13, and 20, the closest larger value would be 13, because it is the smallest number greater than 12.

We are not referring to how close the node is in terms of tree levels or structure, but rather how close the value is based on the number itself. So the idea of “closest” here is conceptual, based on the value we are comparing to — not based on where the node is located in the tree.

**Question:**  
Why is the code different when we're dealing with the root node, and do we need the parent when replacing a node's value?

**Answer:**  
If the node we're replacing or removing is the root, there is no parent to update. That’s why the code has to handle this case separately — there’s no `parent.left` or `parent.right` to change. Instead, we directly update the `self.root` attribute to point to the new or replacement node.

For all other nodes, we do need access to the parent when replacing or removing a node. This is because:
- We need to update the parent’s reference to point to the new or replacement node (fixing the correct child link),
- And if the replacement node is not `None`, we also need to update the replacement node’s `parent` pointer to refer to the current node’s parent.

---

**Question:**  
What if the node we are trying to replace has a left child but no right child?

**Answer:**  
If a node has a left child but no right child, and we are trying to remove or replace it, the left child can simply take its place. We update the parent to point to the left child, and update the left child's parent pointer if necessary. There’s no need to search for a replacement node — the left child becomes the replacement.

**Question:**
What if the node we are trying to replace has no left child but does have a right child?

**Answer:** 
If the node has no left child but does have a right child, the right child takes its place. For example, if we are replacing node 7 and it has no left child, but its right child is 11 (and there is no 9), then 11 would replace 7 directly. This works because there are no smaller values on the left that we need to consider — the right child becomes the next valid node in the tree structure.

**Question:**  
Why do we go to the right and then all the way to the left when looking for a replacement node?

**Answer:**  
We do this to find the closest largest value (also called the in-order successor). When deleting a node that has two children, we go one step to the right (to the right child), and then all the way to the left (to the smallest value in that right subtree). This gives us the next node in sorted order — the smallest value that is larger than the one we are removing.

---

## Deletion cases

**Question:**  
What is happening with the underlying Python list when you delete elements, and how does this relate to binary search trees?

**Answer:**  
When you delete elements from a Python list, Python shifts all the elements that come after the deleted item one position to the left to fill the gap.

However, in our case, we’re working with binary search trees, which are non-linear data structures. That means the data is not stored in a single continuous list or array. Instead, each node is connected to its children and parent through references , not positions in a list.

**Question:**  
Is this the base case?

**Answer:**  
Yes, this is a type of base case. If the root is the value we want to delete and the tree only contains that one node, then deleting it means setting `self.root` to `None`. 

At that point, the entire tree becomes empty, and there are no more nodes to process. This is the simplest scenario — deleting the root when it has no children — and serves as a clear base case.


