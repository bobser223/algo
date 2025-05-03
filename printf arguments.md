Here's a quick reference for common **printf** format specifiers in C:

### Integer types:

- `int`: `%d` or `%i`
- `unsigned int`: `%u`
- `long int`: `%ld`
- `unsigned long int`: `%lu`
- `long long int`: `%lld`
- `unsigned long long`: `%llu`

### Floating-point types:

- `float`, `double`: `%f` (decimal), `%e` (scientific), `%g` (shortest representation)
- `double` explicitly: `%lf`
- `long double`: `%Lf`

### Characters and strings:

- `char`: `%c`
- `char *` (strings): `%s`

### Pointer addresses:

- `void *`: `%p`

### Hexadecimal and octal:

- `%x`: hex integer (lowercase letters, e.g., `7fa3`)
- `%X`: hex integer (uppercase letters, e.g., `7FA3`)
- `%o`: octal integer

### Special modifiers:

- `%zu`: `size_t` (for sizes and lengths)
- `%zd`: `ssize_t`


```c
int i = 10;
unsigned int u = 20;
float f = 3.14;
char c = 'A';
char *str = "Hello";
void *ptr = &i;
size_t len = 100;

printf("int: %d\n", i);
printf("unsigned: %u\n", u);
printf("float: %f\n", f);
printf("char: %c\n", c);
printf("string: %s\n", str);
printf("pointer: %p\n", ptr);
printf("size_t: %zu\n", len);

```