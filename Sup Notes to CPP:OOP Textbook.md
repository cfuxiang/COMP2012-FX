##Chapter 1
###Preprocessing, Linking & Loading
When you compile a C\+\+ source, the first thing that it will happen is preprocessing. The C\+\+ preprocessor will ensure that preprocessing directives (e.g. including other C\+\+ sources, etc) codes are performed before the actual compilation.

After the preprocessing step, the compiler will translate the C\+\+ source into an object code.

Linking phase then will link referenced functions/data that are defined elsewhere (e.g. from the standard libraries) to prevent any missing reference errors. The linker will link the object code with all these references (not defined in the C\+\+ source itself) and produce an executable program.

When you execute the executable program, the loader will transfer the object code, with the required references into the memory

##Chapter 2
###An example code
```cpp
#include <iostream>
using namespace std; //if you do not use the std namespace, everytime you will need to do something like this *std::cout*

int main() {
   cout << "Hello, world!" << endl;
   return 0;
}
```

You can't `cin` and ends with a `endl`.

You can't do this:
```cpp
cout << "Hello, world!" + an_integer << endl;
```
However, you can do this:
```cpp
cout << "Hello, world!" << an_integer << endl;
```

##Chapter 3
###An example code
```cpp
#include <iostream>
#include <string> //if you use Eclipse Oxygen with CDT and do not have this preprocessor, it will work too!
using namespace std;

class GradeBook {
int score; //data member

public:
	explicit GradeBook() { //explicit is to prevent implicit conversion. see https://stackoverflow.com/questions/121162/what-does-the-explicit-keyword-mean
    	this->score = 0;
    }

	explicit GradeBook(int score) : score(score) { //you can initialize like this too, separate by comma for multiple variable initialization
    
    }

	const void display() { //note it's a small s
		cout << "Please enter a value: " << endl;
	}
    
    int getScore() {
    	return score
    }
    
    void setScore(int score) {
    	this->score = score; //use of the pointer arrow, not dot!
    }
}; //don't forget to end with a semi-colon

int main() {
	GradeBook gradeBook;
	gradeBook.display(); //use dot only for objects
    string val = "";
    getline(cin, val);
    cout << "You have entered " << val;
	return 0;
}
```

If you want to distribute your class, remove the main methods, and save the source in a .h file. This should be the interface file (e.g. no implementation of the function prototypes.) You will distribute this file and its corresponding cpp compiled object code (thus nobody will know of its implementaion.) The implementation should be done in a separate cpp file. Only the function definitions are in the cpp file. The cpp file must include the header file and the functions in the cpp file are preceded by the `ClassName::`. When you include a self written header file, it should be using `"`instead of `<` or `>`. For your main program, you should include .h files only.

substr e.g. `name.substr(0, 25); //start at 0, length of 25`

##Chapter 4
There's no `elseif`, only `if ... else if ... else`.

e.g. `unsigned int val = 0; //only store positive integers from 0 onwards`

setting precision
```cpp
	double doubleVal = 11.23645;
	cout << setprecision(2) << fixed; //need to #include <iomanip>
	cout << doubleVal << endl; //it will return 11.24
```

casting
```cpp
	double doubleVal = 11.53645;
	int intVal = static_cast<int> (doubleVal);
	cout << intVal << endl; //it will return 11
```

##Chapter 5
e.g. `cout << "Year" << setw(10) << "Test" << endl; //setw makes the next column right justified having a field width of # char`

e.g. `pow(2,3); //2 to the power of 3, need to have #include <cmath>`

e.g. `(char grade = cin.get()) != EOF; //store 1 char into grade. EOF is either (ctr + d) or (ctr + z)`

##Chapter 6
Formal and actual parameters
```cpp
void foo(int arg); //arg is a formal parameter

int main() {
    int val = 1;
    foo(val);  //val is an actual parameter
}
```

rand() produce a pesudorandom number (the number that looks like random in sequence but it repeats itself when the program re-executed). It's best to use a different seed number each time to make it really random (you can achieve by seeding a time each time). Using the same seed number every program execution can be used for making result deterministic.
e.g. `srand(10); cout << setw(3) << (rand() %  6); //give a seed number, and produce random integer between 0 and 5. Need to #include <cstdlib> for rand()`

e.g. `srand(10); cout << setw(3) << (1 + rand() %  6); //give a seed number, and produce random integer between 1 and 6. Need to #include <cstdlib> for rand()`

e.g. `srand(static_cast<int>time(0)); //this requires #include <ctime>. time(0) returns the current time in seconds. It is of time_t type, so need to cast it to int type.`

enum type is a user-defined type. The identifier starts from 0. e.g. `enum Status {CONTINUE, WON, LOST};`

e.g. `extern int global_x; //it means that global_x is being defined someother places but you can still use this global_x variable in this source.`

If your function is inline e.g. `inline void foo(){...}`, it means that the compiler will try to copy the function body/content to the source whenever there is a function call. This is to reduce the total number of function calls in the program. The resulting program may be slightly faster.

If you want to pass by reference, your function needs to have ampersand for the types.
e.g. 
```cpp
int squareByValue(int); //pass by value
int squareByReference(int &) //pass by reference
```

Alias
e.g.
```cpp
int count = 1;
int &acount = count;
acount++; //acount = 1 & count also = 1. Both acount and count are the same.
```

Unary Scope Resolution Operator `::` - If the variable names of the global and local variables are the same, you can use `::` to reference the global variable within the local variable scope.
e.g.
```cpp
int num = 7;

int main() {
	double num = 6.0;
    cout << ::num << endl; //prints 7
	return 0;
}
```

Function Template (Generics) - For different types of the same function name with the same number but different types (the types must be consistent) of parameters, this is another way of writing overloaded functions.
```cpp
template<typename T> //or template<class T>. Over here, the generic type T is defined. In the later part of the code, T is being used.
T max(T value1, T value2, T value3) {
	T max = value1;
    if(value2 > max)
    	max = value2;
    if(value3 > max)
    	max = value3;
    return max;
}
```

Factorial Recursion
```cpp
function long factorial(long num) {
	if(num == 1)
    	return 1;
    else 
    	num * factorial(num-1);
}
```

Fibonacci Recursion
```cpp
function long fibo(long num) {
	if(num == 0 || num == 1)
    	return num;
    else 
    	return fibo(num-1) + fibo(num-2);
}
```

##Chapter 7
To define an array, use `array<type, srraySize> arrayName;`. Needs to `#include <array>`
e.g. `array<int, 12> c;` or `array<int, 12> c = {1, 2, 3};`

e.g. 
```cpp
for(size_t i; i<c.size(); i++) { //it is size() and the type is size_t
	c[i] = 0;
}
```

`size_t` in C\+\+ is an unsigned integer type.

Range-Based (Enhanced) For Loop
e.g.
```cpp
array<int, 3> items = {1, 2, 3};
for(int item: items) {
	cout << item;
}
```

Constructor can also be passed by reference. e.g. using `&` in the constructor definition.

Sorting and Searching
e.g.
```cpp
#include <iostream>
#include <algorithm> //contain sort and binary_search
#include <string>
#include <array>
using namespace std;

int main() {
	array<string, 3> colors = {"red", "blue", "green"};

	sort(colors.begin(), colors.end()); //sorted in ascending order
    for(string color: colors) {
    	cout << color << endl;
    }
    
    bool found = binary_search(colors.begin(), colors.end(), "red");
    cout << "Found red? " << found;
    
    return 0;
}
```

I find that C\+\+11 new way of creating multidimensional array is overly complicated. Therefore, I will present the oldschool way of creating and initializaing a multidimensional array in C\+\+.
e.g.
```cpp
int a[3][4] = {  
   {0, 1, 2, 3} ,   /*  initializers for row indexed by 0 */
   {4, 5, 6, 7} ,   /*  initializers for row indexed by 1 */
   {8, 9, 10, 11}   /*  initializers for row indexed by 2 */
};
```

Vectors
```cpp
#include <vector> //need to use this
using namespace std;
...
vector<int> intVec(5); //5 elements vector
intVec.size(); //get size of vector
intVec[4] = 100; 
intVec.at(4); //get the value of element #5
try {
	intVec.at(5);
} catch (out_of_range &ex) {
	cerr << "out of range! " << ex.what();
}
intVec.push_back(101); //add 101 to element #6

vector<int> anotherVec(intVec); //use of copy constructor. Initialize a new object with another object.
```

##Chapter 8
Pointers
```cpp
int y=5;
int *yPtr = nullptr; //notice the use of nullptr

yPtr = &y; //notice the difference between this and alias
cout << *yPtr << endl;  //this is the dereferencing operator
*yPtr = 9; //assign 9 to y
```

Pass by reference with Pointers
```cpp
void cube(int *nPtr) { //pass by reference with pointers
	*nPtr = *nPtr * nPtr * nPtr;
}
int main() {
	int num = 5;
    cube(&num); //pass in the address. For the normal pass by reference, you do not need to have the &
}
```

Built-in array
```cpp
int c[12]; //type arrayName[arraySize];
int n[3] = {1, 2, 3}; //this is the same as int n[] = {1, 2, 3};
...
int sum(const int values[], size_t size); //built-in array do not know its own size
...
sort(begin(n), end(n)); //sorting a built-in array n
```

Nonconstant Pointer to Nonconstant Data - the pointer declaration does not have `const``

Nonconstant Pointer to Constant Data - data to which it points cannot be modified
e.g.
```cpp
void f(const int *xPtr) {
	*xPtr = 100; //error. You can't modify
}
int main() {
	int y = 0;
    f(&y); 
}
```

Constant Pointer to Nonconstant Data
e.g.
```cpp
int main() {
	int x, y;
    int * const ptr = &x; //const pointer must be initialised
    
    *ptr = 7; //*ptr is actually referring to x which is not const. So it is ok.
    ptr = &y; //ptr is const! Can't assign the address of y to it.
}
```

Constant Pointer to Constant Data
e.g. 
```cpp
int main() {
	int x = 5;
    int * const ptr = &x;
    
    *ptr = 7; //error. Because x has been defined earlier. Notice the different between this and above.
}
```

To get the number of items of an array e.g. `sizeof num/sizeof(num[0]; //sizeof returns the bytes of the array`

Pointer-Based String
e.g.
```cpp
char color[] = "blue";
const char *colorPtr = "blue";
char color2[] = {'b', 'l', 'u', 'e'}; //all these 3 are the same
```

##Chapter 9
Prevent multiple inclusions of header files
e.g.
```cpp
//this is Time.h (Notic that below is TIME_H. Actually it doesn't matter about the naming.)
#ifndef TIME_H
#define TIME_H
...//your class definition here e.g. class Time {}; //the class Time can be an interface
#endif //last line
```

Throwing an exception e.g. `throw invalid_argument("invalid!");`

You can set the function prototypes in the header files and implement the functions in another source that includes the header files.

Delegating constructor - the constructor that calls another constructor for initialization

Constructors and Destructors for Local Objects
* constructor is called when the execution reached the point where the object is defined
* destructor is called when the execution leaves the object's scope
* if program terminates with `exit` or `abort`, destructor is not called

Constructors and Destructors for static Local Objects
* constructor called only once when execution first reached the point where the object is defined
* destructor called when `main` terminates or `exit` is called
* global and static objects are destroyed in the reverse order of their creation

Return a reference or a pointer to a private data member
e.g.
```cpp
class Time {
	int hour; //this is a private data member
    
    unsigned int &badReturn() {
    	hour = 10;
    	return hour; //it is returning a private data member by a reference
    }
};

int main() {
	Time t;
    int &hour = t.badReturn();
    hour = 100; //it changes the private data member of the class Time!
    
    t.badReturn() = 1000; //a reference can be set as a lvalue. It also modifies the private data member of the class Time!
}
```

Default membership assignment
e.g.
```cpp
Date date1(1,1,1); //date1's constructor is called to initialize
Date date2 = date1; //membership assignment. Each member of date1 is assigned individually to each member of date2. If there is a copy constructor defined, it will be called.

//ref to copy constructor:
//1) https://msdn.microsoft.com/en-us/library/x0c54csc(v=vs.80).aspx
//2) https://www.tutorialspoint.com/cplusplus/cpp_copy_constructor.htm
```

A constant object calling a non-constant function will invoke a compilation error. A constant object can call a constant function, and a non-constant object can call a constant/non-constant function.

Composition - a data member is an object of another class

friend functions and friend classes
* a friend function of a class is a non member function that can access public and non-public class members
* to declare a function as a friend of a class, precede function prototype with `friend``
* if all functions of a class are friends, you can do this `friend class Classtwo`; in the other class
* the other class still needs to declare that the class containing friend function(s) is a friend
* friends are not symmetric or transitive

e.g.
```cpp
class Box {
	double width;
    public:
    	friend void printWidth(Box box);
        void setWidth(double wid);
};

// Member function definition
void Box::setWidth( double wid ) {
   width = wid;
}

// Note: printWidth() is not a member function of any class.
void printWidth( Box box ) {
   /* Because printWidth() is a friend of Box, it can
   directly access any member of this class */
   cout << "Width of box : " << box.width <<endl;
}
 
// Main function for the program
int main() {
   Box box;
 
   // set box width without member function
   box.setWidth(10.0);
   
   // Use friend function to print the wdith.
   printWidth( box );
 
   return 0;
}
```

`this->x; and (*this).x; are the same`

Using this Pointer to enable cascading function call
e.g.
```cpp
class Time {
	unsigned int hour;
public:
	Time &setHour(int hour){
    	this->hour = hour;
        return *this;
    }
    unisgned int getHour(){
    	return hour;
    }
};

int main() {
	Time t;
    cout << t.setHour(10).getHour();
}
```
