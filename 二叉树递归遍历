# shujujiegou  //二叉树的code都是先序遍历创建
#include<stdio.h>
#include<stdlib.h>
#include<malloc.h>
#define STACK_INIT_SIZE  100
#define STACKINCREMENT    10

#define TRUE         1
#define FALSE        0
#define OK           1
#define INFEASIBLE  -1
#define OVERFLOW    -2
#define ERROR        0

typedef int Status;
typedef char TElemType;
#define MAX_TREE_SIZE 100

typedef struct BiTNode{
	TElemType data;
	struct BiTNode *lchild ,*rchild;
}BiTNode,*BiTree;

Status CreateBiTree1(BiTree *T);
Status Visit(TElemType e);
Status PreOrderTraverse(BiTree T, Status( * Visit)(TElemType));
Status InOrderTraverse(BiTree T, Status( * Visit)(TElemType));
Status AfOrderTraverse(BiTree T, Status( * Visit)(TElemType));
void main(){
	BiTree AT;
	printf("请按照先序输入二叉树的各个元素（输入#代表该节点为空）:\n");
	CreateBiTree1(&AT);
	printf("先序输出为:");
	PreOrderTraverse(AT,Visit);
	printf("\n");
	printf("中序输出为:");
	InOrderTraverse(AT,Visit);	
	printf("\n");
	printf("后序输出为:");
	AfOrderTraverse(AT,Visit);	
	printf("\n"); 
}
Status Visit(TElemType e){
	   printf("%c",e);
	   return OK;
}  

Status PreOrderTraverse(BiTree T, Status( * Visit)(TElemType)){

	if(T){
		if(Visit(T->data))
			if(PreOrderTraverse(T->lchild,Visit))
				if(PreOrderTraverse(T->rchild,Visit)) return OK;
		return ERROR;
	}
	else  return OK;
}
Status CreateBiTree1(BiTree *T){
	char ch;
	scanf("%c",&ch);
	if(ch=='#') (*T) = NULL; 
	else{
		if(!( (*T)=(BiTNode *)malloc( sizeof(BiTNode) )))    exit(OVERFLOW);
		(*T)->data = ch;
		CreateBiTree1(&(*T)->lchild);
		CreateBiTree1(&(*T)->rchild);
	}
	return OK;
}
Status InOrderTraverse(BiTree T, Status( * Visit)(TElemType)){
	if(T){
		if(InOrderTraverse(T->lchild,Visit))
			if(Visit(T->data))
				if(InOrderTraverse(T->rchild,Visit)) return OK;
		return ERROR;
	}
	else  return OK;
}
Status AfOrderTraverse(BiTree T, Status( * Visit)(TElemType)){
	if(T){
		if(AfOrderTraverse(T->lchild,Visit))
			if(AfOrderTraverse(T->rchild,Visit))
				if(Visit(T->data)) return OK;
		return ERROR;
	}
	else  return OK;
}         
