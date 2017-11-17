## Chapter 3
* A pop or top on a empty stack is generally considered an error in the stack ADT. On the other hand, running out of space when performing a push is an implementation limit but not an ADT error.
* Linked List Implementation of Stacks
	* Uses a singly linked list. Perform a push by inserting at the front of the list. Perform a pop by deleting the element at the front of the list. Top operation merely examines the element at the front of the list, returning its value.
* Array Implementation of Stacks
	* Uses the back, push_back and pop_back implementation from vector, so the implementation is trivial. Associated with each stack is the theArray and topOfStack, wheich is -1 for an empty stack (this is how an empty stack is initialized). To push some element x onto the stack, we increment topOfStack and then set theArray[topOfStack] = x. To pop, we set the return value to theArray[topOfStack] and then decrement topOfStack.
* Applications of Stack
	* Balancing Symbols
		* Every right brace, bracket and parenthesis must correspond to its left counterpart. The sequence [()] is legal but [(]) is wrong. Make an empty stack. Read characters until end of file. If the character is an opening symbol, push it onto the stack. If it is a closing symbol and the stack is empty, report an error. Otherwise, pop the stack. If the symbol popped is not the corresponding opening symbol, then report an error. At end of file, if the stack is not empty, report an error.
	* Postfix Expressions
		* ```4.99 * 1.06 + 5.99 + 6.99 * 1.06``` A typical evaluation sequence might be to multiply 4.99 and 1.06, saving this answer as A1. We then add 5.99 and A1, saving the result in A1. We multiply 6.99 and 1.06, saving the answer in A2, and finish by adding A1 and A2, leaving the final answer in A1. We can write this sequence of operations as ```4.99 1.06 * 5.99 + 6.99 1.06 * +```. This notation is known as postix, or reverse Polish notation. The easiest way to do this is to use a stack. When a number is seen, it is pushed onto the stack; when an operator is seen, the operator is applied to the two numbers that are popped from the stack, and the result is pushed onto the stack. Notice that when an expression is given in postfix notation, there is no need to know any precedence rules; this is an obvious advantage.
	* Infix (expression in standard form) to Postfix Conversion
		* Suppose we want to convert the infix expression ```a + b * c + ( d * e + f ) * g``` into postfix. A correct answer is ```a b c * + d e * f + g * +```. When an operand is read, it is immediately placed onto the output. Operators are not immediately output, so they must be saved somewhere. The correct thing to do is to place operators that have been seen, but not placed on the output, onto the stack. We will also stack left parentheses when they are encountered. We start with an initially empty stack. If we see a right parenthesis, then we pop the stack, writing symbols until we encounter a corresponding left parenthesis, which is popped but no output. If we see any other symbol ```+,*,(```, then we pop entries from the stack until we find an entry of lower priority. One exception is that we never remove a ( from the stack except when processing a ). For the purposes of this operation, + has lowest prority and ( highest. When the popping is done, we push the operator onto the stack. Finally, if we read the end of input, we pop the stack until it is empty, writing symbols onto the output. The idea of this algorithm is that when an operator is seen, it is placed on the stack. The stack represents pending operators. However, some of the operators on the stack that have high precedence are now known to be completed and should be popped, as they will no longer be pending. Thus prior to placing the operator on the stack, operators that are on the stack, and which are to be completed prior to the current operator, are popped. Parentheses simply add an additional complication. We can view a left parenthesis as a high precedence operator when it is an input symbol (so that pending operators remain pending) and a low precedence operator when it is on the stack (so that it is not accidentally removed by an operator). Right parentheses are treated as the special case.
	* Function Calls
		* The algorithm to check balanced symbols suggests a way to implement function calls in compiled procedural and OO languages. The problem here is that when a call is made to a new function, all the variables local to the calling routine need to be saved by the system, since otherwise the new function will overwrite the memory used by the calling routine's variables. Furthermore, the current location in the routine must be saved so that the new function knows where to go after it is done. The variables have generally been assigned by the compiler to machine registers, and there are certain to be conflicts (usually all functions get some variables assigned to resigter #1), especially if recursion is involved. The reason that this problem is similar to balancing symbols is that a function call and function return are essentially the same as an open parenthesis and closed parenthesis, so the same ideas should work. When there is a function call, all the important information that needs to be saved, such as register values (corresponding to variable names) and the return address (which can be obtained from the program counter, which is typically in a register), is saved "on a piece of paper" in an abstract way and put at the top of a pile. Then the control is transferred to the new function, which is free to replace the registers with its values. If it makes other function calls, it follows the same procedure. When the function wants to return, it looks at the "paper" at the top of the pile and restores all the registers. It then makes the return jump. Clearly, all of this work can be done using a stack, and that is exactly what happens in virtually every programming language that implements recursion. The information saved is called either an activation record or stack frame.

## Chapter 4
* The binary search tree is the basis for the implementation of two library collections classes, set and map.
* A path from node n<sub>1</sub> to n<sub>k</sub> is defined as a sequence of nodes n<sub>1</sub>, n<sub>2</sub>, ... n<sub>k</sub> such that n<sub>i</sub> is the parent of n<sub>i+1</sub> for 1 <= i < k. The length of this path is the number of edges on the path, namely k-1. There is a path of length 0 from every node to itself. In a tree there is exactly 1 path from the root to each node. for every node n<sub>i</sub>, the depth of n<sub>i</sub> is the length of the unique path from the root to n<sub>i</sub>. Thus, the root is at depth 0. The height of n<sub>i</sub> is the length of the longest path from n<sub>i</sub> to a leaf. Thus all leaves are at height 0. The height of a tree is equal to the height of the root. The depth of a tree is equal to the depth of the deepest leaf; this is always equal to the height of the tree. 
* In a preorder traversal, work at a node is performed before (pre) its children are processed. 
* In a postorder traversal, the work at a node is performed after (post) its children are evaluated.
* A binary tree is a tree which no node can have more than two children. A property of a binary tree that is sometimes important is that the deoth of an average binary tree is considerably smaller than N. An analysis shows that the average depth is O(square root (N)) and that for a special type of binary tree, namely the binary search tree, the average value of the depth is O(logN). Unfortunately, the depth can be as large as N-1.
* Because a binary tree node has at most two children, we can keep direct links to them. The declaration of tree nodes is similar in structure to that for doubly linked lists, in that a node is a structure consisting of the element information plus two pointers (left and right) to other nodes.

e.g.

```cpp
struct BinaryNode {
	Object element; //the data in the node
    BinaryNode *left; //left child
    BinaryNode *right; //right child
};
```
* The general strategy (left, node, right) is known as an inorder traversal. An altrenate traversal strategy is to recursively print out the left subtree, the right subtree, and then the operator. This traversal strategy is generally known as a postorder traversal. A third traversal strategy is to print out the operator first and then recursively print out the left and right subtrees. This traversal strategy is a preorder tarversal.
* The property that makes a binary tree into a binary search tree is that for every node X in the tree, the values of all the items in its right subtree are larger than the item in X.

e.g.

```cpp
/*
* Internal method to test if an item is in a subtree.
* x is item to search for.
* t is the node that roots the subtree.
*/
bool contains(const Comparable & x, BinaryNode *t) const {
	if(t == nullptr)
    	return false;
    else if(x < t->element)
    	return contains(x, t->left);
    else if(t->element < x)
    	return contains(x, t->right);
    else
    	return true; //Match
}
```
* findMin and findMax - they return a pointer to the node containing the smallest and largest elements in the tree respectively. To perform a findMin, start at the root and go left as long as there is a left child. The stopping point is the smallest element. The findMax routine is the same except that branching is to the right child.
* insert - to insert X into tree T, proceed down the tree as you would with a contains. If X is found, do nothing. Otherwise, insert X at the last spot on the path traversed. Duplicates can be handled by keeping an extra field in the node record indicating the frequency of occurrence. This adds some extra space to the entire tree but is better than putting duplicates in the tree (which tends to make the tree very deep). Of course, this strategy does not work if the key that guides the < operator is only part of a larger structure. If that is the case, then we can keep all of the structures that have the same key in an auxiliary data structure, such as a list or another search tree.

e.g.

```cpp
/*
* Internal method to insert into a subtree.
* x is the item to insert.
* t is the node that roots the subtree.
* Set the new root of the subtree.
*/
void insert(const Comparable & x, BinaryNode * & t) {
	if(t == nullptr)
    	t = new BinaryNode(x, nullptr, nullptr);
    else if (x < t->element)
    	insert(x, t->left);
    else if (t->element < x)
    	insert(x, t->right);
    else
    	; //Duplicate; do nothing
}
```

* remove - Once we have found the node to be deleted, we need to consider several possibilities. If the node is a leaf, it can be deleted immediately. If the node has one child, the node can be deleted after its parent adjusts a link to bypass the node. The complicated case deals with a node with two children. The general strategy is to replace the data of this node with the smallest data of the right subtree (which is easily found) and recursively delete that node (which is now empty). Because the smallest node in the right subtree cannot have a left child, the second remove is an easy one. If the number of deletions is expected to be small, then a popular strategy to use is lazy deletion: When an element is to be deleted, it is left in the tree and merely marked as being deleted. This is especially popular if duplicate items are present, because then the data member that keeps count of the frequency of appearance can be decremented. If the number of real nodes in the tree is the same as the number of "deleted" nodes, then the depth of the tree is only expected to go up by a small constant, so there is a very small time penalty associated with lazy deletion. Also, if a deleted item is reinserted, the overhead of allocating a new cell is avoided.

e.g.
```cpp
/*
* Internal method to remove from a subtree.
* x is the item to remove.
* t is the node that roots the subtree.
* Set the new root of the subtree.
*/
void remove(const Comparable & x, BinaryNode * & t) {
	if(t==nullptr)
    	return; //item not found; do nothing
    if(x < t->element)
    	remove(x, t->left);
    else if (t->element < x)
    	remove(x, t->right);
    else if(t->left != nullptr && t->right != nullptr) { //two children
    	t->element = findMin(t->right)->element;
        remove(t->element, t->right);
    
    } else {
    	BinaryNode *oldNode = t;
        t = (t->left != nullptr) ? t->left : t->right;
        delete oldNode;
    }
}
```

* Destructor calls makeEmpty. After recursively processing t's children, a call to delete is made for t. Thus all nodes are recursively reclaimed. At the end, t, and thus root, is changed to point at nullptr.

e.g.
```cpp
˜BinarySearchTree() {
	makeEmpty();
}

void makeEmpty(BinaryNode & & t) {
	if(t != nullptr) {
    	makeEmpty(t->left);
        makeEmpty(t->right);
        delete t;
    }
    t = nullptr;
}
```

* AVL
	* An AVL (Adelson-Velskii and Landis) tree is a binary search tree with a balance condition. The balance condition must be easy to maintain, and it ensures that the depth of the tree is O(logN). The simplest idea is to require that the left and right subtrees have the same height. Another balance condition would insist that every node must have left and right subtrees of the same height. Another balance condition would insist that every node must have left and right subtrees of the same height. If the height of an empty subtree is defined to be -1 (as is usual), then only perfectly balanced trees of 2<sub>k</sub>-1 nodes would satisfy this criterion. Thus, although this guarantees trees of small depth, the balance condition is too rigid to be useful and needs to be relaxed. An AVL tree is identical to a binary search tree, except that for every node in the tree, the height of the left and right subtrees can differ by at most 1. The height of an empty tree is defined to be -1.
	* The minimum number of nodes, S(h), in an AVL tree of height h is given by S(h) = S(h-1) + S(h-2) + 1. For h = 0, S(h) = 1. For h = 1, S(h) = 2. The function S(h) is closely related to the Fibonacci numbers.
	* All the tree operations can be performed in O(logN) time, except possibly insertion and deletion. When we do an insertion, we need to update all the balancing information for the nodes on the path back to the root, but the reason that insertion is potentially difficult is that inserting a node could violate the AVL tree property. If this is the case, then the property has to be restored before the insertion step is considered over. It turns out that this can always be done with a simple modification to the tree, known as a rotation.
