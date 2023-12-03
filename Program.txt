//Write a program to implement parent child process and explain their concurrent execution

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
    int pid = fork();
    if(pid<0)
    {
        printf("Unsuccessful fork\n");
        exit(1);
    }
    else if(pid==0)
        printf("Child\n");
    else 
        printf("Parent\n");
}

//Write a c program to take a child process take one variable and show it changes in child and parent process

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
    int n=5;
    int pid = fork();
    if(pid<0)
    {
        printf("Unsuccessful fork\n");
        exit(1);
    }
    else if(pid==0)
    {
        printf("Child : %d\n",++n);
        exit(0);
    }
    else 
    {
        printf("Parent : %d\n",--n);
        exit(0);
    }
}

//Write a c program to create 8 process using minimum fork

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
    fork();
    fork();
    fork();
    printf("%d %d\n", getpid(),getppid());
    exit(0);
}

//Write a program to implement and explain orphan process

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
        int pid = fork();
        if(pid == 0)
        {
                printf("Child Process\n");
                printf("ID : %d\nParent ID : %d\n", getpid(),getppid());
                sleep(5);
                printf("Child Process.\n");
                printf("ID : %d\nParent ID : %d\n",getpid(),getppid());
        }
        else if(pid>0)
        {
                printf("Parent process\n");
                printf("ID: %d\n",getpid());
        }
        else
                printf("Failed to create child process.\n");
}

//Write a program to implement and explain zombie process

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>

int main()
{
        int pid=fork();
        if(pid == 0)
        {
                for(int i=0;i<2;i++)
                        printf("Child Process\n");
        }
        else if(pid>0)
        {
                printf("Parent Process\n");
                while(1);
        }
}

//Write a program to implement and explain how creation of zombie process can be avoided?

#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main()
{	
	int pid=fork();
	if(pid==0)
	{
		for(int i=0;i<2;i++)
			printf("I am child.\n");
	}
	else
	{
		wait(NULL);
		printf("I am parent.\n");
		while(1);
	}
}


//Write a program in c to demonstrate the application of the execvp() function

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>

int main(){
	char* command="ls";
	char* argument_list[]={"ls","-l",NULL};
	//char* argument_list[]={NULL};
	printf("Before calling execvp()\n");

	int status_code = execvp(command, argument_list);
	//execvp() will take control of the process
	if(status_code==-1){
		printf("Process did not terminate correctly\n");
		exit(1);
	}
	printf("This line will not be printed if execvp() runs correctly\n");
	return 0;
}

//Write a C program to demonstrate the application of the execvp() function by calling it inside a chid process

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>

int main(){
	char* command="ls";
	char* argument_list[]={"ls","-l",NULL};
	//char* argument_list[]={NULL};
	printf("Before calling execvp()\n");
	printf("Creating another process using fork()..\n");

	if(fork()==0){
		//sleep(2);
		int status_code = execvp(command, argument_list);
		//execvp() will take control of the process
		if(status_code==-1){
			printf("Process did not terminate correctly\n");
			exit(1);
		}
	}
	else{
		//parent
		printf("This line will be printed for parent");
	}
	return 0;
}

//Write a Program in c to demonstrate the working of 'excel()' function with respect to process

#include<stdio.h>
#include<unistd.h>

int main()
{
	char *const evp[] = {"PATH=/bin:/usr/bin","TERM:console",0};
	printf("Before execl\n");
	execl("/bin/ps","ps","-a",NULL);
	printf("After execlp\n");
}

//Write a program in C to swap one running process with another process using the 'execv()' function

//Child Part :- (childEXECV.c)

#include<stdio.h>

int main(int argc, char *argv[]){
	printf("I am child process\n");
	printf("Argument 1 : %s\n", argv[1]);
        printf("Argument 2 : %s\n", argv[2]);
        printf("Argument 3 : %s\n", argv[3]);
}

// Parent Part :- 

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<errno.h>
int main(){
	printf("I am parent process\n");
	
	char *arg_ptr[4];
	arg_ptr[0] = "childEXECV.c";
        arg_ptr[1] = "Hi from child";
        arg_ptr[2] = "Bye from child";
        arg_ptr[3] = NULL;

	int st = execvp("./Cexecv", arg_ptr);
	printf("Error: %i\n", errno);
}

//Write a program to kill a process using SigKill Signal.

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>

int main(){
	pid_t ppid, pid, cpid;
	ppid = getpid();
	pid = fork();
	if(ppid == getpid())
		printf("Parent\n");
	else if(cpid == getpid())
		printf("Child\n");
	if(pid > 0){
		int i = 0;
		while(i++ < 5){
			printf("In the parent process.\n");
			sleep(1);
		}
	}
	else if(pid == 0){
		int i = 0;
		while(i++ < 10){
			printf("In the child process.\n");
			sleep(1);
			if(i==3){
				kill(ppid, SIGKILL);
				printf("Parent killed. I'm orphan!!!\n");
			}
		}
	}
	else{
		printf("Something bad happened\n");
		exit(EXIT_FAILURE);
	}
}

//Demonstrate through a program how an orphan process is created using SIGKILL signal

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

int main(void)
{
    pid_t ppid,pid,cpid;
    pid = fork();
    if(pid > 0)
    {
        int i = 0;
        while(i++ < 5)
        {
            printf("Parent process.\n");
            sleep(1);
        }
    }
    else if (pid == 0)
    {
        int i = 0;
        while(i++ < 5)
        {
            printf("Child process.\n");
            sleep(1);
            if(i==3)
            {
                kill(getppid(),SIGKILL);
                printf("Parent killed. Orphan Process\n");
            }
        }
    }
    else
    {
        printf("Something bad happened.");
        exit(EXIT_FAILURE);
    }
}

//Write a program to stop the execution of a parent process using Sigstop signal from the child process. Resume the execution of the parent process using Sigcont signal

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

int main(void)
{
    pid_t pid;
    pid = fork();
    if(pid > 0)
    {
        int i = 0;
        while(i++ < 6)
        {
            printf("Parent process.\n");
            sleep(1);
        }
    }
    else if (pid == 0)
    {
        int i = 0;
        while(i++ < 6)
        {
            printf("Child process.\n");
            sleep(1);
            if(i==2)
            {
                kill(getppid(),SIGSTOP);
                printf("Parent stoppped\n");
            }
            if(i==5)
            {
                kill(getppid(),SIGCONT);
                printf("Parent Process Resumed.\n");
            }
        }
    }
}

//Write a program to change the default function of CTRL+C. Restore the default function of the CTRL+C.

#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
int i=2;
void oh(int sig)
{
	printf("Pressed Ctrl+C\n");
    i--;
    signal(SIGINT,oh);
    if(i==0)
    {
        printf("Restoring to default.\n");
        signal(SIGINT,SIG_DFL);
    }
}
int main(){
	signal(SIGINT,oh);
	while(1)
    {
		printf("Hello World!\n");
		sleep(1);
	}
}

//Write a program to implement Semaphore

#include<stdio.h>
#include<stdlib.h>
#include<semaphore.h>
#include<pthread.h>
#include<unistd.h>
#include<string.h>

void *thread_function(void *arg);
sem_t bin_sem;
#define WORK_SIZE 1024
char work_area[WORK_SIZE];

int main()
{
	int res;
	pthread_t a_thread;
	void *thread_result;
	res=sem_init(&bin_sem,0,0);
	if(res!=0)
	{
		perror("Semaphore initialization failed");
		exit(EXIT_FAILURE);
	}
	res=pthread_create(&a_thread,NULL,thread_function,NULL);
	if(res!=0)
	{
		perror("Thread creation failed");
		exit(EXIT_FAILURE);
	}
	printf("Input some text.Enter end to finish\n");
	while(strncmp("end",work_area,3)!=0)
	{
		fgets(work_area,WORK_SIZE,stdin);
		sem_post(&bin_sem);
	}
	printf("\nWaiting for thread to finish...\n");
	res=pthread_join(a_thread,&thread_result);
	if(res!=0)
	{
		perror("Thread join failed\n");
		exit(EXIT_FAILURE);
	}
	printf("Thread joined\n");
	sem_destroy(&bin_sem);
	exit(EXIT_SUCCESS);
}

void *thread_function(void *arg)
{
	sem_wait(&bin_sem);
	while(strncmp("end",work_area,3)!=0)
	{
			printf("You input %d characters\n",strlen(work_area)-1);
			sem_wait(&bin_sem);
	}
	pthread_exit(NULL);
}

//Seminit

#include <stdio.h> 
#include <stdlib.h> 
#include <errno.h> 
#include <sys/types.h> 
#include <sys/ipc.h> 
#include <sys/sem.h>

//int semget(key_t key, int nsems, int semflg);
union semun {
    int val;               /* used for SETVAL only */
    struct semid_ds *buf;  /* used for IPC_STAT and IPC_SET */
    ushort *array;         /* used for GETALL and SETALL */
};

int main(void)
{
    key_t key;
    int semid;
    union semun arg;
    
    if ((key = ftok("SemDemo.c", 'J')) == -1) 
    {   
        perror("ftok");
        exit(1); 
    }
    
/* create a semaphore set with 1 semaphore: */
    if ((semid = semget(key, 1, 0666 | IPC_CREAT)) == -1) 
    {
     	perror("semget");
        exit(1); 
    }
/* initialize semaphore #0 to 1: */ arg.val = 1;
    if (semctl(semid, 0, SETVAL, arg) == -1) 
    {
     	perror("semctl");
        exit(1); 
    }
return 0; 
    
}

//SemDemo

#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/sem.h>


int main(void)
{
    key_t key;
    int semid;
    struct sembuf sb = {0, -1, 0}; /* set to allocate resource */
    if ((key = ftok("SemDemo.c", 'J')) == -1)
    {   perror("ftok");
        exit(1);
    }
/* grab the semaphore set created by seminit.c: */
    if ((semid = semget(key, 1, 0)) == -1)
    {
        perror("semget");
        exit(1);
    }

    printf("Press return to lock: ");
        getchar();
    printf("Trying to lock...\n");
    if (semop(semid, &sb, 1) == -1)
    {
        perror("semop");
        exit(1);
    }

    printf("Locked.\n");
    printf("Press return to unlock: ");
    getchar();

    sb.sem_op = 1; /* free resource */
    if (semop(semid, &sb, 1) == -1)
    {
        perror("semop");
        exit(1);

    }


 printf("\nUnlocked\n");
    return 0;

}

//SemRemove

#include <stdio.h> 
#include <stdlib.h> 
#include <errno.h> 
#include <sys/types.h> 
#include <sys/ipc.h> 
#include <sys/sem.h>

union semun {
    int val;               /* used for SETVAL only */
    struct semid_ds *buf;  /* used for IPC_STAT and IPC_SET */
    ushort *array;         /* used for GETALL and SETALL */
};

 int main(void)
{
    key_t key;
    int semid;
    union semun arg;

    if ((key = ftok("SemDemo.c", 'J')) == -1) 
    { 
      	perror("ftok");
        exit(1); 
    }
    
/* grab the semaphore set created by seminit.c: */ 
    if ((semid = semget(key, 1, 0)) == -1) 
    {
     	perror("semget");
        exit(1); 
    }
    
/* remove it: */
    if (semctl(semid, 0, IPC_RMID, arg) == -1) 
    {
     	perror("semctl");
        exit(1); 
    }
return 0; 
}

//Write a Program to create a POSIX thread where the main thread waits for the child thread to complete.

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
#include<string.h>
void *thread_function(void *arg);
char message[]="Hello World";
int main(){
        int res;
        pthread_t a_thread;
        void *thread_result;
        res=pthread_create(&a_thread,NULL,thread_function,(void*)message);
        if(res!=0){
                perror("Thread Creation Failed\n");
                exit(EXIT_FAILURE);
        }
        printf("Waiting for thread to finish..\n");
        res=pthread_join(a_thread,&thread_result);
        if(res!=0){
        perror("Thread join failed\n");
                exit(EXIT_FAILURE);
        }
        printf("Thread joined,it returned %s\n",(char*)thread_result);
printf("Message is now %s\n",message);
        exit(EXIT_SUCCESS);
}
void *thread_function(void *arg){
        printf("Thread function is running, argument uses %s\n",(char*)arg);
        sleep(3);
        strcpy(message,"Bye!\n");
        pthread_exit("Thank you for CPU time\n");
}

//Write a Program to implement one way simple process
#include<stdio.h>
#include<unistd.h>

int main(){
	int pipefds[2];
	int returnstatus;
	char writemessages [2][20] = {"Hi","Hello"};
	char readmessage [20];
	returnstatus = pipe(pipefds);
	if(returnstatus == -1){
		printf("Unable to create pipe\n");
		return 1;
	}
	printf("Writing to pipe - Message 1 is %s\n",writemessages[0]);
	write(pipefds[1], writemessages[0], sizeof(writemessages[0]));
	read(pipefds[0], readmessage, sizeof(readmessage));
	printf("Reading from pipe - Message 1 is %s\n",readmessage);
	printf("Writing to pipe - Message 2 is %s\n",writemessages[1]);
	write(pipefds[1], writemessages[1], sizeof(writemessages[0]));
	read(pipefds[0], readmessage, sizeof(readmessage));
	printf("reading from pipe - Message 2 is %s\n", readmessage);
	return 0;
}

//Write a program to implement one way pipe to create a text file

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>

int main(){
	FILE *write_fp;
	char buffer[BUFSIZ+1];
	sprintf(buffer, "The Quick brown fox is sleep.\n");
	write_fp = popen("cat > newfile.txt", "w");
	if(write_fp!=NULL){
		fwrite(buffer, sizeof(char), strlen(buffer), write_fp);
		pclose(write_fp);
		exit(EXIT_SUCCESS);
	}
	exit(EXIT_FAILURE);
}

//Establish interprocess communication using both ways pipeline method

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>

int main(){
	int data_processed;
	int file_pipes[2];
	const char some_data[] = "Hello World";
	char buffer[BUFSIZ + 1];
	pid_t fork_result;
	memset(buffer, '\0', sizeof(buffer));
	//printf("hi\n");
	if (pipe(file_pipes)==0){
		fork_result = fork();
		printf("hi\n");
		if(fork_result == -1){
			fprintf(stderr, "fork_failure");
			exit(EXIT_FAILURE);
		}
		else if(fork_result == 0){//child
			sleep(2);
			data_processed = read(file_pipes[0], buffer, BUFSIZ);
			printf("Child read %d bytes : %s\n", data_processed, buffer);
			exit(EXIT_SUCCESS);
		}
		else{//parent
			data_processed = write(file_pipes[1], some_data, strlen(some_data));
			printf("Parent wrote %d bytes : %s\n", data_processed, some_data);
		}
	}
	exit(EXIT_SUCCESS);
}

//Interprocess communication -> Main Process & child & pipe. Main writes data in the pipe and child reads it.

#include<stdio.h>
#include<unistd.h>

int main(){
	int pipefds1[2], pipefds2[2];
	int returnstatus1, returnstatus2;
	int pid;
	char pipe1writemessage[20]="Hi";
	char pipe2writemessage[20]="Hello";
	char readmessage[20];
	returnstatus1 = pipe(pipefds1);
	if(returnstatus1 == -1){
		printf("Unable to create pipe 1 \n");
		return 1;
	}
	returnstatus2 = pipe(pipefds2);
	if(returnstatus2 == -1){
		printf("unable to create pipe 2 \n");
		return 1;
	}
	pid = fork();
	if(pid!=0){
		close(pipefds1[0]);
		close(pipefds2[1]);
		printf("In parent : writing to pipe 1 - Message is %s\n", pipe1writemessage);
		write(pipefds1[1], pipe1writemessage, sizeof(pipe1writemessage));
		read(pipefds2[0], readmessage, sizeof(readmessage));
		printf("in parent : reading from pipe 2 - Message is %s\n", readmessage);
	}
	else{
		close(pipefds1[1]);
		close(pipefds2[0]);
		read(pipefds1[0], readmessage, sizeof(readmessage));
		printf("In child : reading from pipe 1 - Message is %s\n", readmessage);
		printf("In child : writing to pipe 2 - Message is %s\n", pipe2writemessage);
		write(pipefds2[1], pipe2writemessage, sizeof(pipe2writemessage));
	}
	return 0;
}

//Write a C program to implements page replacement algorithms. They are i) FIFO ii) LRU iii) Optimal

#include<stdio.h>
#include<stdlib.h>

int search(int val, int *arr, int lim){
	for(int i=0;i<lim;i++)
		if(val==arr[i]) return i;
	return -1;
}

void printTabh(int frames, int lr){
	if(lr==0) return;
	printf("Ref Str\t\t");
        for(int i=0;i<frames;i++) printf("Frame %d\t\t",i+1);
        printf("Hit/Miss\n");
}

void printTabr(int ac, int frames, int *arr, char isHit, int lr){
	if(lr==0) return;
	printf("%d\t\t",ac);
        for(int i=0;i<frames;i++) printf("%d\t\t",arr[i]);
        printf("%c\n",isHit);
}

int FIFO(int frames, int count, int *arr, int pr){
	int *fr=(int*)calloc(frames, sizeof(int));
	int wr=0, hit=0;
	if(pr==1) printf("\n\nFIFO : frames = %d\n",frames);
	printTabh(frames,pr);
	for(int i=0;i<count;i++){
		char isHit;
		if(search(arr[i],fr,frames)<0){
			fr[wr]=arr[i];
			wr=(wr+1)%frames;
			isHit='M';
		}
		else{
			hit++;
			isHit='H';
		}
		printTabr(arr[i],frames, fr, isHit, pr);
	}
	return hit;
}

int min(int *arr, int lim){
	int sm=0;
	for(int i=1;i<lim;i++)
		if(arr[i]<arr[sm]) sm=i;
	return sm;
}

int max(int *arr, int lim){
	int mx=0;
	for(int i=1;i<lim;i++)
		if(arr[i]>arr[mx]) mx=i;
	return mx;
}

int LRU(int frames, int count, int *arr, int pr){
	int *fr=(int*)calloc(frames*2,sizeof(int));
	int hit=0;
	if(pr==1) printf("\n\nLRU : frames = %d\n",frames);
	printTabh(frames, pr);
	for(int i=0; i<count; i++){
		char isHit;
		int fat = search(arr[i], fr, frames);
		if(fat<0){
			fat=min(&fr[frames],frames);
			fr[fat]=arr[i];
			isHit='M';
		}
		else{
			hit++;
			isHit='H';
		}
		fr[frames+fat]=i+1;
		printTabr(arr[i], frames, fr, isHit, pr);
	}
	return hit;
}

int OPT(int frames, int count, int *arr, int pr){
	int *fr=(int*)calloc(frames,sizeof(int));
	int hit=0;
	if(pr==1) printf("\n\nOPT : frames = %d\n",frames);
	printTabh(frames, pr);
	for(int i=0; i<count; i++){
		char isHit;
		int fat = search(arr[i], fr, frames);
		if(fat<0){
			int lastac = -1, lastacpos=-1;
			for(int j=0;j<frames;j++){
				int nextac = search(fr[j], arr+i+1, count-(i+1));
				if(nextac<0){
					lastac = j;
					break;
				}
				if(lastacpos<nextac){
					lastac = j;
					lastacpos = nextac;
				}
			}
			fr[lastac] = arr[i];
			isHit='M';
		}
		else{
			hit++;
			isHit='H';
		}
		printTabr(arr[i], frames, fr, isHit, pr);
	}
	return hit;
}

void graph(int lim, int *arr, int buff){
	int xmin=arr[min(arr, lim)], xmax=arr[max(arr, lim)];
	int ymin=arr[min(arr+lim,lim)+lim], ymax=arr[max(arr+lim,lim)+lim];
	for(int i=ymax+buff;i>=ymin-buff;i--){
		printf("%d \t|\t",i);
		for(int j = xmin-buff;j<=xmax+buff;j++){
			for(int k=0;k<lim;k++)
				if(arr[k]==j && arr[k+lim]==i){
					printf("*");
					break;
				}
			printf("\t");
		}
		printf("\n");
	}
	printf("\t");
	for(int j = xmin-buff;j<=xmax+buff;j++) printf("--------");
	printf("-\n\t\t");
	for(int j = xmin-buff;j<=xmax+buff;j++) printf("%d\t", j);
	printf("\n");
}

int main(){
	/*int ar[]={1,2,3,4,3,2,5,6,1,5};
	int fr=3, c=10;
	printf("Hits : %d\n",OPT(fr,c,ar,1));*/
	/*int arr[]={1,2,3,1,2,3};
	graph(3,arr,0);*/
	int c, fm, fs, ch=0, full, pr=1;
	printf("Enter no. of access : ");
	scanf("%d",&c);
	int *arr=(int*)calloc(c,sizeof(int));
	printf("Enter %d pages : ", c);
	for(int i=0;i<c;i++){
		//printf("Enter %d : ", i+1);
		scanf("%d",&arr[i]);
	}
	printf("Enter min and max no. of frames : ");
	scanf("%d %d", &fs, &fm);
	full = fm-fs+1;
	int *hitf=(int*)calloc(full*2,sizeof(int));
	for(int i=0;i<full;i++)
		hitf[i]=fs+i;
	while(ch!=5){
		printf("1. FIFO\n2. OPT\n3. LRU\n4. Toggle Table Print\n5. Exit\nEnter choice : ");
		scanf("%d",&ch);
		printf("\n");
		switch(ch){
			case 1:
			case 2:
			case 3:break;
			case 4:pr=(pr+1)%2;
				printf("Table will %sbe printed\n\n",(pr==1)?"":"not ");
				continue;
			case 5:exit(0);
			default:printf("\nWrong Input\n");
		}
		for(int i = 0;i < full;i++){
			switch(ch){
				case 1:hitf[full+i] = FIFO(fs+i,c,arr,pr); break;
				case 2:hitf[full+i] = OPT(fs+i,c,arr,pr); break;
				case 3:hitf[full+i] = LRU(fs+i,c,arr,pr); break;
			}
			if(pr==1) printf("Hit Ratio : %d/%d\n\n",hitf[full+i],c);
		}
		printf("\nNo. of Frames vs Hits\n");
		graph(full,hitf,0);
		printf("\n");
	}
}

//Write a C Program to implement linked list management scheme for memory partitioning Implement First fit, Best fit, Worst fit Algorithm

#include<stdio.h>
#include<stdlib.h>
#include<string.h>

struct Node{
	int pid, start, length;
	struct Node* next;
};
typedef struct Node Node;

Node* allocN(Node *head, int pid, int length){
	Node *proc=(Node *)malloc(sizeof(Node)), *hole=head->next;
	proc->pid=pid;
	proc->start=hole->start;
	proc->length=length;
	hole->start+=length;
	hole->length-=length;
	if(hole->length<=0){
		proc->next=hole->next;
		free(hole);
	}
	else proc->next=hole;
	head->next=proc;
	return proc;
}

Node* deallocN(Node *head, int pid){
	Node *pre=head, *temp=head->next, *toRet=NULL;
	while(temp!=NULL){
		if(temp->pid!=pid){
			pre=temp;
			temp=temp->next;
			continue;
		}
		temp->pid=-1;
		toRet=temp;
		if(temp->next->pid==-1){
			Node *nt=temp->next;
			temp->length+=nt->length;
			temp->next=nt->next;
			free(nt);
		}
		if(pre->pid==-1){
			pre->length+=temp->length;
			pre->next=temp->next;
			free(temp);
			toRet=pre;
		}
		return toRet;
	}
	return toRet;
}

Node* firstFit(Node *head,int val){
	Node *temp,*temp1;
	temp=head;
	temp1=temp;
	temp=temp->next;
	while(temp!=NULL){
		if(temp->pid<0){
			if(((temp->length)-val>=0)){
				return temp1;
			}
		}
		temp1=temp;
		temp=temp->next;
	}
	return NULL;
}

Node* bestFit(Node *head,int val){
	Node *temp,*temp1,*temp2=NULL;
	temp=head;
	temp1=temp;
	temp=temp->next;
	int var=-1;//variable to store lengthth of hole
	while(temp!=NULL){
		if(temp->pid<0){//hole finding
			if((temp->length)-val==0){
				return temp1;
			}
			else if((temp->length)-val>0){
		        if((var-(temp->length)>0 && var!=-1) || var==-1){
		 		 	var=temp->length;
		 		 	temp2=temp1;
				}
			}
		}
		temp1=temp;
		temp=temp->next;
	}
	return temp2;
}

Node* worstFit(Node *head,int val){
	Node *temp,*temp1,*temp2=NULL;
	temp=head;
	temp1=temp;
	temp=temp->next;
	int var=-1;//variable to store lengthth of hole
	while(temp!=NULL){
		if(temp->pid<0){//hole finding
			if((temp->length)-val>=0){
		        if((var-(temp->length)<0 && var!=-1) || var==-1){
		 		 	var=temp->length;
		 		 	temp2=temp1;
				}
			}
		}
		temp1=temp;
		temp=temp->next;
	}
	return temp2;
}

void printL(Node *head){
	Node *temp=head->next;
	while(temp!=NULL){
        if(temp->pid>=0)
            printf("PID : %d",temp->pid);
        else printf("HOLE");
		printf("\tStart : %d\tLength : %d\n",temp->start,temp->length);
		temp=temp->next;
	}
}

void printGV(Node* he) {
	Node *head=he;

    head = head->next;
    while (head != NULL) {
        if (head->pid > 0) {
            printf("%02d +++++ PID %d\n", head->start, head->pid);
            for (int i = 1; i < head->length; i++) {
                printf("   +++++\n");
            }
        }
		else{
            printf("%02d xxxxx HOLE\n", head->start);
            for (int i = 1; i < head->length; i++) {
                printf("   xxxxx\n");
            }
        }
        head = head->next;
    }
}

Node* newMem(int length){
	Node *head=(Node*)malloc(sizeof(Node)), *temp;
	head->pid=head->start=head->length=-2;

	head->next=(Node*)malloc(sizeof(Node));
	head->next->pid=-1;
	head->next->start=0;
	head->next->length=length;
	return head;
}

void freeMem(Node *head){
	if(head==NULL)
		return;
	freeMem(head->next);
	free(head);
}

int chAlgo(){
	int ch=-1;
	while(1){
		printf("1. First Fit\n2. Best Fit\n3. Worst Fit\nChoose Algo : ");
		scanf("%d",&ch);
		if(ch>=0 && ch<=3)
			return ch;
		printf("Invalid Input\n\n");
	}
}

int main(){
	int maxl, ch=-1, algo, pid=1;
	printf("Enter memory size : ");
	scanf("%d",&maxl);
	algo=chAlgo();

	Node *head=newMem(maxl);

	while(ch!=6){
		printf("\n\n1. Clear Memory\n2. Change Algo\n3. Create New Process\n4. Remove Existing Process\n5. Display Graph\n6. Exit\nEnter Choice : ");
		scanf("%d",&ch);
		switch(ch){
			case 1 :freeMem(head);
					head=newMem(maxl);
					printf("\nMemory Cleared\n\n");
					break;
			case 2 :algo=chAlgo();
					break;
			case 3 :int pLen;
					Node *change;
					printf("\nEnter process size : ");
					scanf("%d",&pLen);
					switch(algo){
						case 1 :change=firstFit(head,pLen);break;
						case 2 :change=bestFit(head,pLen);break;
						case 3 :change=worstFit(head,pLen);break;
					}
					if(change==NULL)
						printf("\nSpace not available\n\n");
					else{
						change=allocN(change,pid++,pLen);
						printf("\nProcess allocated as PID %d at %d\n\n",change->pid,change->start);
					}
					break;
			case 4 :int rpid;
					printf("\nEnter PID : ");
					scanf("%d",&rpid);
					Node *newHole=deallocN(head,rpid);
					if(newHole==NULL)
						printf("\nProcess not found\n\n");
					else
						printf("\nProcess removed\nNew hole at %d\n\n",newHole->start);
					break;
			case 5 :printf("\n------------------\n\n");
					printGV(head);
					printf("\n------------------\n\n");
                    break;
			case 6 :break;
			default:printf("\nWrong Input\n\n");
		}
	}
}
