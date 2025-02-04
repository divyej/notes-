# Singly Linked Lists

<img width="1010" alt="image" src="https://user-images.githubusercontent.com/28825619/212460591-c67be464-1b88-473a-84d3-8e7017345694.png">



Link of Last Node of LL contains NULL indicating the end of list, hence this makes it a Singly Linked List


```c
struct List{
    int data;
    // Pointer to next List node
    struct List *link;
};

// Makes it easy to using pointer of LL
typedef struct List *NODE;
```

## Basic Operations on SLL
    - Insertion
        a. Front
        b. End
        c. Position
    - Traversing 
    - Deletion
        a. Front
        b. End
        c. Position    


## Insertion



### 1. Front Insertion


Front insertion is when a new node is attached *before* the current head node.

Two step process: 

    1. Update the link of new node to the current head
    2. Update head pointer to the new node

<img width="1255" alt="image" src="https://user-images.githubusercontent.com/28825619/212463672-e2ed2926-1f87-4ef9-9838-891dd85290bd.png">




Time Complexity: O(1)

Space Complexity: O(1)


```c

NODE insertFront(NODE head, int data){

    NODE newNode;

    // creates an unnamed node & pointer
    newNode = (NODE)malloc(sizeof(struct List));

    // assigns the input data to data of newNode 
        newNode -> data = data;

    if(head == NULL){

        // if head is empty, implies list is empty
        // so creating new node & assigning to head

        head = newNode;
        head -> link = NULL;


    }else{

        // establishing connection by assinging newNode link to head's address
        newNode -> link = head;

        // updates the head to the newNOde
        head = newNode;

    }

    return head;
}
```



### 2. End Insertion

End insertion is when a new node is attached *after* the last node.

Two step process:

    1. Iterate throught SLL until we reach current last node
    2. Change link of last node from NULL to newNode address


<img width="1208" alt="image" src="https://user-images.githubusercontent.com/28825619/212464530-bab64a40-d34d-4806-b693-376a9ec2e224.png">



Time Complexity: O(n) [Since we have to iterate through SLL to reach end of SLL]

Space Complexity: O(1)


```c

NODE insertEnd(NODE head, int data){
    NODE currentNode = head;

    NODE newNode;

    // Creates new node
    newNode = (NODE)malloc(sizeof(struct List));

    newNode -> data = data;
    newNode -> link = NULL;

    if(currentNode == NULL){

        // If there's no node, newNode becomes the head node.
        head = newNode;

    }else{
        
        while(currentNode -> link != NULL){
            currentNode = currentNode -> link;
        }

        // here, current node is now at the end of SLL
        currentNode -> link = newNode;

    }

    return head;


}

```



### 3. Position Insertion

Position insertion is adding node to the specified position.

Three step process:

    1. Iterate until iterator reaches the given position 
    2. Assign the link of preNode to the newNode link
    3. Update the link of preNode to newNode's address

<img width="1202" alt="image" src="https://user-images.githubusercontent.com/28825619/212468258-97bb985d-7b93-45fa-bac1-2d4c623d606f.png">



Time Complexity: O(n) [Worst case scenario]

Space Complexity: O(1)

```c
NODE insertAt(NODE head, int position, int data){
    // Predecessor Node
    NODE preNode = head;

    NODE newNode;

    // Creates new node
    newNode = (NODE)malloc(sizeof(struct List));

    newNode -> data = data;
    

    if(position <= 1){
        newNode -> link = head;
        head = newNode;
        return head;
        
    }

    // --position to iterate through the SLL, as soon as it reaches position, loop terminates.
    // We also use [preNode -> link != NULL] to check whether position > actual length.
    // If position > length of SLL, then we would add the node to the last of it.
    while(--position && preNode -> link != NULL){
        preNode = preNode -> link;
    }


    if(preNode -> link == NULL){
        // Insertion at end
        newNode -> link = NULL;
        preNode -> link = newNode;
        
    }else{
        // Insertion at random 
        newNode -> link = preNode -> link;
        preNode -> link = newNode;
    }


    return head;
}

```


## Traversing

- Traverse until we reach the *last node* of SLL
- Last Node has link = NULL, so we have to iterate till the ```link != NULL```
- Good thing about SLL is that we don't need to know the size of it to iterate unlike arrays
- Time Complexity = O(n)
- Space Complexity = O(1)


```c
struct List{
    int data;
    // Pointer to next List node
    struct List *link;
};

typedef struct List *NODE;


int length(NODE head){
    NODE currentNode = head -> link;
    int count = 0;

    while (currentNode != NULL){
        count++;
        currentNode = currentNode -> link;
    }

    return count;
}
```
    

## Deletion

### 1. Front Deletion

Current header node gets deleted and the next node to it becomes the new header node

Two step process:

    1. Create temporary node to point the same node as of current header node
    2. Move the header node pointer to the next node & dispose temporary node
    
    
<img width="1108" alt="image" src="https://user-images.githubusercontent.com/28825619/212478358-c216a621-95ef-4f92-9b0b-4b75e4ab2949.png">



Time Complexity: O(1)

Space Complexity: O(1)


```c

void deleteFront(NODE head){
    NODE temp = head;

    if(head == NULL){
        return;
    }

    head = head -> link;
    free(temp);

}

```


### 2. End Deletion

Tail node gets deleted and the link of previous node to it becomes the NULL

Two step process:

    1. Iterate until we reach last node, also storing the previous node
    2. Assign previous node's link to NULL & dispose currentNode

    
<img width="1111" alt="image" src="https://user-images.githubusercontent.com/28825619/212494694-10096fa7-9396-45f8-bd1e-26aea7a540fa.png">



Time Complexity: O(n)

Space Complexity: O(1)


```c

void deleteEnd(NODE head){
    NODE currentNode = head;
    NODE preEndNode = head;

    if(head == NULL){
        return;
    }

    while(currentNode -> link != NULL){

        preEndNode = currentNode;

        currentNode = currentNode -> link;

    }

    preEndNode -> link = NULL;

    free(currentNode);

    return;
}

```


### 3. Position Deletion

Node present at given position nee

Two step process:

    1. Create temporary node to point the same node as of current header node
    2. Move the header node pointer to the next node & dispose temporary node

    
<img width="1055" alt="image" src="https://user-images.githubusercontent.com/28825619/212528704-a9a9a046-f487-46ea-a33a-9de180b4c2c2.png">


Time Complexity: O(n)

Space Complexity: O(1)


```c

void deleteAt(NODE head, int position){
    NODE currentNode = head;
    NODE preEndNode = head;

    if(head == NULL){
        printf("List is Empty!");
        return;
    }

    if(position <= 1){

        head = head -> link;
        free(preEndNode);
        return;

    }else{

        while(--position && currentNode -> link != NULL){

            preEndNode = currentNode;

            currentNode = currentNode -> link;

        }

        preEndNode -> link = currentNode -> link;
        free(currentNode); 
    }

    return;
}

```
