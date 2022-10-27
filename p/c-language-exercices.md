
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



