<u>Strings</u>

- Array of chars(char * or char[])
- Easy to corrupt (overwrite '\0')
- must explicitly allocate memory



<u>In C++</u>

```C++
"import <string>;"
  std::string -> type
  - it grows as needed
  - safer to manipulate
  
// C++
  string s = "Hello";
	s = "Hi";
```

<u>Strings Operations</u>

```C++
- Equality (s1 == s2, s1 != s2)
- Comparison: s1 <= s2
- Length (s.length()) //O(1)
- individual chars (s[0], s[1])
- concat: s3 = s1 + s2
  				s3 += s4
- cin >> s; //!!! read a word
							- ignores leading white space
              - stops at next white space
- getline(cin(stream), s(string))  // read in "a line"
```

Example in VSCODE



**<u>Streams</u>** are <u>**abstraction**</u> 

Abstraction:

- Wrap an interface of getting and putting items to keyboard and screen

**<u>Files</u>** (file streams is an <u>abstraction</u>)

- Read/write from/to a file instead cin/cout

- ```C++
  std::ifstream //a file stream for reading
  std::ofstream // a file stream for writing
  
  Ex: in C
  #include <stdio.h>
  int main () {
    char s[256]; // Hoping no words is larger than 255 chars
    FILE *f = fopen("file.txt", "r");
    while (1) {
      fscanf(f, "%255s", s);
    }
  }
  
  in C++:
  import <iostream>;
  import <fstream>;
  using namespace std;
  
  int main () {
    ifstream f{"file.txt"}; // Declaring and initializing opens the file
    string s; // declare string variable; no concerns about size of the words
    while (f >> s) { //f getting from s
      cout << s << endl;
    }
  } // not closing the file because of "f". file is closed when it is out of scope
  ```



<u>Command Line Arguments</u>:

```C++
./program abc 123

//To access these args:
int main(int argc, char* argv[]) {
  // argc: # of arrays given to the program - including program name
  // argb -> array of char *s, i.e - C style Strings
  // argv[c] - name of program
  // argv[1] - 1st arg
  // argv[2] - 2nd arg
  ..
  // argv[argc] - Null Pointer
}
```

Stack

argv: _ _ _ _

Index 0: ./program\0

Index 1: a b c /0

Index 2: 1 2 3 /0

Index 3: NULL



!!! **<u>WRONG</u>**

```C++
if (argv[1] == "abc") // Comparing pointer locations instead of String contents!!!!
```

Advice: Convert the args to **<u>std::String</u>**

Before use: 

```C++
int main(int argc, char **argv) { // char**argv Equivalent to *argv
  for (int i = 0; i < argc; ++i) {
    String arg = argv[c];
    if (arg == "abc") {} //WORKS!
  }
}
```

Ex: Add <u>all integers</u> given as args on the cmd Line:

```C++
int main (int argc, char *argv[]) {
  int sum = 0;
  for (int i = 1; i < argc; ++c) {
    string arg = argv[i];
    int n;
    if (istringstream iss{arg}; iss >> n) sum += n;
  }
  cout << sum << endl;
}
```



**<u>Default Function Parameters</u>**

```c++
void printSuiteFile (string fileName = "suite.txt") {
  ifstream file {fileName};
  string line;
  while (getline(file, line)) cat << line << endl;
}

// printSuiteFile("abc.txt") -> Read from abc txt
// printSuiteFile() -> Read from file - suite.txt
// Notes: optional parameters must be last
```



Who's responsibility is it to provide the default argument? Who writes suite.txt into the stack frame of arguments?

- The caller sets up the argument, not the function itself
- printSuiteFile() is replaced with printSuiteFile("suite.txt") where it is found

void f(int x, string s = "Hello")

Int main() { f(5); }

stack: _ _ main_ (see WeChat visualization)



The defaulted function (f) 

- does not know what will be passed in
- It **cannot** detect the difference between <u>uninitialized memory</u> and <u>o</u><u>ther strings</u>
- Caller of f must provide <u>**all arguments**</u> to f in advance

**<u>Note</u>**: Default parameters are part of the **<u>declaration</u>**, not necessary definition

```
in Header (interface) file:
void f(int x; string s = "Hello");

// in .c file
void f(int x, string s) {}
```

Default parameters are given in the <u>**interface (header file in this case)**</u>, **not** the implementation (.c / .cc file)



<u>Overloading</u>

- if I want a function to negate ints and bool

  - in C: 

    ```C
    int negInt(int x) {return -x;}
    bool negBool(bool b) {return !b;}
    ```

  - in C++:

    ```C++
    int neg(int x) {return -x;}
    bool neg(bool b) {return !b;}
    // neg(5) -> -5
    // neg(true) -> false
    ```

- Compiler chooses which **<u>overload</u>** of the function to use based on the **#** and the **<u>types of the args</u>** at compile line.
  - seen this in the case of operators: ==, <<, >>, +
  - We cannot overload based on the **return** **type** **alone**
  - can't have same args in overloading functions. s.t. f(int x); f(int x)



**<u>Structs</u>**

- Structs also exist in C++, just as they existed in 

  - Ex: Linked list node:

    ```C++
    struct Node {
      int data;
      Node* next;
    }; // dont forget ;
    ```

In C: this would be Struct Node*

in C++: **omit** the struct



Recall: why is this invalid?

```C
struct Node {
  int data;
  Node next;
};
```

- What is the size of the struct?
  - we need to solve the following:
    - **sizeof(Node) = sizeof(int) + sizeof(Node)** X=4+X
  - The previous (*Node) works because ptrs are if fixed size;
    - 8 bytes no matter what you point at



**<u>Constant</u>**

```C++
const int max_grade = 100;
// Any attempt to modify this variable wont compile

const Node n {5, nullptr};
// n's data cannot be changed and n's next field cannot be changed
```

In C:

```C
// The null ptr is inficated via NULL or 0
// wont do this in C++!!!
```

- in C++:

  - Typically in C - there is the following line imported via a library

    - #defines NULL 0 -> replaces all instances of NULL with 0
    - consider following scenario:

    ```C++
    void f(int x);
    void f(Node* p);
    
    1. calls f(NULL) - calls int version of f instead of pounter version
    
    
    // nullptr is a special type that can be automatically converted to any other ptr type
    2. f(nullptr) -> calls ptr version of f
    ```

    

**<u>Parameter Passings</u>**:

```c++
void inc(int x) {++x;}

int main() {
  int x = 5;
  inc(x);
  cout << x <<endl; // output 5 because we made a copy of 5 and call the function (AKA pass by value semantics)
}
```

**<u>Pass-by-value semantics</u>**: Copy arguments into the function instead of using orginal.



```C++
void inc(int *p) {++*p;}

int main() {
  int x=5;
  inc(&x); // pass by reference
  cout << x << endl;
}
```

- If we want to mutate an argument and change the value outside the function
  - pass a pointer



C++ has another pointer-like type: **<u>Reference</u>**

References:

```C++
int x = 5;
int& z=x; // any changes to z will be reflected in x (lvalue)
// z is an "alias" to the value of x
// anytime you see z, simply imagine replacing it with x

++z;
cout << z <<" "<<x<<endl;
// z=x=6
```

- References are somewhat like **<u>const ptrs</u>** with automatic dereference.

- We cannot change what a reference is aliasing

- When does & mean reference vs when it is address-of?

  - & in a **<u>type</u>** means references, **<u>in an expression it means address-of</u>**
  - f(int& x) // int&: reference
  - Int* y = &x; // expression: address-of

- Things you **cannot** do with references:

  - Leave them uninitized, must alias a variable at all times. (**<u>MUST BE INITIALIZED</u>**)

  - ```C++
    int &z; //INVALID: WONT COMPILE
    ```

    

- Lvalue references must always be initialized with lvalues

- Lvalue: a value you may take the address-of

  - Rvalue below:

  - ```C++
    int& z = 5; //cannot take the address of 5, it is not an lvalue - it is an rvalue
    ```

  - lvalues have a "name" - which tells you where in memory the variable is stored!

    - rvalue again: 

      ```C++
      int& z = x + y; // rvalue - tempaorary variable
      int& z = f(); // wont compile
      ```

      

- We cannot **create a pointer** to a **reference**:

  - read from right to left
    - int&*x; // x *wont* compile, ptr to a reference
    - Int*&x; // reference to a ptr - this is *allowed*

- Cannot create  a **refernce** to a **reference**

  - ```c++
    int&& x; //This is not a reference to a reference;
    				 // this is a rvalue reference
    
    ```

- Cannot create <u>an array of references</u> 

  - ```c++
    int&arr[3] = {x,x,x}; // X wont compile
    // Reference Assignment: Once a reference is initialized, it cannot be reassigned to refer to a different object. If you had an array of references, you wouldn't be able to change what they refer to after initialization. 
    ```

  - what are references useful for?

    - Most commonly: Use as **<u>args</u>** to a function

- ```C++
  // we saw
  inc(int x) and inc(int* p) {++*p;}
  ```

- with references:

- ```c++
  void inc(int& x) {++x;} // automatically dereference
  x = 5;
  inc(x);
  cout<<x<<endl; //6
  ```

- How does int x; cin>>x work?

  - ```c++
    cin >> x // actually just a fn call
    // how it is implemented in stardard library
    istream& operator>>(istream& in, int& x) {
      // read from in into X
      return in;
    }
    ```

- Is there a good reason why we take in and <u>return istream&</u> instrad of an istream?

  - **pass by value** can be an <u>expensive</u> operation

  - copying memory from caller's stack frame into called stack frame

  - ex:

  - ```C++
    struct ReallyBig {...}; 
    void f(ReallyBig rb) {...};// no changes reflected, pass by copy (copy all mempory in rb -> expensive)
    void g(ReallyBig& rb) {...} // Changes are reflected, not expensive, this is fast- 8 bytes copied
    void h(const ReallyBig& rb) {...} //FAST; rb wont be changed; should be a good choice
    ```

    

- When designing a function with argiments 

  - **<u>pass-by-const-reference</u>** should be <u>first</u> choice

  - use pass-by-ref only if you **need** to changes reflected outside function call

  - ```c++
    pass-by-value: 2 scenarios;
    1) Arg is small-pointer size of smaller (< 8bytes)
    2) void a(const ReallyBig& rb) {
      ReallyBig temp {rb}; // just use pass-by-value instead, copies for you ???
    }
    ```

    

- ```c++
  in istream& operator>>(istream& in, int&x);
  // pass cin via reference to avoid expenses of copying
  // (streams have copying disabled - copying does not compile)
  void f(int&x); // can only pass by lvalue
  void g(const int&y); // can pass both lvalue and rvalue: g(z), g(5) workss
  ```

  

- **<u>const</u>** - lvalue references like in g(ex above) can bind to rvalues (tempraries) because we know they won't be changed
  - Compiler create <u>a tempory location</u> and store the <u>temporary</u> (ex: g(5) where 5 is the temporary memory) inside





**<u>Dynamic Memory</u>**

- In C:

- ```c
  int *p = malloc(n * sizeof(int)); //dynamic array of int
  ... //maybe resize...
  free(p); // give memory back to OS
  ```

- In C++: use new/delete

- ```c++
  Node* np = new Node {5, nullptr};
  ...
  delete np; // give back memory to OS; follows np's memory address and heap and delete it, but in stack, it changes nothing
  ```

- <u>Important</u>!

  - All local variables rewrite space on the <u>stack</u>
  - use new to acquire memory from the <u>heap</u>
  - calling <u>delete</u> returns memory to OS to be reused,
  - does not affect value of pointer at all
  - Stack allocated vars; // automatically deallocated when they leav scope
  - Heap allocated memory exists until delete is called
  - A <u>memory leak</u> occurs when memory that is allocated with <u>new is new deleted</u>.

- C++: dynamic arrays;

- ```c++
  int* arr = new int[10]; // array of 10 ints on heap;
  												// arr points to first element in the arr
  ...
  delete[] arr; // frees a dynamically allocated array
  ```

- Always pair **<u>new + delete</u>** new[] with delete[]

- Do not mix and match or else undefined behaviour



Lecture 5

We have seen for args: pass-by-value, pass-by-ptr, pass-by-ref

Which is the best option for returns? // 

```c++
Node getMeANode(int n) {
  return Node {n, nullptr}; // return-by-value
}
```

This works - make a copy from getMeANode's stack frame into the caller's frame: Too slow?



not working version:

```c++
Node* getNodeptr(int n) {
  Node x {n, nullptr};
  return &x; // wrong: it returns the pointer into the stack frame; returing a dangling pointer;
  					 // x will be cleaned up when fn returns, pointing at memory we no longer own.
}
```

working version:

```c++
Node* getNodeptr(int n) {
	Node* p = new Node {n, nullptr};
  return p;
}
```

This is fast and works **<u>BUT</u>** user must remeber to **<u>delete</u>** heap allocated memory, or else <u>**memory leak**</u>



Return by Ref:

```c++
Node& getNodeptr(int n) {
  Node x{n, nullptr};
  return x; // still not working because acess to the alias of the node, we still lose x
  				  // dangling ref - same problem as ptr example
}
```



- Returning by reference is rare.

- Returning a reference to stack allocated variables will cause undefined behaviour.

- Only use return-by-ref if returning a ref to a non-local variables. Eg:

- ```c++
  istream& operator>>(istream& in, int& x) {
    ...
    return in; // works because in is not local variable
  }
  ```

- Advice: Usually, **<u>return-by-value</u>**: easy to use, not as slow as it may seem (more semantics)



<u>**Operator Overloading**</u>

- Defined the meaning of operators acting on our own types

- ```c++
  struct vec {
    int x, y;
  };
  
  vec operator+(const vec& v1, const vec& v2) {// const reference of v1; aliases v1
    vec r{v1.x+v2.x, v1.y+v2.y};
    return r;
  }
  
  vec operator*(int k, const vec& v) {
    return {k*v.x, k*v.y}; // vec could go here to explicityly construct a vec, obied, but assumed by return type
  }
  
  vec z = w * 2; //This doesnt mach our first overload - order of args is wrong
  
  vec operator*(const vec& x, int k) {
    return k*v; // int * vec from the previous function. calls the other operator*
  }
  ```
  
  Example: Overloading << and >>
  
  ```c++
  struct grade { // support: grade g; cin>>g; cout<<g;
    int n;
  };
  
  istream& operator>>(istream& in, grade& g) {
    in>>g.n;
    if (g.n < 0) g.n=0;
    if (g.n > 100) g.n=100;
    return in;
  }
  
  ifstream file {"file.txt"};
  grade g;
  file>>g; // works
  
  ostream& operator<<(ostream& out, const grade& g) {
    return out<<g.n<<'%';
  } // generally, no need for operator<<, it would force users to print newlines
  ```





<u>**Separate Compilation**</u>

Point: Speed up compilation times, by only recompiling what we need to

Recall:

- Interface files (think .h files): provide declarations
- Inplementation files (think .c): provide definitions

```c++
// interface (vec.cc)
export module vec; // indicates an interface file
export struct vec z { // Anything marked export clients may use
  int x,y;
}
export vec operator+(const vec& v1, const vec& v2);

// Client code (main.cc)
import vec;
int main() {
  vec v{1,2};
  vec w = v+v;
}

// Implementation file: (vec-implementation.cc)
module vec;
vec operator+(const vec& v1, const vec& v2) {
  return {v1.x+v2.x, v1.y+v2.y};
}
```

Recall:

- we can repeatedly declare structs/functions, but we may only define once.
- Interface files: export module x;
- implementation files: module x;



To create an object file (.o file):

```c++
g++20m vec.cc -c // -c means compile only
  							 // creates vec.o
```

with c++20 modules, object files must be created in **<u>dependency</u>** order

* <u>First</u> compile **interface files**, then **implmentation** / client files!!! (ORDER BELOW)
  * vec.cc
  * vec-impl.cc
  * main.cc

```c++
g++20m vec.o vec.impl.o main.o -o program //links object files
```



Building dependencies with Make in progress.

```c++
g++20m *.cc -c // fail on some which are out of order 
// can write g++20m *.cc without -c
g++20 *.cc -c // create remaining .o files
g++20m *.o -o program // linking
```





**<u>CLASSES</u>**!!

- we can put functinns inside our structs 

  - // Student.cc

  - ```c++
    export module Student;
    export Struct Student {
      int assns, mt, final;
      
      float grade();
    }
    ```

  - // Student-impl.cc

  - ```c++
    float Student::grade() { // Student:: means "scope resolution operator"
      return 0.4*assns + 0.2*mt + 0.4*final;
    }
    
    // C::f - inside of the Class C - define, member function f
    ```

    

  - ```c++
    Student s{60, 70, 80}; // s - is an object, instantiation of a class
    s.grade(); // returns 60*0.4+70*0.2+80*0.4
    
    // Student - this is a class - struct with functions defined inside it.
    // grade() function calls member functions / method
    ```

  - Inside Student::grade we use variables assns, mt, final

  - These variables are bound to the object the method is called on

  - ```c++
    s.grade() // assns is s.assns
    					// mt is s.mt
    
      
    // When we call s.grade(), there is a hidden parameter that is used to access assns, mt, final
    this -> a pointer to the object the methods was called.
     
    s.grade() // inside grade, this == &s
      
      
      
    // we could write:
    export struct Student {
      int assns, mt, final;
      // method
      float grade() { 
    // provide mthod defitions inside of class - but generally best to separate into interface/implementation file
        return this->assns * 0.4+ this->mt * 0.2 + this->final * 0.4;
      }
    };
      
    ```

​		

<u>Initializing objects</u>

```c++
Student S {60, 70, 80};
- To better control initialization, write a constructor (ctor) will get called whenever a Student object is created.
  
Struct Student {
  Student(int assns, int mt, int final) {
    // name of the method is the name of the class
    this->assns = assns;
    this->mt = mt;
    this->final = final;
  }
};

// using this-> to differ between arg and field

Student s{60, 70, 80};
Student *p = new Student{60, 70, 80};
//Invokes a ctor call - passed as args to ctor method.
// If we dont write a ctor - we get C-style field-be-field initialization

Student S = Student{60, 70, 80};
Same as Student S{60, 70, 80};
// Benefit of ctors: write our own logic - use them as functions. eg: default params:
Sturct Student { // default constuctor
  Student(int assns=0, int mt=0, int final=0) {
    this->assns = assns;
    this->mt = mt;
    this->final = final;
  }
};

Student s(60, 70, 80);
Student newkid; // newkid sets to 0
Student toby{40, 50}; // final=0 for default

Student newKid; // calls our 0 -arg ctor
A ctor that may be called with 0 args;
This is a """default ctor""". If you do not write a default ctor - compiler provides one. 
// As soon as you write a ctor - compiler procided default ctor goes away
  
```



Compiler provides default ctor:

- <u>Primitive</u> types - leave them uninitialized //原始type
- Classes/ Structs - calls their default ctor.



1. **Primitive Types**:

   - Examples of primitive types include `int`, `float`, `double`, `char`, etc.

   - If you create a variable of a primitive type without initializing it, the value of that variable is left uninitialized in the case of local (stack) variables. This means the value is essentially whatever garbage data was in that memory location. For global or static variables, they are zero-initialized by default.

     ```
     cppCopy code
     int a;    // uninitialized value for local variable
     static int b; // zero-initialized
     ```

2. **Classes/Structs**:

   - If you define a class or struct without a default constructor, the compiler will implicitly define a default constructor for you, as long as there isn't any user-defined constructor present.

   - The compiler-provided default constructor will do the following:

     - It will call the default constructors for any member objects.
     - It will leave primitive type members uninitialized (just like the rule for standalone primitive types).

     For instance, consider the following example:

     ```
     cppCopy code
     struct Point {
         int x, y;
     };
     
     class Circle {
         Point center;
         double radius;
     };
     ```

The default constructor provided by the compiler will be called. This constructor will call the default constructor of `Point` (which in this case does nothing and leaves `x` and `y` uninitialized) and leaves `radius` uninitialized as it is a primitive type.



```c++
Struct vec {
  vec(int x, int y)  {
    this->x = x;
    this->y = y;
  }
};
// vec v; // should call default ctor but none exist - doesnot compile.

Struct Basis {
  Vec v1, v2;
};
// Basis b; what happened?
// vec is the default ctor. (calls default constructor)
// wont compile, we cannot default construct v1 and v2 since it needs two parameters

Struct Basis {
  vec v1, v2;
  Basis() { // does not compile
    v1 = vec{0, 1}; // if write sth like this, they should have been initialized, but they are not
    v2 = vec(1, 0);
  }
}; // doesnt work because v1 and v2 must have values initialized by the time the function starts to run;
//v1 and v2 are being assigned values inside the constructor body. This means they are default-constructed first and then assigned new values inside the constructor. If the vec type doesn't have a default constructor, this would result in a compilation error.


Instead, you should use an initializer list to directly initialize v1 and v2:

Struct Basis {
  vec v1, v2;
  Basis() : v1{0, 1}, v2(1, 0) {
  }
};
// Now, v1 and v2 are directly initialized with the provided values when an instance of Basis is created. This is more efficient and avoids potential issues if vec does not have a default constructor.
```



**<u>Steps of object creation</u>**

1) Space is allocated
2) save for later
3) Fields are initialized
4) Ctor body runs



**<u>MIL</u>** - Member Initialization List

- Let us provide values for fields in str

- ```c++
  Student::Student(int assns, int mt, int final):
  	assns{assns}, mt{mt}, final{final} {...}
  //Field names{values} -> step 3    {ctor body} -> step 4
  
  //Back to the pre example
  Struct Basis {
    vec v1, v2;
    Basis(): v1{0, 1}, v2{1,0} {}
  };
  ```

- More generally, Overload

- ```c++
  Struct Basis {
    Vec v1, v2;
    Basis(): v1{1,0}, v2{0,1} { }
    Basis(const Vec& v1, const Vec& v2): v1{v1}, v2{v2} {} //copy ctor
  }
  However, you are copying their values into the v1 and v2 member variables of the Basis object. Once those values are copied, you can modify the member variables, but not the original objects passed to the constructor.
  
  // Provide defaults for MIL
  Struct Basis {
    Vec v1{0, 1}, v2{1,0}; // default values, used if nothing is specified in MIL
    Basis() { }
    Basis(const Vec& v1, const Vec& v2): v1{v1}, v2{v2} {} //copy ctor
  }
  ```

- Fields in the MIL will always be **<u>initialized</u>** **<u>in the order</u>** they are declared in the class.

- MIL can also be **<u>more efficient</u>** than using <u>ctor body</u>

- ```c++
  Struct Student {
    int assns, mt, final;
    string name;
    Student(int assns, int mt, int final, const string& name) {//name set to empty string
      this->assns = assns;
      ...
      this->name = name; // overwriting ""(empty string) with name
    }
  };
  
  Use MIL:
  Student(int assns, int mt, int final, const string& name):
  	assns{assns}, mt{mt}, final{final}, name{name} {...} //directly set name to name. more efficient!!
  
  // MIL is required: Object fields want default ctors
  								//  const fields and reference fields
  
  //1. Object fields want default actors: It's often the case that object fields (i.e., fields that are instances of classes) have default constructors that allocate resources or set initial states. By using the MIL, you can avoid calling the default constructor only to immediately overwrite the value inside the constructor body. This can be more efficient.
  
  //2. const fields: In C++, if a member variable is declared const, it can only be initialized and cannot be assigned a value afterward. Therefore, for const member variables, you must use a MIL. Otherwise, you won't be able to set their value.
  
  //3. Reference fields: Like const fields, reference fields in C++ must be initialized when an instance is constructed and cannot be reassigned later. Again, you must use a MIL to set their value.
  
  
  // using MIL as much as possible!
  	
  ```

  



- <u>Copy ctor is running</u>
  - Purpose: create 1 object as the object of another
  - Ex: Student s {60,70,80}
    - ​	      Student r{s}; same as Student r = s; // copy ctor is running



- Compi;er gives you the following if you dont write them;
  - DEFAULT CTOR, DESTEUCTOR, <u>COPY CTOR</u>,
  - COPY assignment operator, move ctor, move assivnment operator



<u>Copy Ctor for Student</u> 

```c++
struct Student {
  int assns, mt, final;
  Student(const Student& other): //const - dont make a copy inside of function, not need to change "other"
  	assns{other.assns}, mt{other.mt}, final{other.final} {}
};

This is a copy ctor - replicates compiler provided copy ctr which inst sets this fields to other;s fields
What is a case where compiler provided copy ctor doesnt work?
  
Struct Node {
  int data;
  Node* next;
};
Node* n = new Node {1, new Node{2, new Node {3, nullptr}}};
Node p {*n};
Node* q = new Node {*n};

// Stack:              Heap:
n ----------------> "1"->"2"->"3""X"     "X" means nullptr // n is pointer
p                   "1"->⬆️						// p is not a ptr, it is a Node; copy ctor
q ----------------> "1"->⬆️      			//is a pointer on the stack that points to the heap; is dereference of n
// they are sharing info (pointer)
  
This is an example of compiler provided copy ctor being wrong: Performs a shallow copy, data is shared amomg lists. We want a deep copy: 3 independent lists with the same info.
  
Here how we do:
struct Node {
  int data;
  Node* next;
  Node(const Node& other):data{other.data},
  	next{other.next? new Node {*other.next}:nullptr} {} // Recursively calls copy ctor. 
  																											// other.next is a pointer
  																											// if other.next is a nullptr: return false
};

// Stack:              Heap:
n ----------------> "1"->"2"->"3""X"     "X" means nullptr // n is pointer
p "1"->"2"->"3""X"						
q ----------------> "1"->"2"->"3""X"      			
```



When is copy ctor called:

- Initialize an object from another of same type
- Pass by value 
- Return by value 

```c++
struct Node {
  Node(int data, Node* next=nullptr): data{data}, next{next} {}
};
Node n {5, nullptr};
Node m{6};
Node p = 100; // implicit conversion; invodes the single arg ctor for Node

string s = "Hello"; // single arg ctor for string that takes in a const char*
//stad::String  const char*

void f(Node n);
f(Node {5});
f(5) // arg 5 is converted into a Node
```



Why might you want to disable this?

- Compi;er does conversion implicitly 
- No warning or indication the conversion will occur
- Potential to miss errors



**<u>Makde the ctor explicit: (to make is disable single arg ctor)</u>**

```c++
struct Node {
  explicit Node(int data, Node*next=nullptr): data{data}, next{next} {}
};

Node p = 4; //DOES NOT COMPILE
f(4); // DOES NOT COMPILE
//only
Node p{4}; 
f(Node {4});
```



**Destructors (dtors)**

- Runs whenever <u>an object is destroyed</u>
- when an object <u>goes out of scope</u> (stack allocated) or when <u>delete</u> is called on a ptr to that object (heap allocated)
- Compiler provided dtor: calls dtor on all object fields
- STeps
  - Dtor body runs
  - object fields have their dtors called in *reverse* declaration order
  - *Later*
  - Space is deallocated

```c++
Nodes: should write our own dtor
Node *p = new Node{1, new Node{2, new Node{3, nullptr}}}; // p is in the stack pointing to "1" Heap 1->2->3X
delete p; // still memory leak
					// only delete "1", not NOdes 2 and 3
// we need to write dtor - handle cleaning up rest of linked list focus

struct Node {
  ...
  ~Node() {
    delete next; // delete nullptr does nothing
    						 // recursively deletes rest of the list, base case is delete nullptr
  }
};

Node p {4, new Node {5, nullptr}};
// Entire linked list is freed when p goes out of scope. 
```



**<u>Copy Assignment Operator</u>**

```c++
Student s {60, 70, 80};
Student r = s; // copy ctor
Student t{0, 0, 100};
r = t; // r exists alrdy, and needs a 
			 // copy assignment operator
```

- **<u>Assign</u>** one object to another of the same type
- Compiler <u>provided copy assignment operator</u>; Does **<u>field-by-field assignemnt</u>**

```c++
Node n{1, new Node{3, new Node{3, nullptr}}};
Node p{4, new Node {5, nullptr}};
p = n; //leads to copy assignemnt operator will run

   Stack           Heap
n   "1"   ------>  "2" -> "3X"
p   "1"   --------^ "5X" // "5X not freed - memory leak"
     

     
struct Node {
  Node& operator = (const Node& other) {
    delete next; // purpose: delete "5X" above. delete an object will recursively called until delete nullptr
    data = other.data;
    next=other.next?new Node{*other.next}:nullptr;
    return *this;
  }
};
// support chained assignment: return type of operator "=" is Node&
int a, b, c, d;
a = b = c = d = 100; // d=100 returns d     d would be this, 100 would be other
										 // c=d returns c       c would be this, d would be other
										 // a=b returns b       

n = n; // this will lead an issue cuz we delete all our own nodes by (delete next;) and try to copy them
			 // self-assignment

// so, we need self-assignment check:
struct Node {
  Node& operator = (const Node& other) {
    if (this==&other) return *this;
    delete next; // purpose: delete "5X" above. delete an object will recursively called until delete nullptr
    data = other.data;
    next=other.next?new Node{*other.next}:nullptr;
    return *this;
  }
};

?? if this still work?  MIDTERM QUESTION NEEDED TO THINK ABOUT: WORKS
if (*this==&other) return *this;

!!!
// one more improvement: if we request memory with new, there is a chance it may fail. Better strategy, request memory before deleting Nodes.
struct Node {
  Node& operator = (const Node& other) {
    if (this==&other) return *this;
    Node* temp = other.next?new Node{*other.next}:nullptr; // better cuz we dont change the original code and will fail if temp not work
    delete next; // purpose: delete "5X" above. delete an object will recursively called until delete nullptr
    data = other.data;
    next = temp;
    return *this;
  }
};
```

```c++
// Copy and Swap Idiom: Another way of writing copy assignment operator
std::swap(a.b) <- <utility> // a gets b's value; b gets a's value
struct Node {
  void swap(Node& other) {
    std::swap(data, other.data);
    std::swap(next, other.next);
  }
};

// deep copy
Node& operator = (const Node& other) {
  Node* temp{other};
  swap{temp};
  return *this;
}
```

