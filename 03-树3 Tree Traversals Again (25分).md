** PUSH操作可以得到前序遍历的结果 1 2 3 4 5 6
POP操作可以得到中序遍历的结果 3 2 4 1 6 5
题目化简为已知前序遍历和中序遍历，如何求得后序遍历
前序遍历：根左右
中序遍历：左根右
后序遍历：左右根
可以发现，根的位置决定了他属于什么序列

回到题目中来，该如何求得后序遍历的具体数值呢？？？？？？

前序遍历的第一个，是根，他应该被放在后序遍历的最后一个值里
让我们看看并剖析一下get算法的具体机理
前4行排除特殊情况
将pre的第一个数作为根，保存到post的最后一个数中
在中序遍历中找到根的位置，找到即break 保存到i中
左边是左子树
右边是右字数
此时i的下标也等于左子树的节点个数
相应的求出我们的右子树节点个数

递归算法是核心部分，看看他做了什么操作
三个数组不解释，
第一个递归将左子树看成一棵独立的树，begin_pre+1表示排除掉根，作为前序遍历的结果（23456），begin_in 中序遍历结果不去动他（324165），begin_post保持不变，L表示左子树看成整棵树

第二个递归将右子树看成一棵独立的树begin_pre+L+1，从右子树的一个元素开始，作为前序遍历的结果（56），相应的分配该右子树中序遍历的结果（65），改变后序遍历结果开始存储的位置为begin_post+L,R表示右子树看成整棵树

关键是切割前序遍历和中序遍历，使得有对应关系，进入递归

有点难，需要熟练掌握 **
```c++
#include <iostream>
#include <stack>
#include <cstdio>
#include <cstring>
using namespace std;
int post[50]={0};
void get(int *pre,int begin_pre,int *in,int begin_in,int *post,int begin_post,int n){
    if(!n) return;
    if(n==1){
        post[begin_post]=pre[begin_pre];
    }//前四行
    else{
        int root=pre[begin_pre];
        post[begin_post+n-1]=root;
        int i;
        for(i=0;i<=n-1;i++){
            if(in[begin_in+i]==root) break;
        }
        int L=i;
        int R=n-1-i;
        get(pre,begin_pre+1,in,begin_in,post,begin_post,L);
        get(pre,begin_pre+L+1,in,begin_in+L+1,post,begin_post+L,R);
    }
    return;
}
int main(){
    int t;
    cin>>t;
    stack<int>S;
    int a=0;
    int b=0;
    int num;
    int pre[50],in[50];
    char op[20];
    for(int q=1;q<=2*t;q++){
        cin>>op;
        if(strcmp("Push",op)==0){
            cin>>num;
            S.push(num);
            pre[a++]=num;
        }
        else{
            in[b++]=S.top();
            S.pop();
        }
    }
    get(pre,0,in,0,post,0,t);
    for(int i=0;i<=t-2;i++){
        cout<<post[i]<<' ';
    }
    cout<<post[t-1]<<endl;
    return 0;
}
```
