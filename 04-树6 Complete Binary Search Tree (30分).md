A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.
A Complete Binary Tree (CBT) is a tree that is completely filled, with the possible exception of the bottom level, which is filled from left to right.

Now given a sequence of distinct non-negative integer keys, a unique BST can be constructed if it is required that the tree must also be a CBT. You are supposed to output the level order traversal sequence of this BST.

Input Specification:
Each input file contains one test case. For each case, the first line contains a positive integer N (≤1000). Then N distinct non-negative integer keys are given in the next line. All the numbers in a line are separated by a space and are no greater than 2000.

Output Specification:
For each test case, print in one line the level order traversal sequence of the corresponding complete binary search tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.
```C++
#include<stdio.h>
#include<stdlib.h>
#include<iostream>
#include<algorithm>
#include<cmath>
#define N 2001
using namespace std;
int in_order[N];
int level_order[N];
bool cmp(const int a, const int b){
	return a < b;
}
int getLeftLen(int n);//获取有n个节点的CBT的左子树节点个数 
void in2level(int in[], int s, int e, int pos);
int main(){
	int n;
	cin >> n;
	for(int i = 0;i < n; i++){
		scanf("%d",&in_order[i]);
	}
	sort(in_order, in_order+n, cmp);
	in2level(in_order, 0, n-1, 0);//CBT中序遍历转层次遍历 
	for(int i = 0; i < n-1; i++){
		printf("%d ", level_order[i]); 
	} 
	printf("%d\n",level_order[n-1]);
	return 0;
}
int getLeftLen(int n){//获取有n个节点的CBT的左子树节点个数
	int height = log2(n); 
	int leftLen = pow(2, height-1) - 1;//除最低层之外的左子树的节点个数 
	int others = n - pow(2,height) + 1;//最低层的节点数
	if(others <= pow(2, height-1)){
		leftLen += others;//如果最底层上的节点不够填满左子树，则所有的最底层上的节点都在左子树上 
	} else{
		leftLen += pow(2, height-1);//左子树被填满 
	}
	return leftLen;
}
void in2level(int in[], int s, int e, int pos){
	if(s>e)
		return;
	int leftLen = getLeftLen(e-s+1);
	level_order[pos] = in[s+leftLen];
	in2level(in, s, s+leftLen-1, 2*pos+1);
	in2level(in, s+leftLen+1, e, 2*pos+2); 
} 
```
