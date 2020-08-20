```
09-排序2 Insert or Merge (25分)
According to Wikipedia:

Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
Each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. 
It repeats until no input elements remain.

Merge sort works as follows: 
Divide the unsorted list into N sublists, each containing 1 element (a list of 1 element is considered sorted). Then repeatedly merge two adjacent sublists to produce new sorted sublists until there is only 1 sublist remaining.

Now given the initial sequence of integers, together with a sequence which is a result of several iterations of some sorting method, 
can you tell which sorting method we are using?

Input Specification:
Each input file contains one test case. For each case, the first line gives a positive integer N (≤100). 
Then in the next line, N integers are given as the initial sequence. The last line contains the partially sorted sequence of the N numbers.
It is assumed that the target sequence is always ascending. All the numbers in a line are separated by a space.

Output Specification:
For each test case, print in the first line either "Insertion Sort" or "Merge Sort" to indicate the method used to obtain the partial result. 
Then run this method for one more iteration and output in the second line the resuling sequence. It is guaranteed that the answer is unique for each test case.
All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

Sample Input 1:
10
3 1 2 8 7 5 9 4 6 0
1 2 3 7 8 5 9 4 6 0
Sample Output 1:
Insertion Sort
1 2 3 5 7 8 9 4 6 0
Sample Input 2:
10
3 1 2 8 7 5 9 4 0 6
1 3 2 8 5 7 4 9 0 6
Sample Output 2:
Merge Sort
1 2 3 8 4 5 7 9 0 6
```
```C++
#include<stdio.h>
#include<malloc.h>
typedef int ElementType;
int  ComPare(int a[],int b[],int N){
	int i,j=0;
	for(i=0;i<N;i++)
		if(a[i]!=b[i]){
			j=1;
			break;
		}
		if(!j)
			return 1; //两个数组一样
		else
			return 0;
}
void Print(ElementType A[],ElementType N){
	int i;
	for(i=0;i<N-1;i++)
		printf("%d\ ",A[i]);
	printf("%d",A[i]);
}
int  InsertionSort(int  A[],int B[],int N )
{ /* 插入排序 */
     int P, i;
     int  Tmp;
     int flag=0; 
     for ( P=1; P<N; P++) {
         Tmp = A[P]; /* 取出未排序序列中的第一个元素*/
         for ( i=P; i>0 && A[i-1]>Tmp; i-- )
             A[i] = A[i-1]; /*依次与已排序序列中元素比较并右移*/
         A[i] = Tmp; /* 放进合适的位置 */
		 if(ComPare(A,B,N)){
			 printf("Insertion Sort\n");
			 flag=1;
			 continue;
		 }
		 if(flag==1){
			 flag=0;
			 Print(A,N);
		 }
     }
	 return 0;
}
void Merge( ElementType A[], ElementType TmpA[], int L, int R, int RightEnd )
{ /* 将有序的A[L]~A[R-1]和A[R]~A[RightEnd]归并成一个有序序列 */
     int LeftEnd, NumElements, Tmp;
     int i;
      
     LeftEnd = R - 1; /* 左边终点位置 */
     Tmp = L;         /* 有序序列的起始位置 */
     NumElements = RightEnd - L + 1;
      
     while( L <= LeftEnd && R <= RightEnd ) {
         if ( A[L] <= A[R] )
             TmpA[Tmp++] = A[L++]; /* 将左边元素复制到TmpA */
         else
             TmpA[Tmp++] = A[R++]; /* 将右边元素复制到TmpA */
     }
     while( L <= LeftEnd )
         TmpA[Tmp++] = A[L++]; /* 直接复制左边剩下的 */
     while( R <= RightEnd )
         TmpA[Tmp++] = A[R++]; /* 直接复制右边剩下的 */
          
     for( i = 0; i < NumElements; i++, RightEnd -- )
         A[RightEnd] = TmpA[RightEnd]; /* 将有序的TmpA[]复制回A[] */
}
/* 归并排序 - 循环实现 */
/* length = 当前有序子列的长度*/
void Merge_pass( ElementType A[], ElementType TmpA[], int N, int length )
{ /* 两两归并相邻有序子列 */
     int i, j;  
     for ( i=0; i <= N-2*length; i += 2*length )
         Merge( A, TmpA, i, i+length, i+2*length-1 );
     if ( i+length < N ) /* 归并最后2个子列*/
         Merge( A, TmpA, i, i+length, N-1);
     else /* 最后只剩1个子列*/
         for ( j = i; j < N; j++ ) TmpA[j] = A[j];
}
 
void Merge_Sort( ElementType A[],ElementType B[], int N )
{    
     int length; 
     ElementType *TmpA;
     int flag=0;
     length = 1; /* 初始化子序列长度*/
     TmpA=(ElementType *)malloc( N * sizeof( ElementType ) );
     if ( TmpA != NULL ) {
          while( length < N ) {
              Merge_pass( A, TmpA, N, length );
              length *= 2;
			  if(flag==1){
				  Print(A,N);
				  flag=0;
			  }
			  if(ComPare(A,B,N)){
				  flag=1;
				  printf("Merge Sort\n");
			  } 
              Merge_pass( TmpA, A, N, length );
              length *= 2;
			   if(flag==1){
				  Print(A,N);
				  flag=0;
			  }
			  if(ComPare(A,B,N)){
				  flag=1;
				  printf("Merge Sort\n");
			  } 
          }
          free( TmpA );
     }
     else printf( "空间不足" );
} 
void CopyArr(int A[], int B[], int N)
{
    int i;
    for(i=0;i<N;i++)
        B[i] = A[i];
}
int main(){
	int N,k=2,i,a1[100],b1[100],*a2,*b2,t;	
	scanf("%d",&N);
	a2 = (int *)malloc(sizeof(int)*N);
    b2 = (int *)malloc(sizeof(int)*N);
	for(i=k;i>0;i--){
		t=0;
		while(t<N){
			if(i==k)
				scanf("%d",&a1[t]);
			else
				scanf("%d",&b1[t]);
			t++;
		}
	}
	CopyArr(a1,a2,N);
	CopyArr(b1,b2,N);
	Merge_Sort(a1,b1,N);
	InsertionSort(a2,b2,N);
	return 0;
}

```
