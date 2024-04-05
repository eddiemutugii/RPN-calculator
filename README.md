# RPN-calculator

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

#define MAX_STACK_SIZE 100

typedef struct {
    int top;
    int items[MAX_STACK_SIZE];
} Stack;

void push(Stack *s, int value) {
    if (s->top == MAX_STACK_SIZE - 1) {
        printf("Stack Overflow\n");
        exit(EXIT_FAILURE);
    } else {
        s->items[++(s->top)] = value;
    }
}

int pop(Stack *s) {
    if (s->top == -1) {
        printf("Stack Underflow\n");
        exit(EXIT_FAILURE);
    } else {
        return s->items[(s->top)--];
    }
}

int isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '%');
}

int applyOperator(int a, int b, char op) {
    switch (op) {
        case '+':
            return a + b;
        case '-':
            return a - b;
        case '*':
            return a * b;
        case '/':
            if (b == 0) {
                printf("Division by zero\n");
                exit(EXIT_FAILURE);
            } else {
                return a / b;
            }
        case '%':
            if (b == 0) {
                printf("Modulo by zero\n");
                exit(EXIT_FAILURE);
            } else {
                return a % b;
            }
        default:
            printf("Invalid Operator\n");
            exit(EXIT_FAILURE);
    }
}

int evaluateRPN(char *expression) {
    Stack stack;
    stack.top = -1;
    int i = 0;
    while (expression[i] != '\0') {
        if (isdigit(expression[i])) {
            push(&stack, expression[i] - '0');
        } else if (isOperator(expression[i])) {
            int b = pop(&stack);
            int a = pop(&stack);
            int result = applyOperator(a, b, expression[i]);
            push(&stack, result);
        }
        i++;
    }
    return pop(&stack);
}

int main() {
    char expression[100];
    printf("Enter the Infix Expression: ");
    scanf("%s", expression);
    int result = evaluateRPN(expression);
    printf("Result: %d\n", result);
    return 0;
}
