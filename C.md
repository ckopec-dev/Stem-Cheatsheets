
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
