
[22. Generate Parentheses](http://leetcode.com/problems/generate-parentheses/)

char* res = malloc(10000 * sizeof(char)) allocates memory for a single string of 10000 characters. res is a pointer to the first character of this string.

char** res = malloc(10000 * sizeof(char*)) on the other hand, allocates memory for an array of 10000 pointers to strings. res is a pointer to the first element of this array of pointers. Each of these pointers can then be used to point to a different string.

The size of 10000 * sizeof(char*) depends on the size of a pointer on your system. On a 32-bit system, a pointer is typically 4 bytes, so 10000 * sizeof(char*) would be 40000 bytes. On a 64-bit system, a pointer is typically 8 bytes, so 10000 * sizeof(char*) would be 80000 bytes.

Remember that sizeof(char*) gives the size of a pointer to a char, not the size of a char itself. The size of a char is always 1 byte, but the size of a pointer can vary depending on the system.

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