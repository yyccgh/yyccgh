
typedef struct time
{
	int hour;
	int min;
}Time;
typedef struct node
{
	char num[10];
	Time reach;
	Time leave;
	int time;
}CarNode;
typedef struct NODE
{
	CarNode stack[MAX+1];
	int top;
}SeqStackCar;
typedef struct car
{
	CarNode data;
	struct car *next;
}QueueNode;
typedef struct Node
{
	QueueNode *head;
	QueueNode *rear;
}LinkQueueCar;
int main()
{
	SeqStackCar Enter,Temp;
	LinkQueueCar Wait,Leavecar,
	int ch;
	InitStack(&Enter);
	InitStack(&Temp);
	InitQueue(&Wait);
	InitQueue(&Leavecar);
	while(1)
	{
		printf("\n*************************************************");
		printf("\n1.车辆到达");
		printf("2.车辆离开");
		printf("3.列表显示");
		printf("4.退出系统\n");
		printf("***************************************************");
		while(1)
		{
			scanf("%d",&ch);
			if(ch>=1&&ch<=4)break;
			else printf("\n请选择: 1|2|3|4.");
		}
		switch(ch)
		{
			case 1:Arrival(&Enter,&Wait);break;
			case 2:Leave(&Enter,&Temp,&Wait,&Leavecar);break;
			case 3:List(&Enter,&Wait,&Leavecar);break;
			case 4:exit(0);
			default:break;
		}
	}
}
void InitStack(SeqStackCar *carstack)
{
	carstack->top=0;
 } 
 int InitQueue(LinkQueueCar *carwait)
 {
 	carwait->head=carwait->rear=(QueueNode *)malloc(sizeof(QueueNode));
 	if(!carwait->head) exit(OVERFLOW);
 	carwait->head->next=NULL;
 	return OK;
 }
 int Arrival(SeqStackCar *enter,LinkQueueCar *wait)
 {
 	if(enter->top<MAX)
 	{
 		printf("请输入车牌号<例如:冀A6666>:");
 		scanf("%s",enter->stack[enter->top].num);
 		printf("该车在停车场第%d个位置\n",(enter->top)+1);
 		printf("请输入该车的到达时间<例如:12:00>:");
 		scanf("%d:%d",&(enter->stack[enter->top].reach.hour),&(enter->stack[enter->top].reach.min));
 		(enter->top)++;
 		return OK;
	 }
	 else
	 {
	 	EnQueue(wait);
	 	printf("请输入车牌号:");
		 scanf("%s",wait->rear->data.num);
		 printf("该车停在便道上等待\n");
		 return OK; 
	 }
 }
 void EnQueue(LinkQueueCar *wait)
 {
 	QueueNode *p=wait->rear;
 	wait->rear=(QueueNode *)malloc(sizeof(QueueNode));
 	if(!wait->rear) exit(OVERFLOW);
 	p->next=wait->rear;
 	wait->rear->next=NULL;
 }
 int Leave(SeqStackCar *enter,SeqStackCar *temp,LinkQueueCar *wait,LinkQueueCar *leavecar)
 {
 	int i,j=enter->top;
 	QueueNode *q=NULL;
 	if(!enter->top)
 	{
 		printf("停车场没有车");
 		return ERROR;
	 }
	 printf("请输入车在停车场中的位序(1-%d):",j);
	 scanf("%d",&i);
	 while(i<1||i>enter->top)
	 {
	 	printf("输入错误，请重新输入(1-%d):",j);
	 	scanf("%d",&i);
	 }
	 while(temp->top+i<j)
	 temp->stack[(temp->top)++]=enter->stack[--(enter->top)];
	 fee(enter,leavecar);
	 while(temp->top)
	 enter->stack[(enter->top)++]=temp->stack[--(temp->top)];
	 if(wait->head!=wait->rear)
	 {
	 	q=wait->head->next;
	 	printf("便道里的%s车进入停车场第%d个位置\n",q->data.num,MAX);
	 	enter->stack[(enter->top)++]=q->data;
	 	printf("请输入现在时间<例如:12:00>:");
	 	scanf("%d:%d",&(enter->stack[enter->top-1].reach.hour),&(enter->stack[enter->top-1].reach.min));
	 	wait->head->next=q->next;
	 	if(!q->next) wait->rear=wait->head;
	 	free(q);
	 }
	 return OK;
 }
 void fee(SeqStackCar *leavecar)
 {
 	EnQueue(leavecar);
 	--(enter->top);
 	printf("请输入离开时间<例如:12:00>:");
 	scanf("%d:%d",&(enter->stack[enter->top].leave.hour),&(enter->stack[enter->top].leave.min));
 	enter->stack[enter->top].time=(enter->stack[enter->top].leave.hour, enter->stack[enter->top].reach.hour)*60;
 	enter->stack[enter->top].time+=enter->stack[enter->top].leave.min-enter->stack[enter->top].reach.min;
 	printf("<车牌号:%s停车时间:%d分钟停车费:%6.2f>\n",enter->stack[enter->top].num,enter->stack[enter->top].time,enter->stack[enter->top].time*price);
 	leavecar->rear->data=enter->stack[enter->top];
 }
 int List(SeqStackCar *enter,ListQueueCar *wait,LinkQueueCar *leavecar)
 {
 	int i,j;
 	QueueNode *q=wait->head,*p=leave->head;
 	while(1)
 	{
 		printf("***************************************************\n");
 		printf("1.停车场 2.便道 3.历史记录 4.返回\n");
 		printf("***************************************************\n");
 		scanf("%d",&i);
 		switch(i)
 		{
 			case 1:{printf("    位置    到达时间    车牌号\n");
 			for("j=0;j<enter->top;j++")
 			printf("%8d%8d:%2d%s\n",j+1,enter->stack[j].reach.hour,enter->stack[j].reach.min,enter->stack[j].num);
				break;
			 }
			 case 2:{if(wait->head==wait->rear)printf("便道里没有车\n");
			 else{
			 	printf("便道内等待车辆车牌号:\n");
			 	while(q=q->next)
			 	printf("%s",q-data.num);
			 	printf("\n");
			 }
				break;
			 }
			 case 3:{printf("  车牌号  到达时间  离开时间  停车时间  费用\n");
			 while(p=p->next)
			 printf("%8s%8d:%2d%8d:%2d%10d%8.2f\n",p->data.num,p->data.reach.hour,p->data.reach.min,p->data.leave.hour,p->data.leave.min,p->data.time,p->data.time*price);
				break;
			 }
			 case 4:return OK;
			
			 
		 }
	 }
 }
