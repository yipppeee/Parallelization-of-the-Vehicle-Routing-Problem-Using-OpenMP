
## Requirements  

- Should work on every Linux Distribution. Tested on Ubuntu 22.04.
- For seqMDS (sequential version): GCC 7+. Tested on GCC 9.3.1
- For parMDS (parallel OpenMP): We use `nvc++` compiler from [NVIDIA's HPC SDK](https://developer.nvidia.com/hpc-sdk).
-For opt_PParMDS :We use `nvc++` compiler from [NVIDIA's HPC SDK](https://developer.nvidia.com/hpc-sdk) , or `g++` als soworks

## How to run

### Build and run the executables
```
## To compile. Runs toy example on parMDS & seqMDS 


## To build and run only seqMDS.
all: parMDS seqMDS

parMDS: parMDS.cpp
	nvc++ -O3 -std=c++14 -acc=multicore  parMDS.cpp -o parMDS.out && ./parMDS.out toy.vrp -nthreads 20 -round 1
	
seqMDS: seqMDS.cpp
	g++ -O3 -std=c++14 seqMDS.cpp -o seqMDS.out && ./seqMDS.out toy.vrp
parMDS: opt_parMDS.cpp
	nvc++ -O3 -std=c++14 -acc=multicore  opt_parMDS.cpp -o opt_parMDS.out && ./opt_parMDS.out toy.vrp -nthreads 20 -round 1
	

clean:
	rm -f *.out

cleanFolders:
	rm -rf outinputs*


## To run the executable

./seqMDS.out toy.vrp [-round 0 or 1 DEFAULT:1 means round it!]
./parMDS.out toy.vrp [-nthreads <n> DEFAULT is 20] [-round 0 or 1 DEFAULT:1]
./_opt_parMDS.out toy.vrp [-nthreads <n> DEFAULT is 20] [-round 0 or 1 DEFAULT:1]


## An example
./parMDS.out inputs/Antwerp1.vrp -nthreads 16 -round 1
./seqMDS.out toy.vrp -round 0
./opt_parMDS.out inputs/Antwerp1.vrp -nthreads 16 -round 1


## Output Description

//One line stats printed on std err whereas the solution on std out
inputs/Antwerp1.vrp Cost 556105 548740 518042 Time(seconds) 0.599911 2.18137 2.19818 parLimit 16 VALID    
Route #1: ..
Route #2: ..
...
Route #k:
Cost <val>

```

### Build and run the paper's artifact

```
bash ./runRounds.sh
```


- Runs both seqMDS and parMDS on all 130 inputs using round conventions.
- Create .sol files for each instance.
- Create a folder `output*` and place all .log and .sol files.
- Create a time.txt file which contains the Cost and Time of all instances.
- In addition to it, Cost and Time are printed on the terminal as well.


