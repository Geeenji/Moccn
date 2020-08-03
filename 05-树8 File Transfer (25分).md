We have a network of computers and a list of bi-directional connections. Each of these connections allows a file transfer from one computer to another. Is it possible to send a file from any computer on the network to any other?

Input Specification:
Each input file contains one test case. For each test case, the first line contains N (2≤N≤10
​4
​​ ), the total number of computers in a network. Each computer in the network is then represented by a positive integer between 1 and N. Then in the following lines, the input is given in the format:

I c1 c2  
where I stands for inputting a connection between c1 and c2; or

C c1 c2    
where C stands for checking if it is possible to transfer files between c1 and c2; or

S
where S stands for stopping this case.

Output Specification:
For each C case, print in one line the word "yes" or "no" if it is possible or impossible to transfer files between c1 and c2, respectively. At the end of each case, print in one line "The network is connected." if there is a path between any pair of computers; or "There are k components." where k is the number of connected components in this network.

Sample Input 1:
5
C 3 2
I 3 2
C 1 5
I 4 5
I 2 4
C 3 5
S
Sample Output 1:
no
no
yes
There are 2 components.
Sample Input 2:
5
C 3 2
I 3 2
C 1 5
I 4 5
I 2 4
C 3 5
I 1 3
C 1 5
S
Sample Output 2:
no
no
yes
yes
The network is connected.
```C++
/*这里老师优化的方法特别好；
  关于按秩归并和路径压缩的思想特别好
  有兴趣可以多看几遍视频，很有意思。 
*/ 
#include <bits/stdc++.h>
using namespace std;
const int Maxsize = 1e4+7;
int s[Maxsize];
void Initialization(int s[],int n)
{
	for(int i = 0;i<n;i++)
	 s[i]=-1;
} 
int Find(int s[],int x)//路径压缩 
{
	if(s[x]<0) return x;//找到集合的根 
	else return s[x]=Find(s,s[x]);  //完成三个步骤；
	                                //先找到根
									//把根变成x的父节点
									//再返回根 
}
void Union(int s[],int root1,int root2)//按规模归并 
{
	if(s[root2]<s[root1])
	{
		s[root2]+=s[root1];
		s[root1]=root2;
	}
	else 
	{
		s[root1]+=s[root2];
		s[root2]=root1;
	}
}
/*
void Union(int s[],int root1,int root2)//按高度归并
{
	if(s[root2]<s[root1])
	{
		s[root1]=root2;
	}
	else 
	{
	if(s[root1]=s[root2])  s[root1]--;
		s[root2]=root1;
	}
}
*/
void Input_connection(int s[])
{
	int u,v;
	int root1,root2;
	scanf("%d %d\n",&u,&v);
	root1 = Find(s,u-1);
	root2 = Find(s,v-1);
	if(root1!=root2)
	  Union(s,root1,root2);	//找到根节点不同的连接	
}
void Check_connection(int s[])
{
	int u,v,root1,root2;
	scanf("%d %d\n",&u,&v);
	root1 = Find(s,u-1);
	root2 = Find(s,v-1);
	if(root1 == root2) 
	  cout<<"yes"<<endl;
	else cout<<"no"<<endl;
}
void Check_network(int s[],int n)
{
	int cnt = 0;
	for(int i = 0;i<n;i++)//统计根节点的个数 
	   if(s[i]<0)
	     cnt++;
	if(cnt==1) cout<<"The network is connected.\n";
	else cout<<"There are "<<cnt<<" components.\n";
}
int main(int argc, char** argv) {
	int n;
	char in;
	scanf("%d",&n);
	getchar();
	Initialization(s,n);//初始化集合 
	do{
		scanf("%c",&in);
		switch(in) {
			case 'I': Input_connection(s);break;
			case 'C': Check_connection(s);break;
			case 'S': Check_network(s,n);break;
		}
	} while(in!='S');
	return 0;
}
```
