# Parallel-MPI-Basic-Linear-Algebra-Subroutine
solve system of linear equation with parallel MPI just one scatter and one allGather with minimum communcation


![Alt text](/screenShot.PNG?raw=true "")

# Issue
 1. not implemnet matrix inverse 


# Formla
   --

#### link : http://mathforum.org/library/drmath/view/55482.html

     
     a +  b - 2c +  d + 3e -  f  =    4
    2a -  b +  c + 2d +  e - 3f  =   20
     a + 3b - 3c -  d + 2e +  f  =  -15
    5a + 2b -  c -  d + 2e +  f  =  - 3
    -3a -  b + 2c + 3d +  e + 3f  =   16
    4a + 3b +  c - 6d - 3e - 2f  =  -27
    
    
## matrix A
 
   1, 1, -2, 1, 3, -1  
   2, -1, 1, 2, 1, -3  
   1, 3, -3, -1, 2, 1  
   5, 2, -1, -1, 2, 1  
   -3, 1, 2, 3, 1, 3  
   4, 3, 1, -6, -3, -2  
    

## matrix C

 

    4
    20  
    -15
    -3
    16 
    -27
 

# Result
## matrix B
 

    a =  1  
    b = -2  
    c =  3  
    d =  4  
    e =  2  
    f = -1  
    
 


###ResultOf Multiblication without invers
 

    126,   
    64,   
    117,   
    83,   
    -96,   
    85,   
 

##  Inverse of A
 

     -28/71	31/142	14/71	13/71	-5/71	-13/142  
     -103/284	109/284	79/142	-23/142	45/284	23/284  
     345/568	-139/568	-175/284	15/284	125/568	127/568  
     -537/568	367/568	223/284	-11/284	3/568	-131/568  
     713/568	-363/568	-267/284	31/284	69/568	111/568  
     -43/284	-51/284	-7/142	29/142	5/284	-29/284  
 



