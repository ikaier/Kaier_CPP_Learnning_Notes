# Primitive Data type

integer

Character

Boolean

Floating point

Double

Void

Wide Character

# Pointer and Dynamic memory

```c++
int* p = new int;
```

:Pointer vairable p is on stack, holding the address in heap

```c++
*p =5;
```

:Address in p equal to 5

```c++
delete p;
```

deallocate memory int(5) in the heap

p is still point to the same address which is not valid (dangling pointer)

Better to do 

```c++
p = NULL; // or p=0;
```

Then

```c++
p = new int(10)	
```

p stores address of 10

# Creating Memory Leak

1. Allocate memory in function but didn't delete

```c++
void x(){
	int* p = new int;
}
```

2. Allocate by pointer but reassign the pointer

```c++
int* p = new int
p = new int
```

