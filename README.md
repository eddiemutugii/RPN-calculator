# RPN-calculator

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

#define MAX_SIZE 100


typedef struct {
    int data[MAX_SIZE];
    int top;
} Stack;


void init_stack(Stack *stack) {
    stack->top = -1;
}


int is_empty(Stack *stack) {
    return stack->top == -1;
}

int is_full(Stack *stack) {
    return stack->top == MAX_SIZE - 1;
}


void push(Stack *stack, int item) {
    if (is_full(stack)) {
        printf("Stack overflow\n");
        exit(1);
    }
    stack->data[++stack->top] = item;
}


int pop(Stack *stack) {
    if (is_empty(stack)) {
        printf("Stack underflow\n");
        exit(1);
    }
    return stack->data[stack->top--];
}


int precedence(char op) {
    switch (op) {
        case '+':
        case '-':
            return 1;
        case '*':
        case '/':
        case '%':
            return 2;
        default:
            return -1;
    }
}


void infix_to_postfix(const char *infix, char *postfix) {
    Stack stack;
    init_stack(&stack);
    int i = 0, j = 0;
    char token;

    while ((token = infix[i++]) != '\0') {
        if (isalnum(token)) {
            postfix[j++] = token;
        } else if (token == '(') {
            push(&stack, token);
        } else if (token == ')') {
            while (!is_empty(&stack) && stack.data[stack.top] != '(') {
                postfix[j++] = pop(&stack);
            }
            if (!is_empty(&stack) && stack.data[stack.top] != '(') {
                printf("Invalid infix expression\n");
                exit(1);
            }
            pop(&stack); 
        } else {
            while (!is_empty(&stack) && precedence(token) <= precedence(stack.data[stack.top])) {
                postfix[j++] = pop(&stack);
            }
            push(&stack, token);
        }
    }

    while (!is_empty(&stack)) {
        postfix[j++] = pop(&stack);
    }

    postfix[j] = '\0';
}

int main() {
    char infix[MAX_SIZE], postfix[MAX_SIZE];
    printf("Enter an infix expression: ");
    fgets(infix, sizeof(infix), stdin);
    infix_to_postfix(infix, postfix);
    printf("Postfix expression: %s\n", postfix);
    return 0;
}
