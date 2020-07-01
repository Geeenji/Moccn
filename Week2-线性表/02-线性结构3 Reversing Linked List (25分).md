### 02-线性结构3 Reversing Linked List (25分)
**Given a constant K and a singly linked list L, you are supposed to reverse the links of *every K* elements on L.
For example, given L being 1→2→3→4→5→6, if K=3, then you must output 3→2→1→6→5→4; if K=4, you must output 4→3→2→1→5→6.**
```
Input Specification:
Each input file contains one test case. For each case, the first line contains the address of the first node, 
a positive N (≤100000) which is the total number of nodes, 
and a positive K (≤N) which is the length of the sublist to be reversed. 
The address of a node is a 5-digit nonnegative integer, and NULL is represented by -1.
```
**Then N lines follow, each describes a node in the format:**

Address Data Next
where Address is the position of the node, Data is an integer, and Next is the position of the next node.
```
Output Specification:
For each case, output the resulting ordered linked list. 
Each node occupies a line, and is printed in the same format as in the input.
```
```
Sample Input:
00100 6 4
00000 4 99999
00100 1 12309
68237 6 -1
33218 3 00000
99999 5 68237
12309 2 33218
```
```
Sample Output:
00000 4 33218
33218 3 12309
12309 2 00100
00100 1 99999
99999 5 68237
68237 6 -1
```
```c++
#include<iostream>
#include<stdio.h>
#include<algorithm>    ///使用到reverse 翻转函数
using namespace std;
 
#define MAXSIZE 1000010   ///最大为五位数的地址
 
struct node    ///使用顺序表存储data和下一地址next
{
   int data;   
   int next;
}node[MAXSIZE];
 
int List[MAXSIZE];   ///存储可以连接上的顺序表
int main()
{
    int First, n, k;  
    cin>>First>>n>>k;   ///输入头地址 和 n，k；
    int Address,Data,Next;
    for(int i=0;i<n;i++)
    {
        cin>>Address>>Data>>Next;
        node[Address].data=Data;
        node[Address].next=Next;
    }
 
    int j=0;  ///j用来存储能够首尾相连的节点数
    int p=First;   ///p指示当前结点
    while(p!=-1)
    {
        List[j++]=p;
        p=node[p].next;
    }
    int i=0;
    while(i+k<=j)   ///每k个节点做一次翻转
    {
        reverse(&List[i],&List[i+k]);
        i=i+k;
    }
    for(i=0;i<j-1;i++)
        printf("%05d %d %05d\n",List[i],node[List[i]].data,List[i+1]);
    printf("%05d %d -1\n",List[i],node[List[i]].data);
    return 0;
}
```
