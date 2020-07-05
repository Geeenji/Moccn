Given a stack which can keep M numbers at most. Push N numbers in the order of 1, 2, 3, ..., N and pop randomly. 
You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack. 
For example, if M is 5 and N is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.

Input Specification:
Each input file contains one test case. For each case, the first line contains 3 numbers (all no more than 1000): 
M (the maximum capacity of the stack), N (the length of push sequence), and K (the number of pop sequences to be checked). 
Then K lines follow, each contains a pop sequence of N numbers. All the numbers in a line are separated by a space.

Output Specification:
For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.

Sample Input:
```
5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2
```
Sample Output:
```
YES
NO
NO
YES
NO
```
解题思路：设置两个指针分别指向数字池的首端和栈顶，根据先入后出原则，用数组模拟栈，栈顶应该是数组的最后一个元素。
如果栈顶元素小于当前数字池的那个元素，则向栈里压入一个新数字，
如果栈顶元素等于当前数字池的那个数字，则出栈一个数组，同时数字池指针向后移一位，表示下一个检测的元素，
如果栈顶元素大于数字池指针指向的元素，则说明数字池的元素被放在了栈顶下面，无法弹出，则为NO；
注意栈满的情况，在入栈之前应该先判断栈是否满了。
```c++
#include <stdio.h>
#include <string.h>
int main() {
    int M, N, K;
    scanf("%d %d %d", &M, &N, &K);
    int stack[M], pool[N];
    while(K--) {
        //初始化要检测的数字池
        int flag = 0;
        for (int i = 0; i < N; ++i) {
            scanf("%d", &pool[i]);
        }
        int pPool = 0, pStack = -1, num = 1;
        while (pPool < N) {
            if (pStack == -1) {
                // 栈空
                stack[++pStack] = num++;
            } else {
                // 栈不为空，检查栈顶元素和数字池顶元素是否匹配
                if (stack[pStack] == pool[pPool]) {
                    // 匹配
                    pStack--;
                    pPool++;
                } else if (stack[pStack] < pool[pPool]) {
                    // 栈顶元素比较小，则先判断后入栈
                    if ((++pStack) == M) {
                        flag = 1;
                        break;
                    }
                    stack[pStack] = num++;
                } else {
                    flag = 1;
                    break;
                }
            }
        }
        if (flag) printf("NO\n");
        else printf("YES\n");
    }
    return 0;
}
/*
5 7 1
1 7 6 5 4 3 2

5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2
 */

```
