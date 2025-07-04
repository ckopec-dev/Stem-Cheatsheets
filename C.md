
# C Cheatsheets

## Basics

### How to compile

~~~
$ gcc -Wall hello.c -o hello 

// Wall shows all errors and warnings
// gcc usually isn’t installed by default

// to execute: 

$ ./hello
~~~

### Create a function

~~~
#include <stdio.h>

// Function prototype. Not strictly necessary, but helps compiler verify that 
// all calls to the function pass the correct parameters.
void SayHello();

int main()
{
	SayHello();
}

void SayHello()
{
	printf("Hello!\n");
}
~~~

### Pointers

A pointer is a variable that contains the address of another variable.

~~~
// Declare a variable
int my_variable; 

// Declare a pointer to variable
int *p_my_variable = &my_variable;
~~~

### Accept input

~~~
#include <stdio.h>

int main()
{
	int a;

	printf("Enter a number: ");

	scanf("%d", &a);

	printf("You entered: %d\n", a);
}
~~~

### Structs

~~~
#include <stdlib.h>
#include <string.h>

struct person
{
        int age;
        char *name;
};

int main(void)
{

        struct person m;

        m.age = 19;
        m.name = malloc(5 * sizeof(char));
        strcpy(m.name, "Fred");

        // pointer to the struct
        struct person *m_ptr = &m;

        // set some data using the pointer
        m_ptr -> age = 29;
        m_ptr -> name = realloc(m_ptr -> name, 6 * sizeof(char));
        strcpy(m_ptr -> name, "Fredy");
}
~~~

## Buffer overflows

Overflow occurs when you write more data to memory than you have allocated. This can result in a corrupt stack or heap.

## Structs
~~~
#include <stdio.h>
#include <string.h>

// Define a struct
struct company {
	char *name;
	int employee_count;
};

int main(void) {

	// Create a company struct
	struct company comp;

	// Assign some values
	comp.name = malloc(5 * sizeof(char));
	strcpy(comp.name, "Acme");

	// Create pointer to struct
	struct company *comp_ptr = &em;

	// Do something with the struct...
}
~~~

## Multithreading

### Basic demo

~~~
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h> 

void *entry_point(void *value) 
{
	printf("Hello from the second thread.\n");

	int *num = (int *) value;

	printf("The value of value is %d.\n", *num);

	return NULL;
}

int main(int argc, char **argv) 
{
	pthread_t thread;

	printf("Hello from the first thread.\n");

	int num = 123;

	// Args: 
	// reference to thread struct
	// pthread attributes
	// entry point function for new thread 	
	// value to send to function
	pthread_create(&thread, NULL, entry_point, &num);

	// Wait for the 2nd thread to finish.
	pthread_join(thread, NULL); 

	return EXIT_SUCCESS;
}
~~~

## Windows Programming

[GDI Reference](https://learn.microsoft.com/en-us/windows/win32/gdi/windows-gdi)

### Basic demo
~~~
#include <Windows.h>

int main()
{
        MessageBoxW(NULL, L"Hello world!", NULL, MB_OK);
}
~~~

### Basic demo compilation
~~~
# Open developer command prompt in Terminal.
C:\temp\demo.c

# Compile normally
C:\temp\cl demo.c user32.lib

# Compile with minimal overhead. When compiling minimized, c libraries are not natively available.
C:\temp\cl demo.c user32.lib /link /entry:main
~~~

### GDI demo
~~~
#include <Windows.h>

int main()
{
        // Get the device context of the entire screen.
        HDC screen = GetDC(NULL);

        // Use a red brush.
        HBRUSH redBrush = CreateSolidBrush(RGB(255, 0, 0));
        SelectObject(screen, redBrush);

        // Repeatedly draw a rectangle.
        for(;;)
        {
                Rectangle(screen, 100, 100, 500, 500);
                Sleep(20);
        }
}
~~~

### GDI demo compilation
`C:\temp\cl Gdi32.lib User32.lib gdi_demo.c`

