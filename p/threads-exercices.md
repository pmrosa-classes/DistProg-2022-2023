# Threads Exercices

## Simple multithread C Language Program

Creates several threads and prints their ID and number.
Fully commented on the source code.

```
// Simple multithread C Language Program
// Creates NUMTHREADS threads and waits for enter
// 
// You can check Windows Process created in Resource Monitor

#include <windows.h>
#include <stdlib.h>
#include <stdio.h>
#include <process.h>

#define NUMTHREADS 8
int global = 0;

// Thread function
void ThreadCode(void* param);

// First thread
int main()
{

    int i;
    int val = 0;
    HANDLE handle;

    printf("\tThreads Simple Demo\n");

    for (i = 1; i <= NUMTHREADS; i++)
    {
        val = i;

        printf("ThreadID: %d\n", GetCurrentThreadId());
        // Create thread - first argument is the function to be called, last args
        handle = (HANDLE)_beginthread(ThreadCode, 0, &val);

        // Lets use a global var to check if all threads can use it
        global++;
        Sleep(100);
        // If you need to wait for the thread to end use WaitForSingleObject
        //WaitForSingleObject(handle, INFINITE);

    }
    printf("Ended. Hit any key!\n");
    getchar();
    return 0;
}


void ThreadCode(void* param)
{
    int local = 0;

    int h = *((int*)param);

    // What thread is running?
    printf("Thread %d of %d is Running!\n", h, NUMTHREADS);
    // GetCurrentThreadId() returns the thread identifier that uniquely
    // identifies the thread throughout the system.
    printf("  ThreadID: %d\n", GetCurrentThreadId());
    // Printing local and global vars. First will be always 0,
    // second will increase in each thread
    printf("  local=%d  global=%d\n", local, global);

    _endthread();

}
```
