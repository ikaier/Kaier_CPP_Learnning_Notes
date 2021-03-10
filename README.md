# Introduction

This is houkai's learning note of C++

Reference: 

- Dr. Chad Zammar's Comp 322  McGill University 
- A Tour of C++ by Bjarne Stroustrup



# Primitive Data type

integer

Character

Boolean

Floating point

Double

Void

Wide Character

# String

```c++
strlen(some_string)
```

string length

```c++
strcpy(some_string,some_string2)
```

Copy string



# Class

- class is implemented by struct, they are interchangeable

- Everything in class is private and everything in struct is public

  



## Implement method outside the class

Reason why we  implement method outside the class:

1. Don't want the client to see the implementation
2. More organized when separeting interface and implementation
3. 

```c++
class Person
{
public:
	bool vanVote();
	void setAge(int age);
protected:
	int age;
	char sex;
};

boo Person::canVote() //method defined outside the class, can even be in different file
{
	if (age>=18)
		return true;
	return false;
}
```



# Constructor

Every class must have least one constructor, if you didn't create one, system will give you one by default.

method which has same name as the class

Normal way to implment constructor:

```c++
class Person
{
public:
	Person(int age, char sex);
protected:
	int age;
	char sex;
}

// constructors:
Person::Person(){
	this->age = 0;
	this->sex = 'U';
}

Person::Person(int age, char sex){
  this->age = age;
  this->sex = sex;
}
```



## Initialization list

Prefered way:

- better performance: no local variable is created in the process

```c++
class Person
{
public:
	Person();
	Person(int age, char sex);
protected:
	int age;
	char sex;
}

//Initialization list
Person::Person():age(0),sex('U'){
}
Person::Person(int age, char sex):age(age),sex(sex){
}
```



# Destructors

Special method kicks in when an object gets out of scope

Every class should has only one destructors

No return value

Compiler will generate minimum default destructor when you didn't create one

- Usually need to DIY when memory is allocated in initialization 

```c++
class Person
{
public:
	Person();
	Person(int age, char sex, char *name);
  //Destructor
  ~Person();
protected:
	int age;
	char sex;
  char *name;
}
Person::Person(int age, char sex, char *name){
  cout<<"constructor got called"<<endl;
  this->age = age;
  this->sex = sex;
  this->name= new char[strlen(name)];
  strcpy(this->name,name);
}
//Destructor
Person::~Person(){
	cout<<"Destructor got called"<<endl;
  delete [] this->name;
}

int main()
{
  Person Mike(24,'M',"Michael");
}
//Print result:
//Constructor got called
//Destructor got called

```



# Class with Dynamic Allocation

If you create a class using a pointer,Contructor will not be called until you create a real object:

```c++
Someclass *classPointer; //No constructor called, pointer is assigned by a random address
classPointer= new Someclass();//Constructor is called, classPointer points to the address of the class.
delete classPointer; //Destructor is called
```

This class has no name, and can only access this class by pointer.

**No desctructor is called unless you manually delete the pointer.**



# Pointer and Dynamic memory

If you create a pointer without assignning any value, compiler will assign a random address to the pointer, change value of a ramdom address is very dangerous. 

Correct way to initilze a pointer:

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



# Causing Memory Leak

Memory Leak Caused by losing track of allocated memory's address before free them by 

```c++
delete p;
```

Two classic examples:

1. Allocate memory in function but didn't free it before the return statement 

```c++
void x(){
	int* p = new int;
}
```

2. Allocate by pointer but reassign the pointer later

```c++
int* p = new int
p = new int
```

