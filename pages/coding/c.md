### C split string

```c

// A C/C++ program for splitting a string 

#include <stdio.h> 
#include <stdlib.h>
#include <string.h> // strtok

char* substr(const char* pstr, int pos, int count) {
	if (!pstr) return NULL;

	int len = strlen(pstr);
	if (count < 0 || pos + count > len) {
		count = len - pos;
	}

	if (count <= 0) {
		return NULL;
	}

	char* result = (char*)malloc(sizeof(char) * (count + 1));
	memcpy(result, pstr + pos, count);
	result[count] = '\0';
	return result;
}

char** split(const char* pstr, char split, int* length) {
	*length = 0;

	if (!pstr) return NULL;
	
	int len = strlen(pstr);
	char* caches[len];
	int p = 0, q = 0, n = 0;
	for (int i = 0; i < len; ++i) {
		if (pstr[i] != split) {
			++q;
		}else {
			if (q > p) {
				caches[n++] = substr(pstr, p, q - p);
			}
			p = i + 1;
			q = p;
		}
	}
	if (q > p) {
		caches[n++] = substr(pstr, p, q - p);
	}
	
	if (0 == n) return NULL;

	*length = n;
	char** results = (char**)malloc(sizeof(char*) * n);

	for (int i = 0; i < n; ++i) {
		results[i] = caches[i];
	}

	return results;
}

// #define TEST_STR "---12-34-56-1--"
#define TEST_STR "1---12-34-56-1--1"
  
int main() 
{ 
	///////////using strtok
	char str1[] = TEST_STR;
	char* token = strtok(str1, "-"); 
	while (token != NULL) { 
		printf("%s\n", token); 
		token = strtok(NULL, "-"); 
	}
	/////////////using split
	char str2[] = TEST_STR;
	int len = 0;
	char** results = split(str2, '-', &len);
	printf("len=%d\n", len);
	for (int i = 0; i < len; ++i) {
		printf("%d:%s\n", i, results[i]);
		free(results[i]); //free by yourself!
	}
	
	return 0; 
} 

```