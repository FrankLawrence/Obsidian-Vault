---
Tags: 
Created: 2022-10-09 19:48:25
---
(Links:: [[Computer Programming]])
## Pointer basics
### Pointer variables
A **pointer** is a variable that holds a memory address.
A pointer has a data type, and the data type determines what type of address is held in the pointer.
A pointer is declared by includeing `*` before the pointer's name. Ex: `int* maxItemPointer`
The **reference operator** (&) obtains a variable's address
Ex: `&someVar` returns the memory address of variable someVar.
### Dereferencing a pointer
The **dereference operator** (`*`) is prepended to a pointer variable's name to retrieve the data to which the pointer variable points.
Ex: If valPointer points to a memory address containing the integer 123, then `cout << *valPointer;` dereferences valPointer and outputs 123.
### Null pointer
A pointer that is assigned with the keyword **nullptr** is said to be null. Ex: `int *maxValPointer = nullptr;` makes maxValPointer null.
### Common pointer errors
- Use the dereference operator when initializing a pointer. Ex: `*valPointer = & maxValue;` is a syntax error because `*valPointer` is referring to the value pointed to, not the pointer itself.
## Operators: new, delete, and ->
### New Operator
The **new operator** allocates memory for the given type and returns a pointer to the allocated memory. 
### The member access operator
When using a pointer to an object, the **member access operator** (**->**) allows access to the object's members with the syntax **a->b** instead of **(*a).b**
### The delete operator
The **delete operator** deallocates a block of memory that was allocated with the new operator.
### Allocating and deleting object arrays
The new operator creates a dynamically allocated array of objects if the class name is followed by square brackets containing the array's length
## A first linked list
- Items can be inserted without the need to shift items (as required for a vector)
- Each **list node** (list item) is comprised of the data to be stored in each list item
- A special node head is created to represent the front of the list, after which regular items can be inserted.
```cpp
#include <iostream>
using namespace std;

class IntNode {
public:
   IntNode(int dataInit = 0, IntNode* nextLoc = nullptr);
   void InsertAfter(IntNode* nodeLoc);
   IntNode* GetNext();
   void PrintNodeData();
private:
   int dataVal;
   IntNode* nextNodePtr;
};

// Constructor
IntNode::IntNode(int dataInit, IntNode* nextLoc) {
   this->dataVal = dataInit;
   this->nextNodePtr = nextLoc;
}

/* Insert node after this node.
 * Before: this -- next
 * After:  this -- node -- next
 */
void IntNode::InsertAfter(IntNode* nodeLoc) {
   IntNode* tmpNext = nullptr;
   
   tmpNext = this->nextNodePtr;    // Remember next
   this->nextNodePtr = nodeLoc;    // this -- node -- ?
   nodeLoc->nextNodePtr = tmpNext; // this -- node -- next
}

// Print dataVal
void IntNode::PrintNodeData() {
   cout << this->dataVal << endl;
}

// Grab location pointed by nextNodePtr
IntNode* IntNode::GetNext() {
   return this->nextNodePtr;
}

int main() {
   IntNode* headObj  = nullptr; // Create IntNode objects
   IntNode* nodeObj1 = nullptr;
   IntNode* nodeObj2 = nullptr;
   IntNode* nodeObj3 = nullptr;
   IntNode* currObj  = nullptr;
   
   // Front of nodes list
   headObj = new IntNode(-1);
   
   // Insert nodes
   nodeObj1 = new IntNode(555);
   headObj->InsertAfter(nodeObj1);
   
   nodeObj2 = new IntNode(999);
   nodeObj1->InsertAfter(nodeObj2);
   
   nodeObj3 = new IntNode(777);
   nodeObj1->InsertAfter(nodeObj3);
   
   // Print linked list
   currObj = headObj;
   while (currObj != nullptr) {
      currObj->PrintNodeData();
      currObj = currObj->GetNext();
   }
   
   return 0;
}
``

___
References: