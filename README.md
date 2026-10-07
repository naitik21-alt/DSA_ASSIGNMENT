# DSA_ASSIGNMENT

Q1: Stack Implementation Using Array
A Stack operates on a Last-In, First-Out (LIFO) principle. The C program below implements a fixed-size stack using an array and a top index variable, including bounds checks for Stack Overflow and Stack Underflow.

c program:

#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 5

typedef struct {
    int arr[MAX_SIZE];
    int top;
} Stack;

// Initialize stack
void initStack(Stack *s) {
    s->top = -1;
}

// Check if stack is full
int isFull(Stack *s) {
    return s->top == MAX_SIZE - 1;
}

// Check if stack is empty
int isEmpty(Stack *s) {
    return s->top == -1;
}

// PUSH operation
void PUSH(Stack *s, int value) {
    if (isFull(s)) {
        printf("Error: Stack Overflow! Cannot push %d. Stack is full.\n", value);
        return;
    }
    s->top++;
    s->arr[s->top] = value;
    printf("Successfully pushed %d onto stack.\n", value);
}

// POP operation
int POP(Stack *s) {
    if (isEmpty(s)) {
        printf("Error: Stack Underflow! Cannot pop from an empty stack.\n");
        return -1;
    }
    int poppedValue = s->arr[s->top];
    s->top--;
    return poppedValue;
}

// PEEK operation
int PEEK(Stack *s) {
    if (isEmpty(s)) {
        printf("Error: Stack is empty! Nothing to peek.\n");
        return -1;
    }
    return s->arr[s->top];
}

// DISPLAY operation
void DISPLAY(Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty.\n");
        return;
    }
    printf("Current Stack (top to bottom): ");
    for (int i = s->top; i >= 0; i--) {
        printf("%d ", s->arr[i]);
    }
    printf("\n");
}

int main() {
    Stack s;
    initStack(&s);

    PUSH(&s, 10);
    PUSH(&s, 20);
    PUSH(&s, 30);
    PUSH(&s, 40);
    PUSH(&s, 50);
    
    // Triggers Stack Overflow
    PUSH(&s, 60);

    DISPLAY(&s);

    printf("Top element (PEEK): %d\n", PEEK(&s));

    printf("Popped: %d\n", POP(&s));
    printf("Popped: %d\n", POP(&s));

    DISPLAY(&s);

    POP(&s);
    POP(&s);
    POP(&s);
    
    // Triggers Stack Underflow
    POP(&s);

    return 0;
}

Additional Analysis: Stack Complexities & Overflow
Complexity Analysis:
Operation Time.     Complexity Space Complexity
PUSH(x) O(1) O(1)
POP() O(1) O(1)
PEEK() O(1) O(1)
DISPLAY() O(n) O(1) auxiliary

Overall Space Complexity: O(N), where N is MAX_SIZE (the allocated array capacity).
Inserting Beyond Capacity (Fixed Size): When the stack is fixed-size and top reaches capacity - 1, attempting to insert another element causes a Stack Overflow. In low-level languages like C, attempting to write past array bounds without checks leads to memory corruption or segmentation faults. In properly guarded code, the operation is blocked to preserve existing stack integrity.






Q2: Circular Queue Implementation Using Array
A Circular Queue operates on a First-In, First-Out (FIFO) basis, wrapping front and rear pointers using modulo arithmetic ((index + 1) % MAX_SIZE).

c program:

#include <stdio.h>

#define MAX_SIZE 5

typedef struct {
    int arr[MAX_SIZE];
    int front;
    int rear;
} CircularQueue;

// Initialize circular queue
void initQueue(CircularQueue *q) {
    q->front = -1;
    q->rear = -1;
}

// Check if queue is empty
int isEmpty(CircularQueue *q) {
    return q->front == -1;
}

// Check if queue is full
int isFull(CircularQueue *q) {
    return (q->rear + 1) % MAX_SIZE == q->front;
}

// ENQUEUE operation
void ENQUEUE(CircularQueue *q, int value) {
    if (isFull(q)) {
        printf("Error: Queue Overflow! Cannot enqueue %d.\n", value);
        return;
    }
    
    if (isEmpty(q)) {
        q->front = 0;
        q->rear = 0;
    } else {
        q->rear = (q->rear + 1) % MAX_SIZE;
    }
    
    q->arr[q->rear] = value;
    printf("Enqueued %d successfully.\n", value);
}

// DEQUEUE operation
int DEQUEUE(CircularQueue *q) {
    if (isEmpty(q)) {
        printf("Error: Queue Underflow! Cannot dequeue from empty queue.\n");
        return -1;
    }

    int removedValue = q->arr[q->front];

    // Reset pointers if queue becomes empty
    if (q->front == q->rear) {
        q->front = -1;
        q->rear = -1;
    } else {
        q->front = (q->front + 1) % MAX_SIZE;
    }

    return removedValue;
}

// FRONT operation
int FRONT(CircularQueue *q) {
    if (isEmpty(q)) {
        printf("Error: Queue is empty.\n");
        return -1;
    }
    return q->arr[q->front];
}

// DISPLAY operation
void DISPLAY(CircularQueue *q) {
    if (isEmpty(q)) {
        printf("Queue is empty.\n");
        return;
    }

    printf("Circular Queue elements: ");
    int i = q->front;
    while (1) {
        printf("%d ", q->arr[i]);
        if (i == q->rear) {
            break;
        }
        i = (i + 1) % MAX_SIZE;
    }
    printf("\n");
}

int main() {
    CircularQueue q;
    initQueue(&q);

    ENQUEUE(&q, 10);
    ENQUEUE(&q, 20);
    ENQUEUE(&q, 30);
    ENQUEUE(&q, 40);
    ENQUEUE(&q, 50);

    // Queue is full
    ENQUEUE(&q, 60);

    DISPLAY(&q);

    printf("Dequeued element: %d\n", DEQUEUE(&q));
    printf("Dequeued element: %d\n", DEQUEUE(&q));

    DISPLAY(&q);

    // Wraps around to reuse freed space at the beginning
    ENQUEUE(&q, 60);
    ENQUEUE(&q, 70);

    DISPLAY(&q);

    printf("Front element: %d\n", FRONT(&q));

    return 0;
}

Additional Analysis: Circular vs. Linear Queue Comparison
Problem in Linear Queue (False Overflow): In a linear queue using a static array, REAR increments sequentially. Once REAR reaches MAX_SIZE - 1, any subsequent ENQUEUE attempt will fail with an overflow error—even if multiple DEQUEUE operations have freed space at the beginning of the array. These freed locations remain unusable without costly O(N) element shifts.
Why Circular Queue Provides Better Memory Utilization: It connects the last position of the array back to the first position using modulo arithmetic (rear + 1) % MAX_SIZE. When slots at indices 0, 1, ... become available due to DEQUEUE actions, ENQUEUE can seamlessly reuse those freed indices without needing to shift existing array elements.
Time Complexity:
ENQUEUE: O(1)
DEQUEUE: O(1)
FRONT: O(1)
Space Complexity: Auxiliary space per operation is O(1). Total allocated space is O(N) for an array capacity of N.
