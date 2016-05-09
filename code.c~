 /**
 * Problem     : Paralle Assignment#5  Basic Linear Algebra Subroutine
 * Author      : Almgwary
 * Date        : 4-May-2016
 * Thin        : 
 **/

#include <stdio.h>
#include <mpi.h>
#include <time.h>
#include <math.h>


 
void arr_alloc(int** arr, int r,int c){*arr= malloc(r*c*sizeof(int));}

void arr_init(int* arr, int r, int c, int init){

   int i=0;
    for(i=0;i<r;++i){
        int j=0;
        for(j=0;j<c;++j){

            arr[i*c+j]=init;
        }
    }
}

void printMatrix(int* arr,int r,int c){
    int i = 0;
    for(i=0;i<r;++i){
        int j=0;
        for(j=0;j<c;++j){
            printf("%d, ",arr[i*c+j]);
        }
        printf("\n");
    }
}

void arr_dealloc(int** arr){free(*arr);}

void arr_input(int* arr, int r, int c){
	int i=0;
	for (i = 0; i < r; ++i) {
		int j;
		for (j = 0; j < c; ++j) {
			scanf("%d",&arr[i*c+j]);
		}
	}
}

void arr_file_input(FILE *file,int* arr, int r, int c){
	int i=0;
	for (i = 0; i < r; ++i) {
		int j;
		for (j = 0; j < c; ++j) {
			fscanf(file,"%d",&arr[i*c+j]);
		}
	}
}

void arr_alloc_input(int** arr_addres, int r,int c){
	 	//allocation
	    arr_alloc(arr_addres,r,c);
	    //input
	    arr_input(*arr_addres,r,c);

}

void arr_alloc_file_input(FILE *file,int** arr_addres, int r,int c){
	 	//allocation
	    arr_alloc(arr_addres,r,c);
	    //input
	    arr_file_input(file,*arr_addres,r,c);

}

void arr_alloc_intit(int** arr_addres, int r, int c, int init){
	 	//allocation
	    arr_alloc(arr_addres,r,c);
	    //initialize
	    arr_init(*arr_addres,r,c,init);
}

int * matrix_multiblication(int* A, int* B, int A_r, int A_c, int B_r, int B_c){
	int *C;
	int C_r= A_r,
		C_c= B_c;

	//allocat C
	arr_alloc(&C,C_r,C_c);
	//multiblaication operation
	int i=0;
	for (i = 0; i < C_r; ++i) {
		int j=0;
		for (j = 0; j < C_c; ++j) {
			int k=0,sum=0;
			for (k = 0; k < A_c; ++k) {
				int A_cell =A[ i * A_c + k ],
					B_cell =B[ k * B_r + j ];
				sum+=( A_cell * B_cell );
			}
			//store result
			C[i*C_c+j]=sum;
		}
	}
	//printMatrix(C,C_r,C_c);
	return C;
}               

int main(int argc, char  *argv[])
{
 	int my_rank;
 	int size ;
 	int tag;
 	int source;
 	int dest ;
 	MPI_Status status;


 	MPI_Init(&argc,&argv);
 	MPI_Comm_size(MPI_COMM_WORLD, &size);
 	MPI_Comm_rank(MPI_COMM_WORLD , &my_rank);
 	 
 	int input = 0 ;
 	int A_r = 0, A_c = 0,
		B_r = 0, B_c = 0;
	int *A,*B,*C;
 	 

 	
 	if(my_rank==0){
 		printf("Welcome to vector Matrix multiplication program!\n");
		FILE *file= fopen("test.txt","r+"); //readFromFile
		if(file!=NULL){
			fscanf(file,"%d%d",&A_r,&A_c);
			fscanf(file,"%d%d",&B_r,&B_c);
			// check dimnsions of matrix is divisable by # of process
			if (A_c%size != 0)
		 	{
		 		printf("# of MPI tasks Must Divisble by Matrix dimentions. Quitting...\n");
				MPI_Abort(MPI_COMM_WORLD, 0);
				exit(1);
		 	}
		 	// check that  A  [n*n], B  [n*1]
			if(A_c==B_r && A_c==A_r && B_c == 1){
				arr_alloc_file_input(file,&A,A_r,A_c);
				arr_alloc_file_input(file,&B,B_r,B_c);
				arr_alloc(&C,A_r,B_c);			 
			}else{
				printf("not Valid Matrix dimentions");
				MPI_Abort(MPI_COMM_WORLD, 0);
				exit(1);
			}
			fclose(file);
		}else{printf("unable to open file");}
 		printf("------ A ---------\n");
		printMatrix(A , A_r ,A_c);
		printf("------ B ---------\n");
		printMatrix(B , B_r ,B_c);
 
 
 	}
 	 
 	/* Bcast dimentions*/
 	MPI_Bcast (&A_c, 1, MPI_INT, 0, MPI_COMM_WORLD);
 	MPI_Bcast (&A_r, 1, MPI_INT, 0, MPI_COMM_WORLD);
 	MPI_Bcast (&B_c, 1, MPI_INT, 0, MPI_COMM_WORLD);
 	MPI_Bcast (&B_r, 1, MPI_INT, 0, MPI_COMM_WORLD);
 	// allocations
 	if (my_rank!=0)
 	{
 		arr_alloc(&A,A_r,A_c);
 		arr_alloc(&B,B_r,B_c);
 		arr_alloc(&C,A_r,B_c);
 	}
 	// n is dimention 
 	int n = A_r ;
   
   /**
 	* Scatter A and B to smallA and smallB
 	* 
 	* smallA.size = nRows*n
 	* nRows = n/size ;
 	* 
 	* smallB.size = n/size ;
 	**/

 	//allocate
 	int *smallA , *smallB;
 	int nRows = n/size ;
 	int smallA_size = nRows*n;
 	int smallB_size = n/size ;

 	arr_alloc(&smallA, smallA_size, 1);
 	arr_alloc(&smallB, smallB_size, 1);
 	
 	//Scatter A
 	MPI_Scatter(A,smallA_size,MPI_INT,smallA,smallA_size,MPI_INT,0,MPI_COMM_WORLD);
 	//Scatter B
 	MPI_Scatter(B,smallB_size,MPI_INT,smallB,smallB_size,MPI_INT,0,MPI_COMM_WORLD);
 	

 	// printf("---------P%d------------\n",my_rank);
 	// printMatrix(smallA, 1, smallA_size);
 	// printMatrix(smallB, 1, smallB_size);

   /**
   	* make each process Bcast its smallB then all process do corresponing calculations
   	* prebare matrix smallB_Cast to bCast the smallB
   	* copy matrix memcpy(smallB_Cast, smallB, smallB_size * sizeof(int));
   	* prebare matrix smallC to store the result
   	* smallC.size  = smallB.size = n/size ;  
   	*
   	**/

   	// allocation
	int *smallB_Cast ,*smallC ,smallC_size=n/size;
	
	arr_alloc(&smallB_Cast, smallB_size, 1);
	arr_alloc(&smallC, smallC_size, 1);
	arr_init(smallC, smallC_size, 1, 0);

	// each process casting smallB
	int i = 0 ;
	for ( i = 0; i < size; ++i)
	{
		// prepare my smallB_Cast from smallB
		if (my_rank == i )
		{
		  memcpy(smallB_Cast, smallB, smallB_size * sizeof(int));
		  printf("P%d sending smallB\n", my_rank);

		} 
		
		// casting from root i
		MPI_Bcast (smallB_Cast , smallB_size , MPI_INT , i , MPI_COMM_WORLD);
		 
	   /**
	    *  do  work and store result
	    *  for each row in smallA 
	    *      r0 start from piont x =[i*smallB_size]
	    *      r1 start from piont x =[i*smallB_size+n]
	    *      rj start from piont x =[i*smallB_size+(n*j)]
	    *      go from  x = start point to < (x+smallB_size)
	    *      multibly and sum in smallC[CurrentRow]
		**/

	    int j = 0 ; 
		for ( j ; j < nRows; ++j) // for each row in smallA
		{
			//   
			int x = i*smallB_size+(n*j) , x_init=i*smallB_size+(n*j) ;
			int b = 0 ; // iterat over smallB_Cast
			for ( x ; x < (x_init+smallB_size); ++x)// for each cell in row
			{
			 	smallC[j]+= (smallA[x]*smallB_Cast[b]);
			 	b++;
			}
		}
		 
		// collect smallC
 		MPI_Gather(smallC , smallC_size , MPI_INT , C , smallC_size , MPI_INT , 0 , MPI_COMM_WORLD);
	}

	if (my_rank==0)
 	{
 		printf("------ Final C -------\n");
		printMatrix( C, A_r,B_c); 
 	} 


	MPI_Finalize();
 	return 0;

 
// 	MPI_Gather(subC ,nRow*B_c ,MPI_INT , C , nRow*B_c, MPI_INT, 0, MPI_COMM_WORLD  );




 	 
 	 
 	
 }
 	 
 