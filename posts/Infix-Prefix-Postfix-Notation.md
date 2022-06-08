<!--
.. title: Infix, Prefix, Postfix Notation
.. slug: infix-prefix-postfix-notation
.. date: 2019-10-14 09:57:59
.. tags: Python
.. category: Python
.. link:
.. description:
.. type: text
-->

[TOC]

Infix, prefix and postfix notations are three equivalent ways of writing mathematical expressions. 

# 1 Infix Notation

Infix notation is the most natural way we write mathematical expressions, as we use it every day. For example, $A+B$, which means a plus b. However, infix notation needs extra information to make the order of evaluation of the operators clear, in which case, means the arithmetic rules. An expression like $(A+B)*(C+D)$ means that we should do the two additions in the brackets first and then do the multiplication by the two results.

# 2 Prefix Notation

When using prefix notation, the operator is written before their operands. For example, assume you want to convert the infix expression $(A+B)*(C+D)$ to prefix, the result is like this: $*+AB+CD$. In fact, we can add brackets to make this more explicit: $((A+B)*(C+D))$. As you can find, the operator of the two operands just moves to the left bracket in prefix expression.

# 3 Postfix Notation

Contrary to the prefix notation, the operator is written after their operands in postfix expression. For example, infix expression $(A+B)*(C+D)$ should be writen as $AB+CD+*$. If we use the same adding-brackets trick, you will find the operator of the operands moves to the right in postfix expression.

Though equivalent, infix, prefix and postfix notations have different application scenarios.

> Becasue infix notation is so common in mathematics, it is much easier to read, and so is used in most computer languages. However, prefix notation is often used for operators that take a single operand(e.g. negation) and function calls. Although postfix notation and prefix notation have similar complexity, postfix is slightly easier to evaluate in simple circumstances, such as in some calculators, as the operators really are evaluated strictly left-to-right.

# 4 Conversion of Infix Expressions to Prefix and Postfix

## 4.1 Infix-to-Postfix Conversion

The basis for the algorithms goes like this:

- Initialize an empty list and stack.
- Read the infix expression from left to right.
- If an operand is encountered, add it to the list.
- If a left parenthesis is encountered, add it to the stack.
- If a right parenthesis is encountered, pop the stack until the corresponding left parenthesis is removed and append each operator to the list.
- If an operator is encountered:
  - If the operator has a higher precedence over other operators in the stack, push it on the stack.
  - If the operator has a lower or equal precedence over other operators in the stack, pop the stack until the rest operators have a lower precedence. Append those operators poped to the list and push the operator on the stack.
- Reached the end of the expression, pop everything in the stack and append them to the list.

Here is the algorithm implementation in Python.

```python
# Define Stack class.
class Stack():
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        return self.items.pop()
    
    
    def size(self):
        return len(self.items)        
    
    def peak(self):
        return self.items[len(self.items)-1]
    
    def isEmpty(self):
        return len(self.items) == 0
    
```

```python
def infixToPostfix(infixexpr):
    prec = {'*':3, '/':3, '+':2, '-':2, '(':1} # Define the arthmetic precedence using a dict.
    operator = Stack()
    postfixList = []
    tokenList = infixexpr.split()
    
    for token in tokenList:
        if token in "ABCDEFGHIJKLMNOPQRSTUVWXYZ" or token in "0123456789":
            postfixList.append(token)
        elif token == '(':
            operator.push(token)
        elif token == ')':
            topToken = operator.pop()
            while not topToken == '(':
                postfixList.append(topToken)
                topToken = operator.pop()
        else:
            while (not operator.isEmpty()) and (prec[operator.peak()] >= prec[token]):
                postfixList.append(operator.pop())
            operator.push(token)
    
    while not operator.isEmpty():
        postfixList.append(operator.pop())
    return ' '.join(postfixList)
```

## 4.2 Postfix Evaluation

Here we will consider the evaluation of an expression which is already in postfix notation. The basis for the algorithm goes like this:

- Initialize an empty stack.
- Read the postfix expression from left to right.
- If a number is encountered, add it to the stack.
- If an operator is encountered, pop two operands from the stack and do the corresponding operation. Push the result on the stack.

Here is the algorithm implementation in Python.

```python
def postfixEval(postfixExpr):
    operandStack = Stack()
    tokenList = postfixExpr.split()

    for token in tokenList:
        if token in "0123456789":
            operandStack.push(int(token))
        else:
            operand2 = operandStack.pop()
            operand1 = operandStack.pop()
            result = doMath(token,operand1,operand2)
            operandStack.push(result)
    return operandStack.pop()

def doMath(op, op1, op2):
    if op == "*":
        return op1 * op2
    elif op == "/":
        return op1 / op2
    elif op == "+":
        return op1 + op2
    else:
        return op1 - op2
```

## 4.3 Infix-to-Prefix Conversion

The basis for the algorithms goes like this:

- Initialize an empty list and stack.
- Reverse the infix expression and read it from left to right.
- If an operand is encountered, add it to the list.
- If a right parenthesis is encountered, add it to the stack.
- If a left parenthesis is encountered, pop the stack until the corresponding left parenthesis is removed and append each operator to the list.
- If an operator is encountered:
  - If the operator has a higher or equal precedence over other operators in the stack, push it on the stack.
  - If the operator has a lower precedence over other operators in the stack, pop the stack until the rest operators have a lower or equal precedence. Append those operators poped to the list and push the operator on the stack.
- Reached the end of the expression, pop everything in the stack and append them to the list.
- Reverse the list.

Here is the algorithm implementation in Python.

```python
def infixToPrefix(infixexpr):
    prec = {'*':3, '/':3, '+':2, '-':2, ')':1} # Define the arthmetic precedence using a dict.
    operator = Stack()
    prefixList = []
    tokenList = infixexpr.split()
    tokenList.reverse()
    
    for token in tokenList:
        if token in "ABCDEFGHIJKLMNOPQRSTUVWXYZ" or token in "0123456789":
            prefixList.append(token)
        elif token == ')':
            operator.push(token)
        elif token == '(':
            topToken = operator.pop()
            while not topToken == ')':
                prefixList.append(topToken)
                topToken = operator.pop()
        else:
            while (not operator.isEmpty()) and (prec[operator.peak()] > prec[token]):
                prefixList.append(operator.pop())
            operator.push(token)
    
    while not operator.isEmpty():
        prefixList.append(operator.pop())
    prefixList.reverse()
    
    return ' '.join(prefixList)
```

## 4.4 Prefix Evaluation

The algorithms for prefix evaluation is almost like postfix.

- Reverse the prefix expression.
- If a number is encountered, push it on the stack.
- If a operator is encountered, pop two operands from the stack and do the corresponding operation. Push the result on the stack.

```python
def prefixEval(prefixExpr):
    operandStack = Stack()
    tokenList = prefixExpr.split()
    tokenList.reverse()

    for token in tokenList:
        if token in "0123456789":
            operandStack.push(int(token))
        else:
            operand1 = operandStack.pop()
            operand2 = operandStack.pop()
            result = doMath(token,operand1,operand2)
            operandStack.push(result)
    return operandStack.pop()

def doMath(op, op1, op2):
    if op == "*":
        return op1 * op2
    elif op == "/":
        return op1 / op2
    elif op == "+":
        return op1 + op2
    else:
        return op1 - op2
```

**References**

- http://www.cs.man.ac.uk/~pjj/cs212/fix.html
- https://runestone.academy/runestone/books/published/pythonds/BasicDS/InfixPrefixandPostfixExpressions.html

