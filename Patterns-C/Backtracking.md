
[22. Generate Parentheses](http://leetcode.com/problems/generate-parentheses/)

### difference between char** res = malloc(10000 * sizeof(char*)) and char* res = malloc(10000 * sizeof(char))

char* res = malloc(10000 * sizeof(char)) allocates memory for a single string of 10000 characters. res is a pointer to the first character of this string.

char** res = malloc(10000 * sizeof(char*)) on the other hand, allocates memory for an array of 10000 pointers to strings. res is a pointer to the first element of this array of pointers. Each of these pointers can then be used to point to a different string.

The size of 10000 * sizeof(char*) depends on the size of a pointer on your system. On a 32-bit system, a pointer is typically 4 bytes, so 10000 * sizeof(char*) would be 40000 bytes. On a 64-bit system, a pointer is typically 8 bytes, so 10000 * sizeof(char*) would be 80000 bytes.

Remember that sizeof(char*) gives the size of a pointer to a char, not the size of a char itself. The size of a char is always 1 byte, but the size of a pointer can vary depending on the system.

```c
int* res = malloc(10 * sizeof(int)); // Dynamically allocates memory for an array of 10 integers

char** res = malloc(10 * sizeof(char*)); // Dynamically allocates memory for an array of 10 strings
for(int i = 0; i < 10; i++) {
    res[i] = malloc(50 * sizeof(char)); // Allocate memory for each string
}

free(res); // For the integer array

for(int i = 0; i < 10; i++) {
    free(res[i]); // Free each string
}
free(res); // Then free the array of strings
```

### why path[*pathSize] = '\0'?

The line path[*pathSize] = '\0' is used to add a null character at the end of the current string stored in path.

In C, strings are arrays of characters, and the end of the string is marked by a special character: the null character, which is represented as '\0'. This is a convention in C programming to denote the end of a string.

When you're building a string character by character, like in a backtracking algorithm, you need to manually add this null character at the end of your string once you're done building it. This is what path[*pathSize] = '\0' does: it adds the null character at the position *pathSize in the path array, effectively marking the end of the string.

This is important because many functions in the C standard library (like printf, strcpy, etc.) expect strings to be null-terminated. They keep reading characters until they encounter a '\0', so if you don't add one, they could read past the end of your actual data, leading to undefined behavior.

### why char*** res in backtrack func? 

In the backtrack function, res is used to store the generated combinations of parentheses. Each combination is a string, and there can be multiple combinations, so an array of strings (i.e., a char**) is needed. Since the array needs to be modified from within the function, a pointer to the array (i.e., a char***) is used.

Solution:

```c
void backtrack(int left, int right, char* path, char*** res, int* returnSize, int* pathSize) {
    if (left == 0 && right == 0) {
        path[*pathSize] = '\0';
        (*res)[*returnSize] = malloc((*pathSize + 1) * sizeof(char));
        strcpy((*res)[*returnSize], path);
        (*returnSize)++;
        return;
    }
    if (left > 0) {
        path[*pathSize] = '(';
        (*pathSize)++;
        backtrack(left - 1, right, path, res, returnSize, pathSize);
        (*pathSize)--;
    }
    if (right > left) {
        path[*pathSize] = ')';
        (*pathSize)++;
        backtrack(left, right - 1, path, res, returnSize, pathSize);
        (*pathSize)--;
    }
}

char** generateParenthesis(int n, int* returnSize) {
    char** res = malloc(10000 * sizeof(char*));
    char* path = malloc((2 * n + 1) * sizeof(char)); // Allocate 2n + 1 bytes for path
    int pathSize = 0;
    *returnSize = 0;
    backtrack(n, n, path, &res, returnSize, &pathSize);
    free(path);
    return res;
}
```