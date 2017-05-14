# CSSoftware

#include<mpi.h>
#include<iostream>
#include<math.h>
#include<fstream>
#define MAXSIZE 1000
#define BLOCK_LOW(id,p,n) ((id)*(n)/(p))
#define BLOCK_HIGH(id,p,n)(BLOCK_LOW((id)+1,p,n)-1)
#define BLOCK_SIZE(id,p,n)(BLOCK_HIGH(id,p,n)-BLOCK_LOW(id,p,n))
using namespace std;

/*double dx_arctan(double x)
{
return (1.0 / (1.0 + x*x));
}*/

double dx_arctan(double x)
{
	return (1.0/(1+x*x));
}

void para_range(int n1,int n2,int numprocs,int irank,int*ista,int*iend)
{
	int iwork1=(n2-n1+1)/numprocs;
	int iwork2=(n2-n1+1)%numprocs;
	*ista=irank*iwork1+n1+min(irank,iwork2);
	*iend=*ista+iwork1-1;
	if(iwork2>irank)
		*iend+=1;
}
int main(int argc,char**argv)
{

	//Slide_3_40
	/*int world_size,world_rank;
	MPI_Init(NULL,NULL);
	MPI_Comm_size(MPI_COMM_WORLD,&world_size);
	MPI_Comm_rank(MPI_COMM_WORLD,&world_rank);
	char processor_name[MPI_MAX_PROCESSOR_NAME];
	int name_len;
	MPI_Get_processor_name(processor_name,&name_len);
	printf("Hello world from processor %s, ranks %d out of %d processors\n",
	processor_name,world_rank,world_size);
	int version,sub;
	MPI_Get_version(&version,&sub);
	cout<<endl<<"Version "<<version<<" ,Subversion "<<sub<<endl;
	MPI_Finalize();
	return 0;
	*/

	/* Slide 3_41
	int rank, size;
	MPI_Init(&argc, &argv);
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	MPI_Comm_size(MPI_COMM_WORLD, &size);
	cout <<"Hello World from process"<<rank<<" of " 
	<<size<<endl;
	MPI_Finalize();
	return 0;
	*/

	//Slide 3_49
	/*
	int numtasks,rank,len,rc;
	char hostname[MPI_MAX_PROCESSOR_NAME];
	rc=MPI_Init(&argc,&argv);
	if(rc!=MPI_SUCCESS)
	{
	cout<<"Error starting MPI program. Termentating.\n";
	MPI_Abort(MPI_COMM_WORLD,rc);
	}
	MPI_Comm_size(MPI_COMM_WORLD,&numtasks);
	MPI_Comm_rank(MPI_COMM_WORLD,&rank);
	MPI_Get_processor_name(hostname,&len);
	cout<<"Number of tasks = "<<numtasks<<endl<<"My rank is "<<rank<<endl<<" Running on "<<hostname<<endl;

	MPI_Finalize();
	return 0;*/

	//Slide 3_55
	/*int rank, nprocs;
	MPI_Init(&argc,&argv);
	MPI_Comm_size(MPI_COMM_WORLD,&nprocs);
	MPI_Comm_rank(MPI_COMM_WORLD,&rank);
	MPI_Barrier(MPI_COMM_WORLD);
	printf("Hello, world. I am %d of %d\n", rank,nprocs);fflush(stdout);
	MPI_Finalize();
	return 0;
	*/

	//Slide 3_74 [Calculate PI]

	/*
	int rank, numprocs, n;
	double mypi, pi, w;
	MPI_Init(NULL, NULL);
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	MPI_Comm_size(MPI_COMM_WORLD, &numprocs);
	if (rank==0)
	{
	cout << "enter number of segments \n";
	cin >> n;
	}
	MPI_Bcast(&n, 1, MPI_INT, 0, MPI_COMM_WORLD);
	w = 1.0/n;
	mypi = 0.0;
	for (int i = rank+.5; i < n; i+=numprocs)
	{
	mypi += dx_arctan(i*w);
	}
	mypi = mypi*w * 4;
	MPI_Reduce(&mypi, &pi, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);
	if (rank==0)
	{
	cout << "PI=" << pi;
	}
	MPI_Finalize();
	return 0;*/

	/*Slide 74 Again()
	int rank,numprocs,n;
	double w,myPi,Pi;
	MPI_Init(NULL,NULL);
	MPI_Comm_size(MPI_COMM_WORLD,&numprocs);
	MPI_Comm_rank(MPI_COMM_WORLD,&rank);

	if(rank==0)
	{
	cout<<"Enter No. Of Segements"<<endl;
	cin>>n;

	}
	MPI_Bcast(&n,1,MPI_INT,0,MPI_COMM_WORLD);
	myPi=0.0;
	w=1.0/n;
	for(int i=rank+0.5;i<n;i+=numprocs)
	{
	myPi+=dx_arctan(i*w);
	}
	myPi=4*myPi*w;
	MPI_Reduce(&myPi,&Pi,1,MPI_DOUBLE,MPI_SUM,0,MPI_COMM_WORLD);
	if(rank==0)
	{
	cout<<"PI = "<<Pi<<endl;
	}
	MPI_Finalize();*/


	/*
	// Slide 3_74 Factorial
	int rank,numprocs,N,i,lower,upper;
	double local_result=1.0,total;
	MPI_Init(NULL,NULL);
	MPI_Comm_rank(MPI_COMM_WORLD,&rank);
	MPI_Comm_size(MPI_COMM_WORLD,&numprocs);
	if(rank==0)
	{
	cout<<"Enter a number"<<endl;
	cin>>N;
	}
	MPI_Bcast(&N,1,MPI_INT,0,MPI_COMM_WORLD);
	para_range(1,N,numprocs,rank,&lower,&upper);
	cout<<"Lower: "<<lower<<" ,Upper:"<<upper<<" ,Rank:"<<rank<<endl;
	for(int i=lower;i<=upper;i++)
	local_result*=(double)i;
	MPI_Reduce(&local_result,&total,1,MPI_DOUBLE,MPI_PROD,0,MPI_COMM_WORLD);
	if(rank==0)
	{
	cout<<"Factoraial of "<<N<<" is "<<total<<endl;
	cout<<"Calculated using "<<numprocs<<" processes"<<endl;
	}
	MPI_Finalize();
	return 0;
	*/

MPI_Init(NULL,NULL);
double d1=MPI_Wtime();
d1=MPI_Wtime()-d1;

cout<<MPI_Wtick()<<endl;
MPI_Finalize();

return 0;

	//Slide 3_76

}
