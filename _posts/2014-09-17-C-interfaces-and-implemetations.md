**C Interfaces and Implementations**
-----

#Chapter 1 Introduction

##1.1 Literate Programs
> Literate programming is an approach to programming introduced by Donald Knuth in which a program is given as an explanation of the program logic in a natural language, such as English, interspersed with snippets of macros and traditional source code, from which a compilable source code can be generated.

##1.2 Programming Style

> It is more important for programs to be read easily and understood by people than it is for them to be compiled easily by computers.

In general, longer, evocative names are used for *global variables* and routines, and short names, which may mirror common mathematical notation, are used for *local variables*.

##1.3 Efficiency

A program needs only to be fast enough, not necessarily as fast as possible.

#Chapter 2 Interfaces and Implementations

##2.1 Interfaces

##2.2 Implementations

##2.3 Abstract Data Type
An *abstract data type* is an interfaces that defines a data type and operations on values of that type.

##2.4 Client Responsibilities

    typedef struct T *T;
    int Stack_empty(const T stk){
        assert(stk);
        return stk->count == 0;
    }

The use of *const* is wrong. The intent here is to declare *stk* to be a **"pointer to a constant struct T"**. But the declaration *const T stk* declares *stk* to be a **"constant pointer to a struct T"**. The typedef for T wraps the *struct T\** in a single type, and this entire type is the operand of const **const (struct T\*)**, which is meaningless since all scalars are passed by value in C.

##2.5 Efficiency

#Chapter 3 Atom
An *atom* is a pointer to a unique, immutable sequence if zero or more arbitrary bytes. Most atoms are pointers to null-terminated
strings, but a pointer to any sequence of bytes can be an atom
