
# C Language Exercice Solutions

**Exercice 1**
*Simple C Language "program"*
```
// Simple C Language "program"

#include <stdio.h>

int main() {
  printf("Hello World!");
  return(0):
}
```

**Exercice 2**
*Change a string to Uppercase*

```
// String manipulation
// string uppercase
//
#include <stdio.h>
#include <ctype.h>

int uppercase(char *string);

int main () {
  char str[20];

  printf("string :");
  scanf("%s",str);

  // 0=only characters in string; 1=numbers are present in string
  printf("return value: %d\n",uppercase(str));

  printf("new string: |%s|\n",str);

}
 
int uppercase(char *string) {

  int isnumber=0;

  // search for end of string and call uppercase for each character
  while (*string!='\0') {
      *string=toupper(*string);
      // if a digit is find, return 1 else 0
      if (isdigit(*string++)!=0)
        isnumber=1;
  }
  
  return(isnumber);
}
  
```

**Exercice 3**
*Substitute a string*
```
// String manipulation
// Substitute string2 with string3 in string1
//
#include <stdio.h>
#include <stdlib.h>
#include <string.h>  // strings library

int subst(char *string1, char *string2, char *string3);

int main () {
  char str1[20], str2[20], str3[20];

  printf("string1 :");
  scanf("%s",str1);
  printf("string2 :");
  scanf("%s",str2);
  printf("string3 :");
  scanf("%s",str3);

  // Return value: 0 not found or not possible ; 1 found and substituted
  printf("return value : %d\n",subst(str1,str2,str3));

  printf("new string1: |%s|\n",str1);

}
 
int subst(char *string1, char *string2, char *string3) {

  char *whereisstring;
  int i=0;
 
  // check if string2 is the same size of string3 
  if (strlen(string2)!=strlen(string3)) {
    printf("Error: string2 and string3 of different sizes\n");
    return(0);
  }
  // whereisstring to be pointed to the place where string2 is at the beggining
  if ((whereisstring=strstr(string1, string2))==NULL) {
    printf("Error: string2 not found in string1!!!\n");
    return(0);
  } else {
    // change string1 (remember that whereisstring is pointing to it) with string3
    while (*string3!='\0')
      *whereisstring++=*string3++;
  }
  return(1);
}  

```

**Exercice 4**
*Read a file with numbers, sort and save to another file*
```
//
// Sort numbers in file, output to another file
//
// Read "n" numbers from file in argv[1] and save them ordered in file argv[2]
//
// Parameters:  argv[1] input filename
//              argv[2] output filename
//
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{

  int i,j,num[100];
  int x,y,z,temp;
  char s[20];
  FILE *f;

  // Test argument number - two arguments are expected
  if (argc<3)
  {
    printf("Invalid argument number!\n");
    exit(1);
  }

  // Opening and testing if filename in argv[1] exists. If not exit the program with errorlevel 1
  if ((f=fopen(argv[1],"r"))==NULL){
    printf("Problem reading input file (%s)!!!\n",argv[1]);
    exit(1);
  }
  
  // Read file for array, counts line numbers
  i=0;
  while (fscanf(f,"%d\n",&num[i])!=EOF) 
    i++;
  
  i--;
  printf("\nInput file read sucessufully!\n\n");

  printf("Input file :");
  for (j=0;j<=i;j++)
    printf("%d ",num[j]);
  printf("\n");

  // Ordering vector with a rather poor algoritm (too much swaps). You can improve the program with other algorithm but
  // the main purpose of this exercice is to use files!
  for (z=0;z<=i;z++)
    for (y=i;y>=z;y--) 
      if (num[y]>num[y+1]){
         temp=num[y];
         num[y]=num[y+1];
         num[y+1]=temp;
      }

  // Output of ordered array
  printf("Ordered array: :");
  for (j=0;j<=i;j++)
    printf("%d ",num[j]);
  printf("\n");

  // Saving ordered array in filename argv[2]. You can improve the program by doing the output and saving in one loop only.
  if ((f=fopen(argv[2],"w"))==NULL){
    printf("Problem creating output file (%s)!!!\n",argv[2]);
    exit(1);
  } else {
    for (j=0;j<=i;j++)
      fprintf(f,"%d\n",num[j]);
    fclose(f);
    printf("\nFile ordered and saved sucessufully!\n");
    exit(0);
  }
  exit(2);

}
 

```

**Exercice 5**
*Search a string in a file*
```
//
// Search a string in a file and outputs the line number and the full line contents. Outputs also the total number of occurrences.
//
// Parameters:  argv[1] input filename
//              argv[2] string to search
//
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{

  int linenum,occurrences;
  char s[250];
  FILE *f;

  // Test argument number - two arguments are expected
  if (argc<3)
  {
    printf("Invalid argument number!\n");
    exit(1);
  }

  // Opening and testing if filename in argv[1] exists. If not exit the program with errorlevel 1
  if ((f=fopen(argv[1],"r"))==NULL){
    printf("Problem reading input file (%s)!!!\n",argv[1]);
    exit(1);
  }
  
  // Search, output and counts the line and occurrences number
  linenum=0;occurrences=0;
  while (fgets(s,250,f)!=NULL) { 
    linenum++;
    if (strstr(s,argv[2])!=NULL) {
       printf("%d: %s",linenum,s);
       occurrences++;
    }
  }
  linenum--;
  fclose(f);

  printf("Found %d occurrences of string \"%s\"\n",occurrences,argv[2]);

}
 

```

---------

# Winsock Exercice Solutions

**Important: You need to add the lib `ws2_32.lib` to the linker input (Open project's Property; Choose the Configuration Properties > Linker > Input property page; add `ws2_32.lib` to the Additional Dependencies property)**

First run the server application in a command line `server`. It will listen on TCP port 27015 for a client to connect. The server receives data from the client and sends it back to the client. 

**Exercice 6**
*Winsock Server*
```
#undef UNICODE

#define WIN32_LEAN_AND_MEAN

#include <windows.h>
#include <winsock2.h>
#include <ws2tcpip.h>
#include <stdlib.h>
#include <stdio.h>

// Need to link with Ws2_32.lib
#pragma comment (lib, "Ws2_32.lib")
// #pragma comment (lib, "Mswsock.lib")

#define DEFAULT_BUFLEN 512
#define DEFAULT_PORT "27015"

int __cdecl main(void) 
{
    WSADATA wsaData;
    int iResult;

    SOCKET ListenSocket = INVALID_SOCKET;
    SOCKET ClientSocket = INVALID_SOCKET;

    struct addrinfo *result = NULL;
    struct addrinfo hints;

    int iSendResult;
    char recvbuf[DEFAULT_BUFLEN];
    int recvbuflen = DEFAULT_BUFLEN;
    
    // Initialize Winsock
    iResult = WSAStartup(MAKEWORD(2,2), &wsaData);
    if (iResult != 0) {
        printf("WSAStartup failed with error: %d\n", iResult);
        return 1;
    }

    ZeroMemory(&hints, sizeof(hints));
    hints.ai_family = AF_INET;
    hints.ai_socktype = SOCK_STREAM;
    hints.ai_protocol = IPPROTO_TCP;
    hints.ai_flags = AI_PASSIVE;

    // Resolve the server address and port
    iResult = getaddrinfo(NULL, DEFAULT_PORT, &hints, &result);
    if ( iResult != 0 ) {
        printf("getaddrinfo failed with error: %d\n", iResult);
        WSACleanup();
        return 1;
    }

    // Create a SOCKET for the server to listen for client connections.
    ListenSocket = socket(result->ai_family, result->ai_socktype, result->ai_protocol);
    if (ListenSocket == INVALID_SOCKET) {
        printf("socket failed with error: %ld\n", WSAGetLastError());
        freeaddrinfo(result);
        WSACleanup();
        return 1;
    }

    // Setup the TCP listening socket
    iResult = bind( ListenSocket, result->ai_addr, (int)result->ai_addrlen);
    if (iResult == SOCKET_ERROR) {
        printf("bind failed with error: %d\n", WSAGetLastError());
        freeaddrinfo(result);
        closesocket(ListenSocket);
        WSACleanup();
        return 1;
    }

    freeaddrinfo(result);

    iResult = listen(ListenSocket, SOMAXCONN);
    if (iResult == SOCKET_ERROR) {
        printf("listen failed with error: %d\n", WSAGetLastError());
        closesocket(ListenSocket);
        WSACleanup();
        return 1;
    }

    // Accept a client socket
    ClientSocket = accept(ListenSocket, NULL, NULL);
    if (ClientSocket == INVALID_SOCKET) {
        printf("accept failed with error: %d\n", WSAGetLastError());
        closesocket(ListenSocket);
        WSACleanup();
        return 1;
    }

    // No longer need server socket
    closesocket(ListenSocket);

    // Receive until the peer shuts down the connection
    do {

        iResult = recv(ClientSocket, recvbuf, recvbuflen, 0);
        if (iResult > 0) {
            printf("Bytes received: %d\n", iResult);

        // Echo the buffer back to the sender
            iSendResult = send( ClientSocket, recvbuf, iResult, 0 );
            if (iSendResult == SOCKET_ERROR) {
                printf("send failed with error: %d\n", WSAGetLastError());
                closesocket(ClientSocket);
                WSACleanup();
                return 1;
            }
            printf("Bytes sent: %d\n", iSendResult);
        }
        else if (iResult == 0)
            printf("Connection closing...\n");
        else  {
            printf("recv failed with error: %d\n", WSAGetLastError());
            closesocket(ClientSocket);
            WSACleanup();
            return 1;
        }

    } while (iResult > 0);

    // shutdown the connection since we're done
    iResult = shutdown(ClientSocket, SD_SEND);
    if (iResult == SOCKET_ERROR) {
        printf("shutdown failed with error: %d\n", WSAGetLastError());
        closesocket(ClientSocket);
        WSACleanup();
        return 1;
    }

    // cleanup
    closesocket(ClientSocket);
    WSACleanup();

    return 0;
}

```
*Source: https://learn.microsoft.com/en-us/windows/win32/winsock/complete-server-code*

**Troubleshooting the server:** If you get an error opening the program 1) check in *task manager* if the program is still running from a previous run/session; 2) check with `netstat -a -n` (in a *command line*) if the port is still open (it can happen when the program didn't end normally).

**Exercice 7**
*Winsock Client*

The client application requires a name or ip address to connect to (where the server was run). Try to execute it on the same computer running `client localhost`. Then, try to execute on a different computer by running `client x.x.x.x` where x.x.x.x is the ip address of the computer where the server is running.

```
#define WIN32_LEAN_AND_MEAN

#include <windows.h>
#include <winsock2.h>
#include <ws2tcpip.h>
#include <stdlib.h>
#include <stdio.h>


// Need to link with Ws2_32.lib, Mswsock.lib, and Advapi32.lib
#pragma comment (lib, "Ws2_32.lib")
#pragma comment (lib, "Mswsock.lib")
#pragma comment (lib, "AdvApi32.lib")


#define DEFAULT_BUFLEN 512
#define DEFAULT_PORT "27015"

int __cdecl main(int argc, char **argv) 
{
    WSADATA wsaData;
    SOCKET ConnectSocket = INVALID_SOCKET;
    struct addrinfo *result = NULL,
                    *ptr = NULL,
                    hints;
    const char *sendbuf = "this is a test";
    char recvbuf[DEFAULT_BUFLEN];
    int iResult;
    int recvbuflen = DEFAULT_BUFLEN;
    
    // Validate the parameters
    if (argc != 2) {
        printf("usage: %s server-name\n", argv[0]);
        return 1;
    }

    // Initialize Winsock
    iResult = WSAStartup(MAKEWORD(2,2), &wsaData);
    if (iResult != 0) {
        printf("WSAStartup failed with error: %d\n", iResult);
        return 1;
    }

    ZeroMemory( &hints, sizeof(hints) );
    hints.ai_family = AF_UNSPEC;
    hints.ai_socktype = SOCK_STREAM;
    hints.ai_protocol = IPPROTO_TCP;

    // Resolve the server address and port
    iResult = getaddrinfo(argv[1], DEFAULT_PORT, &hints, &result);
    if ( iResult != 0 ) {
        printf("getaddrinfo failed with error: %d\n", iResult);
        WSACleanup();
        return 1;
    }

    // Attempt to connect to an address until one succeeds
    for(ptr=result; ptr != NULL ;ptr=ptr->ai_next) {

        // Create a SOCKET for connecting to server
        ConnectSocket = socket(ptr->ai_family, ptr->ai_socktype, 
            ptr->ai_protocol);
        if (ConnectSocket == INVALID_SOCKET) {
            printf("socket failed with error: %ld\n", WSAGetLastError());
            WSACleanup();
            return 1;
        }

        // Connect to server.
        iResult = connect( ConnectSocket, ptr->ai_addr, (int)ptr->ai_addrlen);
        if (iResult == SOCKET_ERROR) {
            closesocket(ConnectSocket);
            ConnectSocket = INVALID_SOCKET;
            continue;
        }
        break;
    }

    freeaddrinfo(result);

    if (ConnectSocket == INVALID_SOCKET) {
        printf("Unable to connect to server!\n");
        WSACleanup();
        return 1;
    }

    // Send an initial buffer
    iResult = send( ConnectSocket, sendbuf, (int)strlen(sendbuf), 0 );
    if (iResult == SOCKET_ERROR) {
        printf("send failed with error: %d\n", WSAGetLastError());
        closesocket(ConnectSocket);
        WSACleanup();
        return 1;
    }

    printf("Bytes Sent: %ld\n", iResult);

    // shutdown the connection since no more data will be sent
    iResult = shutdown(ConnectSocket, SD_SEND);
    if (iResult == SOCKET_ERROR) {
        printf("shutdown failed with error: %d\n", WSAGetLastError());
        closesocket(ConnectSocket);
        WSACleanup();
        return 1;
    }

    // Receive until the peer closes the connection
    do {

        iResult = recv(ConnectSocket, recvbuf, recvbuflen, 0);
        if ( iResult > 0 )
            printf("Bytes received: %d\n", iResult);
        else if ( iResult == 0 )
            printf("Connection closed\n");
        else
            printf("recv failed with error: %d\n", WSAGetLastError());

    } while( iResult > 0 );

    // cleanup
    closesocket(ConnectSocket);
    WSACleanup();

    return 0;
}

```
*Source: https://learn.microsoft.com/en-us/windows/win32/winsock/complete-client-code*

**Troubleshooting the client:** If you get an error running the program, check too see if the server component is running (or else it wouldn't have any socket to connect to in the *coonnect* call).
