# shujujiegou
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
#define ERROR       -4



//队列的数据结构
typedef struct QNode{
	int     data;
	struct  QNode  *next;
}QNode,*QueuePtr;
typedef struct {
	QueuePtr  front;
	QueuePtr  rear;
}LinkQueue;
//
typedef int Status;
typedef char VertexType;
#define MAX 20
#define MAX_VERTEX_NUM 20
typedef struct ArcNode{
	int             adjvex;
	struct ArcNode  *nextarc;
//	InfoType        *info;
}ArcNode;
typedef struct VNode{
	VertexType data;
	ArcNode    *firstarc;
}Vode,AdjList[MAX_VERTEX_NUM];
typedef struct{
	AdjList vertices;
	int     vexnum,arcnum;  //图的顶点数和弧数
	//int     kind;         //图的种类标志
}ALGraph;

bool visited[MAX]; //
void DFSTraverse(ALGraph G );
void DFS(ALGraph G,int v);
Status CreateGraph(ALGraph *G);
Status FirstAdjVex(ALGraph G,int v);
Status NextAdjVex(ALGraph G,int v,int w);
Status InitQueue(LinkQueue *Q);
Status EnQueue(LinkQueue *Q , int e);
Status DeQueue(LinkQueue *Q , int *e);
int QueueLength(LinkQueue Q);
void BFSTraverse(ALGraph G);

//
Status CreateGraph(ALGraph *G);

void main(){
	ALGraph G;
	CreateGraph(&G);
	DFSTraverse(G);
	BFSTraverse(G);
	printf("\n");
}
Status InitQueue(LinkQueue *Q){
	Q->front = Q->rear = (QueuePtr)malloc(sizeof(QNode));
	Q->front->next = NULL;
	return OK;
}
Status EnQueue(LinkQueue *Q , int e){
	QueuePtr p;
	p = (QueuePtr)malloc(sizeof(QNode));
	if(!p) exit(OVERFLOW);
	p->data = e; p->next = NULL;
	Q->rear->next = p;//头指针没有数据
	Q->rear = p;
	return OK;
}
Status DeQueue(LinkQueue *Q,int *e){
	QueuePtr p;
	if(Q->front == Q->rear)  return ERROR;
	p = Q->front->next;   
	*e = p->data;
	Q->front->next = p->next;
	if(Q->rear == p) Q->rear = Q->front;
	free(p);//将出队的数据释放掉
	return OK;
}
int QueueLength(LinkQueue Q){
	return Q.rear-Q.front;
}
Status CreateGraph(ALGraph *G){
	ArcNode *p;
	char data;
	int i,j,locat; 
	G->vexnum=0,G->arcnum=0,i = 0;     printf("请输入每个顶点:\n");
	scanf("%c",&data);
	while(data != '#'){
		G->vertices[i].data = data;
		G->vertices[i].firstarc=NULL;
		i++;
		G->vexnum++;
		scanf("%c",&data);
	}
	for(j=0 ;j<i ;j++){
		printf("请输入顶点%c的关系(就是该顶点zhixiang定点的位置)",G->vertices[j].data);
		while(scanf("%d",&locat)==1){
			p = (ArcNode *)malloc(sizeof(ArcNode));
			p->adjvex = locat;
			p->nextarc = G->vertices[j].firstarc;
			G->vertices[j].firstarc = p;
			G->arcnum++;
		}
	}
	return OK;
}
void DFSTraverse(ALGraph G ){
	int v;
	for(v=0;v<G.vexnum;++v) visited[v] = FALSE;
	for(v=0;v<G.vexnum;++v) 
		if(!visited[v]) { DFS(G,v);   printf("\n");}
}
void DFS(ALGraph G,int v){
	int w;
	visited[v] = TRUE;  
	printf("%c",G.vertices[v].data);
	for(w = FirstAdjVex(G,v); w >= 0; w = NextAdjVex(G,v,w))
	{
		if(!visited[w])  DFS(G,w);
	}
}
Status FirstAdjVex(ALGraph G,int v){
	if(G.vertices[v].firstarc == NULL)
		return -1;
	else
		return G.vertices[v].firstarc->adjvex;
}
Status NextAdjVex(ALGraph G,int v,int w){
	ArcNode *p; p = G.vertices[v].firstarc;
	if(!p) return -1;
	while(p->adjvex!=w)
		p=p->nextarc;
	if(!p->nextarc) return -1; 
	else return p->nextarc->adjvex;

}
void BFSTraverse(ALGraph G){
	LinkQueue Q;
	int v,u,w;
		for(v=0;v<G.vexnum;++v) visited[v] = FALSE;
		InitQueue(&Q);
		for(v=0;v<G.vexnum;++v) 
			if(!visited[v]){
				visited[v] = TRUE;
				printf("%c",G.vertices[v].data);
				EnQueue(&Q,v);
				while(QueueLength(Q)){
					DeQueue(&Q,&u);
					for(w = FirstAdjVex(G,u); w >= 0; w = NextAdjVex(G,u,w))
					{
							if(!visited[w]){
									visited[w] = TRUE; printf("%c",G.vertices[w].data);
									EnQueue(&Q,w);
						}
					}
				}
			}
}
