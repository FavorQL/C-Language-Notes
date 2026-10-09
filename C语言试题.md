# C语言基础复习

## 1. 指针数组、数组指针、函数指针、指针函数

### 指针数组
```c
int *arr[5];
```
arr 是一个数组，包含 5 个元素，每个元素都是指向 int 的指针。

### 数组指针
```c
int (*p)[5];
```
p 是一个指针，指向一个包含 5 个 int 的数组。

### 函数指针
```c
int add(int a, int b) { return a + b; }

int (*p)(int, int) = add;
int result = p(2, 3); // 5
```
函数指针指向函数。

### 指针函数
```c
int *get_value(int *p) {
    return p;
}
```
指针函数是返回指针的函数。

| 名称 | 示例 | 含义 |
|---|---|---|
| 指针数组 | `int *arr[5];` | 数组，元素是指针 |
| 数组指针 | `int (*p)[5];` | 指针，指向数组 |
| 函数指针 | `int (*p)(int);` | 指针，指向函数 |
| 指针函数 | `int *fun(void);` | 函数，返回指针 |

## 2. do...while 和 while 的区别

`while` 先判断条件，条件为真才执行，循环体可能执行 0 次。

```c
int i = 5;
while (i < 3) {
    printf("%d\\n", i);
    i++;
}
```

`do...while` 先执行循环体，再判断条件，因此至少执行一次。结尾必须有分号。

```c
int i = 5;
do {
    printf("%d\\n", i);
    i++;
} while (i < 3);
```

## 3. for 循环的执行顺序

```c
for (初始化; 条件; 更新) {
    // 循环体
}
```

执行顺序：
1. 初始化，只执行一次。
2. 判断条件。
3. 条件为真，执行循环体；为假，结束循环。
4. 执行更新表达式。
5. 返回第 2 步。

示例：
```c
for (int i = 0; i < 3; i++) {
    printf("%d ", i);
}
// 输出：0 1 2
```

## 4. 为什么 switch 要加 break

如果某个 `case` 末尾没有 `break`、`return` 等跳转语句，程序可能继续执行后面的分支，这称为贯穿（fall-through）。

```c
switch (n) {
case 1:
    printf("one\\n");
    break;
case 2:
    printf("two\\n");
    break;
default:
    printf("other\\n");
    break;
}
```

通常加 `break` 是为了防止意外执行后续分支。

## 5. 字符串指针代码找错

题目：
```c
char* s = “hello”；
*s=‘H’
```

问题：
1. 使用了中文全角引号和分号，C 语言应使用英文半角符号。
2. 字符串字面量不能被修改，执行 `*s = 'H';` 会导致未定义行为。

正确写法：
```c
char s[] = "hello";
s[0] = 'H';
printf("%s\\n", s); // Hello
```

也可以写 `const char *s = "hello";`，表示不通过 s 修改字符串内容。
