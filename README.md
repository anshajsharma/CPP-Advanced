<h1 align="center">
    C++ Advanced
 </h1>
 <h5 align="center">
   Author: Anshaj Kumar
 </h5>
 
 
 ### Contents  
 
| No. | Topics |
| --- | --------- |
|    | **Core C++** |
|1   | [Special Notes](#special-notes-) |
|2   | [Strings in C++](#strings-in-c) |
|3   | [Namespaces](#namespaces) |
|4   | [DYNAMIC MEMORY ALLOCATION & Pointers and Double pointers](#dynamic-memory-allocation--pointers-and-double-pointers) |
|5   | [Struct and Classes](#struct-and-classes) |
|6   | [Constructor and Destructor](#constructor-and-destructor) |
|7   | [Visibility](#visibility) |
|8   | [Inheritance](#inheritance) |
|9   | [Virtual Function](#virtual-function) |  
|10  | [Pure Virtual Function](#pure-virtual-function) |
|11  | [Member Initializer List](#member-initializer-list) |
|12  | [Implicit And Explicit Keyword](#implicit-and-explicit-keyword) |
|13  | [Operators And Operators Overloading](#operators-and-operators-overloading) |
|14  | [`this` keyword](#this-keyword) |
|15  | [Lifetime](#lifetime) |
|16  | [Copy And Copy Constructor](#copy-and-copy-constructor) |
|17  | [Arrow Operator & Scope Pointer](#arrow-operator--scope-pointer) |
|18  | [Determining Offset](#determining-offset) |
|19  | [Vector Copy Techniques](#vector-copy-techniques) |
|20  | [Static and Dynamic Linking](#static-and-dynamic-linking) |
|21  | [Templetes](#templetes) |
|22  | [Function Pointers](#function-pointers) |
|23  | [Lambdas](#lambdas) |
|24  | [Threads](#threads) |
|25  | [Timings](#timings) |


 #### Refer [CPPReference](https://en.cppreference.com/w/) for more. ####



 ###    Special Notes :  
 
 1. If we do not define *main()* function in any cpp file compiler will compile the file but linker will give missing **entry point error**.
 2. *Refrences should we initialised with a value/refrence at time of deceleration*  
 Reference is not a variable and we have to declare it at time of initialisation and 
	 can't be modified furthur i.e. assigned to reference of other variable if we try to do this 
  	it will simply copy the value.
    ```C++
		int a = 5;
		int b = 6;
		// int& ref ;  X wrong (ref should we initialised wiyh a value/refrence at time of deceleration)
		int& ref = a;
		ref = b ; ( implies a = 6 , b = 6 ) 
    ```  
   3. `const int* const a=new int`  Here, 1st pointer means you cant change refrence of pointer(i.e memory address it is pointing to) 2nd const means you can't change Value after dereferencing pointer.
   4. If any method is const (declare const after method name like `int GetCount() const { return count; }`) i.e. it is read only and not going to change value of any variable of class.
   5. If a variable is declared as `mutable int a;`, it can be changed from inside of const methods.  
   6. `Object obj = new Object();` initialises Object in Heap while `Object obj;` initialise Object in stack.  
>  *  Allocating in Heap takes longer than allocating in stack.  
>  *  In case of heap , You have to manually free the memory you allocated. (i.e. `delete obj;`)  
>  *  In case of heap , contiguous memory is allocated i.e. we have to find a place from where required amount of memory is present which of course takes time. (There is something called freelist which takes care of free memories/memories addresses.)
	
   7. `new` keyword calls constructor and `delete` keyword calls destructor in case of class.
   

   
**[⬆ Back to Top](#----c-advanced-)** 

# Strings in C++
```C++
#include<iostream>
#include<string>
int main()
{
	const char* name = u8"Anshaj";      // 1 byte per character utf8
	const wchar_t* name2 = L"Anshaj";   // 2 byte per character
	const char16_t* name3 = u"Anshaj";  // 2 byte per character utf16
	const char32_t* name4 = U"Anshaj";  // 4 byte per character utf32
  
	using namespace std::string_literals;
	std::string name0 = u8"Anshaj"s + "Kumar";
	std::wstring name01 = L"Anshaj"s + L"Kumar";
	std::u16string name02 = u"Anshaj"s + u"Kumar";
	std::u32string name03 = U"Anshaj"s + U"Kumar";

	//Multiline string in C++
	const char* example = R"(   
		Line1
		Line2
		Line3
		Line4)";                
	std::cout << example << std::endl;
	std::cin.get();
}
```  

**[⬆ Back to Top](#----c-advanced-)**
#  Namespaces
*  [Reference](https://en.cppreference.com/w/cpp/language/namespace)
*  Namespaces are used to avoid naming conflicts. 
```C++
  #include<iostream>
  
  namespace apple {
 	void printf(const char* a)
  	{
  		std::cout << a << std::endl;
  	}
  }

  namespace orange {
  	void printf(const char* a)
  	{
  		std::cout << a << std::endl;
  	}
  }

  int main()
  {
	
	  apple::printf("Apple printf");
	  orange::printf("Orange printf");
	  std::printf("std printf\n");  
          std::cout << "Hello world!!" << std::endl;
  	  std::cin.get();
  }
```  

**[⬆ Back to Top](#----c-advanced-)**
  
##  DYNAMIC MEMORY ALLOCATION **&** Pointers and Double pointers
```C++
#include<iostream>

void IncrementUsingPointer(int* a)
{
	(*a)++;  // will derefence pointer and replace value stored in memory with value + 1 
}

void IncrementUsingReference(int& a)
{
	  a++;
}

int main()
{
	//-----------DYNAMIC MEMORY ALLOCATION---------------
	//-----------Pointers and Double pointers---------------
	char* buffer = new char[8];
	memset(buffer, 0, 8);

	// Double pointer
	char** ptr = &buffer;

	// MEMORY DEALLOCATION using delete keyword
	delete[] buffer;

	int var = 5;
	int& ref = var;
	ref = 2;
	// here 2 will be printed
	std::cout << var << std::endl;
	IncrementUsingPointer(&var);
	// var will be 3
	std::cout << "IncrementUsingPointer " << var << std::endl;
	IncrementUsingReference(var);
	// var will be 4
	std::cout << "IncrementUsingReference " << var << std::endl;
	std::cin.get();
}

```  

**[⬆ Back to Top](#----c-advanced-)**

# Struct and Classes

```c++
class Player {
	public:
		int x, y;  // by default x,y will be private in classes
		int speed;

	// Functions inside class is called method
		void Move( int xa, int ya)
		{
			x += xa * speed;
			y += ya * speed;
		}
};
```


```c++
struct PlayerStruct {
	int x, y;  // by default x,y will be public in structs
				    // this is the difference b/w class and struct
	int speed;

	void Move(int xa, int ya)
	{
		x += xa * speed;
		y += ya * speed;
	}
};
```



## NOTE:
 **Static keyword outside the class means that entity will only be usable to that single transitional unit(obj file)
 but static variable or method inside class means that that variable or method will have only one instance
 independent to the instances of class.**

 ## We can use *extern* to refer public/global variable of other cpp file

 ```c++
 #include<iostream>

 extern int var; // compiler will understand that variable is defined globally anywhere in other cpp file.

 int main()
 {
	 std::cout << var << std::endl;
	 std::cin.get();
 }
 ```

 **Static means its lifetime is infinite.**  
  Static variable is declared only once i.e. single instance is created and further we use it.


```c++
 #include<iostream>

void function()
{
	static int i = 0;
	i++;
	std::cout << i << " ";
}

int main()
{
	function();
	function();
	function();
	function();
	function();
	function();

	std::cin.get();
}
// Output of this code will be 1 2 3 4 5 6
```
**[⬆ Back to Top](#----c-advanced-)**

# Constructor and Destructor
```c++
#include<iostream>

class Entity {
public:
	float x, y;

	//Entity() = delete;  // will delete default c++ constructor

	Entity()
	{
		std::cout <<"Entity created!!"<< std::endl;
		x = 0;
		y = 0;
	}

	Entity(float a , float b )
	{
		std::cout << "Entity created!!" << std::endl;
		x = a;
		y = b;
	}
	~Entity()  // Destructor
	{
		std::cout << "Entity destroyed!!" << std::endl;
	}

	void Print()
	{
		std::cout << x << " " << y << std::endl;
	}

};

void Function()
{
	Entity e(1.0f,2.5f) ;
	e.Print();
}

int main()
{
	Function();
	std::cin.get();
}
// Output:
// Entity created!!
// 1 2.5
// Entity destroyed!!
```

**[⬆ Back to Top](#----c-advanced-)**

# Visibility
```C++
#include<iostream>


class Entity {
public:
	float x;   // Accessible everyehere
protected:
	float y;  // Accessible for subclasses
private:
	float z;  // Only Accessible to this class
};

class Player : public Entity
{
public:
	void modify()
	{
		x = 5;
		y = 4;
		// z = 3; //X wrong , because Z is private 
	}

};

int main()
{
	
	Entity e ;  

	e.x = 5;
	//e.y = 6; //X wrong , because y is protected 
	//e.z = 7; //X wrong , because Z is private 
	std::cin.get();
}
```

**[⬆ Back to Top](#----c-advanced-)**
# Inheritance
```C++
class Entity {
public:
	float x, y;

	void Move(float xa,float ya)
	{
		x += xa;
		y += ya;
	}
};
```

```C++
class Player : Entity {   // All properties of Entity are inherited to Player.
public:
	const char* Name;

	void PrintName()
	{
		std::cout << Name << std::endl;
	}
};
```

```C++
  std::cout << sizeof(Entity) << std::endl;  // 8 
  std::cout << sizeof(Player) << std::endl;  // 16 
```

**[⬆ Back to Top](#----c-advanced-)**

# Virtual Function
**In Below case:**
without using virtual "entity entity" is printed with  virtual "entity anshaj" is printed i.e. virtual tells the compiler to see if there is any similar fuction in player entity.
 we can use override in getname() of player to be more sure that GetName() already exists in entity class.
```C++
#include<iostream>
#include<string>

class Entity {
public:
	virtual std::string GetName() { return "Entity"; }
};

class Player : public Entity
{
private:
	std::string m_name;
public:
	Player(const std::string& name)
		: m_name(name) {};
	std::string GetName() override { return m_name; }
};

void printName(Entity* e)
{
	std::cout << e->GetName() << std::endl;
}

int main()
{
	Entity* e = new Entity();
	printName(e);
	Player* p = new Player("Anshaj");
	printName(p);

	std::cin.get();
}
```

**[⬆ Back to Top](#----c-advanced-)**


# Pure Virtual Function 
* Pure Virtual Function in C++ is same as Interface in Java. It ensures that if any class inherits/implements it, requried method is   
present in that class.
```C++
#include<iostream>
#include<string>

class printable {
public:
	virtual std::string GetClassName() = 0; // Pure virtual function.
};

class Entity : public printable{
public:
	virtual std::string GetName() = 0;    // Pure virtual function.
	virtual std::string GetClassName() { return "Entity"; }
};

class Player : public Entity 
{
private:
	std::string m_name;
public:
	Player(const std::string& name)
		: m_name(name) {};
	std::string GetName() override { return m_name; }
	std::string GetClassName() override { return "Player"; }
};
class Anshaj : public printable
{

public: 
	std::string GetClassName() override { return "Anshaj"; }
};

void printName(Entity* e)
{
	std::cout << e->GetName() << std::endl;
}

void print(printable* p)
{
	std::cout << p->GetClassName() << std::endl;
}

int main()
{
	//Entity* e = new Entity();  // Now we can't initialise like this 
	Entity* e = new Player("Amit");  // We should use object like player where GetName() is confirmly implemented

	printName(e);
	Player* p = new Player("Anshaj");
	printName(p);

	print(p);   //Player
	print(new Anshaj()); //Anshaj

	std::cin.get();
}
```

**[⬆ Back to Top](#----c-advanced-)**


# Member Initializer List
```C++
#include<iostream>
#include<string>

class Entity {
private:
	std::string m_Name;
	int m_Score;
public:
	float x, y;

	//Entity() = delete;  // will delete default c++ constructor

	Entity()
	{
		x = 0;
		y = 0;
		m_Score = 100;
		m_Name = "Unknown";
	}

	Entity(std::string name)
	 : m_Name(name), m_Score(0), x(0),y(0) //this way of initialising is called InitialiserList techniqueue.
					       //Always try to initialise the variable in the order of decleration.
                                               //it is also more memory efficient than initialising it under constructor.
	{
	}

	std::string GetName() const
	{
		return m_Name;
	}

	
	

};


int main()
{
	Entity e0;
	std::cout << e0.GetName() << std::endl;

	Entity e1("Anshaj");
	std::cout << e1.GetName() << std::endl;

	std::cin.get();
}
// Output:
// Unknown
// Anshaj
```

**[⬆ Back to Top](#----c-advanced-)**

# Implicit And Explicit Keyword
```C++
#include<iostream>
#include<string>

class Entity {
private:
	std::string m_Name;
	int m_Age;
public:
	Entity(const std::string& name)
		:m_Name(name),m_Age(-1){}

	explicit Entity(int age)
		:m_Name("Unknown") , m_Age(20){}

	std::string GetName() const
	{
		return m_Name;
	}

};

int main()
{
	using namespace std::string_literals;
	// As  Entity(const std::string& name) is not explicit we can declear it 
	// explicitly as well as implicitly
	Entity e0 = "Anshaj"s;
	Entity e1("Anshaj");
	std::cout << 2 << std::endl;

	// But 'explicit Entity(int age)' is explicit
	// Entity e2 = 22;    // X wrong , as implicit declearation is not allowd
	Entity e3(22);
	std::cout << e3.GetName() << std::endl;
	std::cout << e0.GetName() << e1.GetName() << std::endl;
	std::cin.get();
}
// Output:
// Unknown
// Anshaj Anshaj
```  

**[⬆ Back to Top](#----c-advanced-)**

# Operators And Operators Overloading
```C++
// Operator is something which is used in place of func.
// Operator Overloading means changing/defining default behaviour of operator
#include<iostream>
#include<string>

class Vector2 {
public:
	float x, y;
	Vector2(float X,float Y)
		:x(X),y(Y) {}

	Vector2 Add(const Vector2& other) const
	{
		return Vector2(x + other.x, y + other.y);
	}
	Vector2 Multiply(const Vector2& other) const
	{
		return Vector2(x * other.x, y * other.y);
	}
	Vector2 operator +(const Vector2& other) const
	{
		return Add(other);
	}
	Vector2 operator *(const Vector2& other) const
	{
		return Multiply(other);
	}

};

// << ovearloading
std::ostream& operator<<(std::ostream& stream, const Vector2& other)
{
	stream << other.x << ", " << other.y ;
	return stream;
}

int main()
{
	Vector2 position(4.0f, 4.0f);
	Vector2 speed(2.2f, 2.5f);
	Vector2 powerUp(1.1f, 1.1f);

	// Language like java has only this choice because there is no operator overloading
	Vector2 result = position.Add(speed.Multiply(powerUp));
	
	//But in c++ we can also do like 
	Vector2 result2 = position + speed * powerUp;

	std::cout << result.x << " " << result.y << std::endl;
	std::cout << result2.x << " " << result2.y << std::endl;

	//After << oveloading we can simply use
	std::cout << result2 << std::endl;
	std::cin.get();
}
// Output:
// 6.42 6.75
// 6.42 6.75
// 6.42, 6.75
```  

**[⬆ Back to Top](#----c-advanced-)**

# `this` Keyword
```C++
class Entity {
public:
	int x, y;

	Entity(int x, int y)
	{
		this->x = x;
		this->y = y;
		PrintEntity(*this);
	}
};
void PrintEntity(const Entity& e)
{

}
```  

**[⬆ Back to Top](#----c-advanced-)**


# Lifetime
```C++
#include<iostream>

class Entity {
public:
	int x, y;

	Entity()
	{
		std::cout << "Entity Created!!" << std::endl;
	}
	~Entity()
	{
		std::cout << "Entity Destroyed!!" << std::endl;
	}
};

int main()
{
	// Initialised in Stack
	{
		Entity e;
	}  // Entity e will be destroyed as it goes out of scope

	// Initialised in Heap
	{
		Entity* e = new Entity();
	} // Entity e will not get destroyed even after going out of scope

	std::cin.get();
}
// At end of program all memory allocated get cleared from stack as well as heap


// Output:
// Entity Created!!
// Entity Destroyed!!
// Entity Created!!
```

**We can use scope pointer to delete heap memory**
```c++
class ScopedPtr
{
private:
	Entity* m_Ptr;
public:
	ScopedPtr(Entity* ptr)
	:m_Ptr(ptr) {}
	~ScopedPtr()
	{
		delete m_Ptr;
	}
};
```  
now, we can initialise `ScopePtr e = new Entity()` which will be created in heap and will get destroyed when  
we go out of scope


**[⬆ Back to Top](#----c-advanced-)**



# Copy And Copy Constructor
```C++
#include<iostream>

// Making our own string class like std::string in c++
class String {
private:
	char* m_Buffer;
	unsigned int m_Size;
public:
	String(const char* string)
	{
		m_Size = strlen(string);
		m_Buffer = new char[m_Size + 1];
		memcpy(m_Buffer, string, m_Size);
		m_Buffer[m_Size] = 0;
	}

	// String(const String& other) = delete ;  // To stop copying 

	String(const String& other)
		:m_Size(other.m_Size)
	{
		m_Buffer = new char[m_Size + 1];
		memcpy(m_Buffer, other.m_Buffer, m_Size + 1);
	}

	~String()
	{
		delete[] m_Buffer;
	}

	char& operator[] (unsigned int index) const
	{
		return m_Buffer[index];
	}

	//Here friend keyword means allowing function to use private variable
	friend std::ostream& operator<<(std::ostream& stream, String string);

};

std::ostream& operator<<(std::ostream& stream, String string)
{
	stream << string.m_Buffer; 
	return stream;
}

void printString(const String& string)  // If we pass here by value copy constructor will be called
					// means new String class allocated in heap and after execution of this fuction destructor
					// will be called... That's take time so be just pass by refrence and marks as const
{
	std::cout << string << std::endl;
}

int main()
{
	String string = "Anshaj";
	String string2 = string; // without copy constructor it will just copy during copying 
							//`char* m_Buffer;` pointer value will be same for both
	string2[0] = 'a';
	printString(string);
	printString(string2);
	std::cin.get();  // If we do not use copy constructor program is going to crash after hitting enter.
	            // Because `delete[] m_Buffer;` in destructor of string goint to free same memory two times
}
// Output:
// Anshaj
// anshaj
```


**[⬆ Back to Top](#----c-advanced-)**


# Arrow Operator & Scope Pointer
```C++
#include<iostream>

class Entity
{
public:
	void print() const { std::cout << "Hello!!" << std::endl; }
};

class ScopedPtr
{
private:
	Entity* m_Ptr;
public:
	ScopedPtr(Entity* ptr)
		:m_Ptr(ptr) {}
	~ScopedPtr()
	{
		delete m_Ptr;
	}
	Entity* operator->() 
	{
		return m_Ptr;
	}
	const Entity* operator->() const
	{
		return m_Ptr;
	}

};

int main()
{
	Entity e;
	e.print();

	Entity* ptr = &e;

	// Both are same...2nd one is good
	(*ptr).print();
	  ptr->print(); 

	  // ScopePtr will delete the heap momory after going out of scope.
	  ScopedPtr e1(new Entity());
	  e1->print();
	  const ScopedPtr e2(new Entity());
	  e2->print();
}
// Output:
// Hello!!
// Hello!!
// Hello!!
// Hello!!
// Hello!!

```


**[⬆ Back to Top](#----c-advanced-)**


# Determining Offset

```C++
#include<iostream>

struct Vector3
{
	float x, y, z;
};

int main()
{
	// offset of x,y,z in memory. 
	int offsetX = (int)&((Vector3*)nullptr)->x;
	int offsetY = (int)&((Vector3*)nullptr)->y;
	int offsetZ = (int)&((Vector3*)nullptr)->z;

	std::cout << offsetX << std::endl;
	std::cout << offsetY << std::endl;
	std::cout << offsetZ << std::endl;

	std::cin.get();
}

// Output:
// 0 
// 4
// 8
```


**[⬆ Back to Top](#----c-advanced-)**

# Vector Copy Techniques

```C++
#include<iostream>
#include<vector>
struct Vertex {
	float x, y, z;
	Vertex(int x,int y,int z)
		:x(x),y(y),z(z) {}

	Vertex(const Vertex& vertex)
		:x(vertex.x), y(vertex.y), z(vertex.z)
	{
		std::cout << "Copied!!" << std::endl;
	}
};
int main()
{
	// copied 6 times
	std::vector<Vertex> vertices;
	vertices.push_back(Vertex(1, 2, 3));
	vertices.push_back(Vertex(4, 5, 6));
	vertices.push_back(Vertex(7, 8, 9));

	// copied 3 times
	std::vector<Vertex> vertices2;
	vertices2.reserve(3);
	vertices2.push_back(Vertex(1, 2, 3));
	vertices2.push_back(Vertex(4, 5, 6));
	vertices2.push_back(Vertex(7, 8, 9));

	// copied 0 times
	// i.e. most efficient way
	std::vector<Vertex> vertices3;
	vertices3.reserve(3);
	vertices3.emplace_back(1, 2, 3);
	vertices3.emplace_back(4, 5, 6);
	vertices3.emplace_back(7, 8, 9);

	std::cin.get();
}

// Output:
// Copied!! -> 9 times
```


**[⬆ Back to Top](#----c-advanced-)**

# Static and Dynamic Linking
*  Static linking happens at compile time but dynamic linking happens at runtime.  
*  Static linking includes the files that the program needs in a single executable file.

*  Dynamic linking is what you would consider the usual, it makes an executable that still requires DLLs and such to be in the same directory (or the DLLs could be in the system folder). (DLL = dynamic link library)

***Dynamically linked executables are compiled faster and aren't as resource-heavy.***



**[⬆ Back to Top](#----c-advanced-)**


# Templetes
```C++
#include<iostream>

// Here print function DNE, it is created when we call it using argument
template<typename T>
void print(T value)
{
	// If we do some error here and do not call print func any time compiler will work fine
	// as print DNE A/C to compiler
	std::cout << value << std::endl;
}

int main()
{	
	// we can use like this
	print<int>(5);

	// It's a better way
	print(5);
	print("Anshaj");


	std::cin.get();
}

// Output:
// 5
// 5 
// Anshaj
```


**[⬆ Back to Top](#----c-advanced-)**


# Function Pointers
```C++
#include<iostream>
#include<vector>
#include<string>

template<typename T>
void print(T value)
{
	std::cout << value << std::endl;
}
template<typename T>
void ForEach(const std::vector<T>& values, void(*func)(T))
{
	for (auto value : values)
		func(value);
}

int main()
{	
	std::vector<int> values = { 1,2,3 };
	ForEach(values, print);

	std::vector<std::string> values2 = { "Amit" , "Anshaj" };
	ForEach(values2, print);

	std::cin.get();
}

// Output:
// 1
// 2
// 3
// Amit
// Anshaj

```


**[⬆ Back to Top](#----c-advanced-)**

# Lambdas
*  [Reference](https://en.cppreference.com/w/cpp/language/lambda)
*  It is like a virtual function used instead of a function.  
*  & (implicitly capture the used automatic variables by reference) and
*  = (implicitly capture the used automatic variables by copy).

* mutable: allows body to modify the objects captured by copy, and to call their non-const member functions
```C++
#include<iostream>
#include<vector>
#include<string>

void ForEach(const std::vector<std::string>& values, void(*func)(std::string))
{
	for (auto value : values)
		func(value);
}

int main()
{	
	
	std::vector<std::string> values2 = { "Amit" , "Anshaj" };
	// Here,Lambda function is used instead of a real function
	ForEach(values2, [](std::string value)
	{
		std::cout << value << std::endl;
	});
	// same but more clear way
	auto lambda = [](std::string value) {std::cout << value << std::endl; };
	ForEach(values2, lambda);
	// [=] : means capture by value
	// [&] : means capture by value
	std::cin.get();
}

// Output:
// Amit
// Anshaj
// Amit
// Anshaj

```


**[⬆ Back to Top](#----c-advanced-)**

# Threads
*  The class [thread](https://en.wikipedia.org/wiki/Thread_(computing)) represents a single [thread](https://en.cppreference.com/w/cpp/thread/thread) of execution. Threads allow multiple functions to execute concurrently.
```C++
#include<iostream>
#include<thread>

static bool s_Finished = false;
void DoWork()
{
	using namespace std::literals::chrono_literals;

	std::cout << "Strated thread id = " << std::this_thread::get_id() << std::endl;

	while (!s_Finished)
	{
		std::cout << "Working....\n";
		std::this_thread::sleep_for(0.5s);
	}
}

int main()
{
	std::thread worker(DoWork);

	std::cin.get();
	s_Finished = true;

	// This means we are saying main thread to wait until worker
	// thread completes its execution
	worker.join();

	std::cout << "Strated thread id = " << std::this_thread::get_id() << std::endl;
	std::cout << "Finished....\n";
	std::cin.get();
}
// Output:
// It will print "Working....\n" in every 0.5s until we press enter
// After hitting enter "Finished....\n"; eill be printed
// Thread id printed will be different for worker and main thread 

```


**[⬆ Back to Top](#----c-advanced-)**


# Timings
```C++
#include<iostream>
#include<chrono>
#include<thread>

struct Timer
{
	std::chrono::time_point<std::chrono::steady_clock> start, end;
	std::chrono::duration<float> duration;

	Timer()
	{
		start = std::chrono::high_resolution_clock::now();
	}
	~Timer()
	{
		end = std::chrono::high_resolution_clock::now();
		duration = end - start;

		float ms = duration.count() * 1000.0f;
		std::cout << "Timer tooks " << ms << "ms " << std::endl;
	}
};

void function()
{
	Timer timer;
	for (int i = 0; i < 100; i++)
		std::cout << i << " ";

}

int main()
{
	using namespace std::literals::chrono_literals;

	auto start = std::chrono::high_resolution_clock::now();
	std::this_thread::sleep_for(1s);
	auto end = std::chrono::high_resolution_clock::now();

	std::chrono::duration<float> duration = end - start;
	std::cout << duration.count() << std::endl;
	function();
	std::cin.get();
}
// Output:
// will be very close to 1 in my case 1.00025
// Timer tooks 39.1761ms
```


**[⬆ Back to Top](#----c-advanced-)**

