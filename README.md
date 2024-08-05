#include<stdio.h>
#include<stdbool.h>
typedef struct {
	int pid;
	int AT;
	int BT;
	int remtime;
	int CT;
	int WT;
	int TAT;
}process;
int main(){
	int n,quantum;
	printf("enter the no of process:");
	scanf("%d",&n);
	process p[n];
	for(int i=0;i<n;i++){
		p[i].pid=i+1;
		printf("enter arrival time and burst time for process %d",p[i].pid);
		scanf("%d%d",&p[i].AT,&p[i].BT);
		p[i].remtime=p[i].BT;
	}
	printf("enter the time quantum:");
	scanf("%d",&quantum);
	int time=0;
	int processcompleted=0;
	int totalWT=0;
	int totalTAT=0;
	while(processcompleted<n){
		bool done=true;
		for(int i=0;i<n;i++){
			if(p[i].AT<=time&& p[i].remtime>0){
				done=false;
				if(p[i].remtime>quantum){
					time+=quantum;
					p[i].remtime-=quantum;
				}else{
					time+=p[i].remtime;
					p[i].CT=time;
					p[i].WT=p[i].CT-p[i].AT-p[i].BT;
					p[i].TAT=p[i].CT-p[i].AT;
					p[i].remtime=0;
					totalWT+=p[i].WT;
					totalTAT+=p[i].TAT;
					processcompleted++;
				}
			}
		}
		if(done){
			time++;
		}
	}
	printf("PID\tAT\tBT\tCT\tWT\tTAT\n");
	for(int i=0;i<n;i++){
		printf("%d\t%d\t%d\t%d\t%d\t%d\n",p[i].pid,p[i].AT,p[i].BT,p[i].CT,p[i].WT,p[i].TAT);
	}
	double averageWT=(double)totalWT/n;
	double averageTAT=(double)totalTAT/n;
	printf("average waiting time:%2f\n",averageWT);
	printf("average turn around time time:%2f\n",averageTAT);
	return 0;
}
