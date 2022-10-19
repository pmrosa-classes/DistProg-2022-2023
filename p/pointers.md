
# C Language Exercice Solutions
## Pointers

**Exercice 3**
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
