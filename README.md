#multilevel queue scehduling algorithm
#multilevel queue scheduling algorithm
#include<stdio.h>
#include<stdlib.h>

int currTime=0,queueTimer=10,total;

struct MutilevelQ
{
	int p_id;int ct;int arr;int start;int prio;            
	int bt;int store;
};

void roundRobinAlgorithm(struct MutilevelQ p[],int queueNo)
{		printf("Round Robin is Running at %d sec\n",currTime);
		int timeQuantum=10;
		int i;
		for( i=0;i<queueNo;i++)
		{	
			if(timeQuantum<=queueTimer&&p[i].arr<=currTime)
			{
			if(p[i].start==-1)
			{
			p[i].start=currTime;
			}
			if(p[i].bt>=timeQuantum)
			{
			p[i].bt=p[i].bt-timeQuantum;
			currTime=currTime+timeQuantum;
			queueTimer-=timeQuantum;
			}
			if(p[i].bt<timeQuantum&&p[i].bt>0) 
			{
			currTime=currTime+p[i].bt;
			queueTimer-=p[i].bt;
			p[i].bt=0;
			}
			if(p[i].bt==0&&p[i].ct==-1)
			{
				p[i].ct=currTime;
				total--;
			}
			}
		}
		queueTimer=10;
}

void firstComeFirstServe(struct MutilevelQ p[],int r)
{
	printf("First Come First Serve is Running at %d sec\n",currTime);
	int i;
		for(i=0;i<r;i++)
		{	
			if(p[i].bt<=queueTimer && p[i].arr<=currTime && p[i].bt>0)
			{			
			if(p[i].start==-1)
			p[i].start=currTime;
			p[i].bt=0;
			currTime=currTime+p[i].bt;
			queueTimer-=p[i].bt;
			if(p[i].bt==0&&p[i].ct==-1)
			{
				p[i].ct=currTime;
				total--;
			}
		    }
			else
			{
				if((p[i].bt-queueTimer>0)&&p[i].arr<=currTime&&p[i].bt>0)
				{
				if(p[i].start=-1)
				p[i].start==currTime;
				p[i].bt=p[i].bt-queueTimer;
				currTime=currTime+queueTimer;
				queueTimer=0;
				if(p[i].bt==0&&p[i].ct==-1)
				{
					p[i].ct=currTime;
					total--;
				}
				}
			}
		}
		queueTimer=10;
}

int priorityAlgo(struct MutilevelQ p[],int r,int remain)
{
	printf("Priority Queue is Running at %d sec \n",currTime);
	int smallest;
	int i;
	while(remain!=0&&queueTimer>0)
	{
	    smallest=r;
	    for(i=0;i<r;i++)
	    {	
	        if(p[i].arr<=currTime && p[i].prio<p[smallest].prio && p[i].bt>0)
	        {	
	            smallest=i;
	        }
	    }
	    if(p[smallest].start==-1)
	    {
	        p[smallest].start=currTime;
	    }
	    queueTimer-=1;
	    p[smallest].bt-=1;
	    if(p[smallest].bt==0&&p[smallest].ct==-1)
	    {
	    	p[smallest].ct=currTime;
	        total--;
	        remain--;
	    }
	    currTime+=1;
	}
	queueTimer=10;
}

int main()
{	
	int n,i,rr=0,pr=0,fcfs=0;
	printf("Enter the number of processes: ");
	scanf("%d",&n);
	total=n;
	struct MutilevelQ ps[n];
    for(i=0;i<n;i++)
    {
    	printf("PROCESS %d :\n",i+1);
		ps[i].p_id=i+1;
		printf("Enter the Arrival Time: ");
    	scanf("%d",&ps[i].arr);
    	printf("Enter the priority 1 for Round Robin, 2 for First Come First Serve and 3 for priority: ");
    	scanf("%d",&ps[i].prio);
    	if(ps[i].prio==1)
		rr++;
		else if(ps[i].prio==2)
		fcfs++;
		else if(ps[i].prio==3)
		pr++;
		printf("Enter burst time: ");
    	scanf("%d",&ps[i].bt);	
		ps[i].store=ps[i].bt;
		ps[i].start=-1;
    	ps[i].ct=-1;
	}
	int rrid=0,prid=0,fcfsid=0;
	struct MutilevelQ roundqueue[rr],priorqueue[pr+1],fcfsqueue[fcfs];
	for(i=0;i<n;i++)
	{
		if(ps[i].prio==1)
		{
			roundqueue[rrid]=ps[i];
			rrid++;
		}
		else if(ps[i].prio==2)
		{
			fcfsqueue[fcfsid]=ps[i];
			fcfsid++;
		}
		else if(ps[i].prio==3)
		{
			priorqueue[prid]=ps[i];
			prid++;
		}
	}
	int remain=pr;
	while(total>0)
	{
		roundRobinAlgorithm(roundqueue,rr);
		remain=priorityAlgo(priorqueue,pr,remain);
		firstComeFirstServe(fcfsqueue,fcfs);
	}
	printf("Process Id\tArrival Time\tBrust Time\tStarting Time\tTurn Around Time\tPriority\n");
	for(i=0;i<rr;i++)
	{
		printf("%d\t\t%d\t\t%d\t\t%d\t\t%d\t\t\t%d\n",roundqueue[i].p_id,roundqueue[i].arr,roundqueue[i].store,roundqueue[i].start,roundqueue[i].ct,roundqueue[i].prio);
	}
	for(i=0;i<fcfs;i++)
	{
		printf("%d\t\t%d\t\t%d\t\t%d\t\t%d\t\t\t%d\n",fcfsqueue[i].p_id,fcfsqueue[i].arr,fcfsqueue[i].store,fcfsqueue[i].start,fcfsqueue[i].ct,fcfsqueue[i].prio);
	}
	for(i=0;i<pr;i++)
	{
		printf("%d\t\t%d\t\t%d\t\t%d\t\t%d\t\t\t%d\n",priorqueue[i].p_id,priorqueue[i].arr,priorqueue[i].store,priorqueue[i].start,priorqueue[i].ct,priorqueue[i].prio);
	}
}
