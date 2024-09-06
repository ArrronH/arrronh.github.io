---
layout: project
type: project
image: img/Calculator.jpeg
title: "Command-line Calculator"
date: 2024/03/03
published: true
labels:
  - C
---

summary: "A very simple command-line calculator that I made in ICS 212."

This is a command-line calculator that I made for one of the assignments in ICS 212! This calculator is able to add, subtract, multiply, and divide any non-decimal number. If you enter anything invalid, it shoots back an error and tells you to re-enter the equation. Here is a snippet of the code.

```cpp
    //Input validation
    if(argc != 4) {
        printf("ERROR: You must enter four commandline arguments! You entered %i.\n", argc);
        puts("The input must be: <./program> <num1> <operator> <num2>");
        puts("Example: ./program 3 + 6");
        puts("The valid operators are '+' for addition, '-' for subtraction, '.' for multiplication, or '/' for  division.");
        return 1;
    }

    //Check to see if argv[1] and argv[3] are actually digits
    if(argv[1][0] > '9' || argv[1][0] < '0' || argv[1][1] != '\0') {
        puts("ERROR: Your second argument must be a digit from 0 - 9.");
        return 1;
    }

    if(argv[3][0] > '9' || argv[3][0] < '0' || argv[3][1] != '\0') {
        puts("ERROR: Your fourth argument must be a digit from 0 - 9");
        return 1;
    }

    if(argv[2][0] != '+' && argv[2][0] != '-' && argv[2][0] != '.' && argv[2][0] != '/' || argv[2][1] != '\0') {
        puts("ERROR: Your third argument must be a +, -, ., or /");
        return 1;
    }

    //Done with input validation
    num1 = argv[1][0] - '0';
    num2 = argv[3][0] - '0';
    operator = argv[2][0] - '+';

    //Array of functions
    int (*functArray[SIZE])(int, int) = {add, NULL, sub, mult, div};
 
    //Make sure the operator is valid
    if(functArray[operator] != NULL) {
        printf("%i %c %i = %i\n",num1, argv[2][0], num2, functArray[operator](num1, num2));
    } else {
        puts("Invalid operator");
    }    
    return 0;
}
```
