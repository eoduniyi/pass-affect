# Statistics & Probability Using Python

## Introduction

### Why python for Statistics?

What is **probability** - *What is the chance of an event (outcome) happening*

* To calculate the chance (probability) of an event happening
  * *Consider* all the other events that can occur at that point of time
* Sample space - *The set of all possible events that can happen*
* P(A) - *# of times A occurs* / Sample space
* **From probability we can predict** 

```python
import random

# Simulate 10 coin toses
def coin_trial():
  heads = 0 for i in range(100):
    if random.random() <= 0.5
    	heads = heads + 1
  return heads

def simulae(n):
  trials = []
  for i in range(n):
    trails.append(coin_trial())
    return(sum(trials)/n)
```

## Basics of probability and statistics

*Example:*
```python
sum = sum(p)
avg = sum/len()
print('The sum of the random numbers is: ', sum)
print('The avg of the random numbers is: ', avg)
```

*Example: Calculating probablity*
```python
event = 1
ss = 2
prob_event = event/ss
print(round(prov_event, 2))
```

*Example: Functions of probabilities*
```python
def event_probability(event_outcomes, sample_space):
  probability = (event_outcomes/sample_space) * 100
  return round(probability, 1)

ss = 2
event = 1
probability = event_probability(event, ss)
print(str(probability) + '%')
```

## Belief Propagation

* statistical graphical models
* probabilistic graphical models
* statistical and probabilistic models
  * **Bayesian** networks
  * Markov **random fields**
  * **Factor** graphs

`Markov -> networks -> graphs`

`Markov -> graphs`

`chain -> graphs`

`probabilistic sequential graphs `

`Beysian -> random fields -> factor`

**Beliefs** - approximately computed marginal probabilities:

```python
Approx(p(x_1, ..., x_n)) 
```

----
# Parallel Programming in Python

## What is parallel programming?

* **parrallel programming** - Style of programming concerned with software that is run on workstations, computers, cores, etc.
* Requires the decomposition of problems
  * Divide and conquer strategy
    * Problem is divided across multiple workstations, computers, cores, etc.
* Concerned with computations carried across shared and distributed memories (workspaces)

### What is MPI?

* **Message-Passing-Interface** - Protocol for distributed computing (i.e., high performance computing)

  * **Common MPI functions:**
    * `Gather` (many-to-one): Bring others into me
    * `Scatter` (one-to-many): `Divide` me into others
    * `Reduction` (many-to-one): `Aggregate`, `Collapse`, `Collect`, Form me from others.
    * `Broadcast` (one-to-many): `Clone` me to others.
  * **Requirements:**
    * Write code for each communication that occurs between nodes: workstations, computers, cores, etc.
  * **Pros**
    * Single large computations across similar machines across a reliable network: computer lab, computer clusters
  * **Cons:**
    * Does not handle distributed applications well
      * MPI has trouble with clients continually connecting and disconnecting to the network

  #### MPI in `python` (mpi4py)

  **Communicators and Ranks**
  ```python
  from mpi4py import MPI
  
  comm = MPI.COMM_WORLD
  rank = comm.Get_rank()
  print('My rank is ', rank)
  ```

  **Point-to-point Communication**

  *Example A*
  ```python
  from mi4py import MPI
  import numpy
  
  # Infrasturcture of MPI
  comm = MPI.COMM_WORLD # Need to define a communicator
  rank = comm.Get_rank() # Need to always be aware of the rank of the process it's on 
  
  # If this program is running on processor with rank 0
  if rank == 0:
    data = {'a': 7, 'b': 3.14}
    comm.send(data, dest=1) # Send information to processor (rank, node) 1 
  elif rank == 1: # If this program is running on processor with rank 1
    data = comm.recv(source=0) # Recieve information to processor (rank, node) 0 
    print('On process 1, data is ', data)
  
  ```

  *Example B*
  ```python
  from mi4py import MPI
  import numpy
  
  comm = MPI.COMM_WORLD
  rank = comm.Get_rank()
  
  # Send msg [int] to 1 if 0
  if rank == 0:
    	comm.send(1, dest=1)
  elif rank == 1: # Rec msg [int] to 0 if 1
    	idata = comm.recv(source=0)
      comm.recv(1, rank=0)
      print('On process 1, data is ', idata)
  ```

  *Example C*
  ```python
  from mi4py import MPI
  import numpy
  
  comm = MPI.COMM_WORLD
  rank = comm.Get_rank()
  
  # Send msg [int] to 1 if 0
  if rank == 0:
      comm.send(0, dest=1)
  elif rank == 1: # Send msg [int] to 0 if 1
    	comm.send(1, dest=0)
      print('On process 1, data is ', idata)
  ```

  *Example D*
  ```python
  from mi4py import MPI
  import numpy
  
  comm = MPI.COMM_WORLD
  rank = comm.Get_rank()
  
  # Send msg [int] to 1 if 0
  if rank == 0:
    	idata = comm.recv(source=1)
      comm.send(idata, dest=1)
  elif rank == 1: # Send msg [int] to 0 if 1
    	idata = comm.recv(source=0)
    	comm.send(idata, dest=0)
      print('On process 1, data is ', idata)
  ```

  *Example E*
  ```python
  from mi4py import MPI
  import numpy as np
  
  comm = MPI.COMM_WORLD
  rank = comm.Get_rank()
  
  if rank == 0:
    # Read in data parameters from a file
    numData = 10
    comm.send(numData, dest=1)
    
    data = np.linspace(0.0, 3.14, numData)
    comm.Send(data, dest=1)
    
  elif rank == 1: 
    numData = comm.recv(source=0)
    print('Number of data to recieve: ', numData)
    
    data = np.empty(numData, dtype='d') # allocate space to receive the array
    comm.Recv(data, source=0)
    
    print('data recieved: ',data)
  ```

  **Note:** There is some importance to the difference between lower and uppercase functions

  *Example F*: Broadcasting:
  `Broadcast` (one-to-many): `Clone` me to others.
  ```python
  import mpi4py as MPI
  import numpy as np
  
  comm = MPI.COMM_WORLD() # environment
  rank = comm.Get_rank() # give node names
  
  # 2 nodes
  if rank == 0:
    d = 0
    comm.send(d, dest=1)
    print('Sent ', d, 'to node: ', 1)
  
  elif rank == 1:
    d = recv(source=0)
    print(d)
    
  ```

  ```python
  from mpi4py import MPI
  
  comm = MPI.COMM_WORLD()
  rank = comm.Get_rank()
  
  if rank == 0
  	data = {'key1': [1, 2, 3],
            'key2': ('abc', 'xyz')}
  else 
  	data = None
    
  data = comm.bcast(data, root=0)
  print('Rank: ', rank, ', data: ', data)
  ```

  ```python
  from mpi4py import MPI
  import numpy as np
  
  comm = MPI.COMM_WORLD
  rank = comm.Get_rank()
  
  if rank == 0:
    # create a data array on process 0
    # in real code, this section might
    # read in data params from a file
    numData = 10
    data = np.linespace(0., 3.14, numData)
  else: 
    numdata = None
    
  # Broadcast numData and aallocate array on other ranks>
  numData = comm.bcast(numData, root=0)
  if rank != 0
  	data = np.empty(numData, dtype='d')
    
  comm.Bcast(data, root=0) # broadcast the array from rank 0 to all other ranks
  
  print('Rank: ', rank, ' , data rceieved ', data)
  ```

  *Example: Scatter*:
  (one-to-many): `Divide` me into others*
  ```python
  from mpi4py import MPI
  import numpy as np
  
  comm = MPI.COMM_WORLD()
  size = comm.Get_size()
  rank = comm.Get_rank()
  
  numDataPerRank = 10
  data = None
  if rank == 0:
    	data = np.linspace(1, size*numDataPerRank, numDataPerRank*size)
      # when size=4 (using -n 4), data = [1.0:40.0]
  
  recvbuf = np.empty(numDataPerRank, dtype ='d') # allocate space for recvbuf
  comm.Scatter(data, recvbuf, root=0)
  
  print('Rank: ', rank, ' , recvbuf recieved: ', recvbuf)
  ```

  *Example: Gathering*:
  (many-to-one): Bring others into me
  ```python
  import mpi4py as MPI
  import numpy as np
  
  comm = MPI.COMM_WORLD()
  size = comm.Get_size()
  rank = comm.Get_rank()
  
  numDataPerRank = 10
  sendbuff = np.linespace(rank*numDataPerRank+1, (rank+1)*numDataPerRank, numDataPerRank)
  print('Rank: ', rank, ', sendbuf: ', sendbuf)
  
  recvbuf = None
  if rank == 0:
    # Collect input from others
    recvbuf = np.empty(numDataPerRank*size, dtype='d')
  
  # Gather
  comm.Gather(sendbuf, recvbuf, root=0)
   
  if rank == 0:
    print('Rank: ', rank ', recvbuf recieved:', recvbuf)
  ```

  *Example: Reduction:*
  (many-to-one): `Aggregate`, `Collapse`, `Collect`, Form me from others.
  ```python
  import mpi4py as MPI
  import numpy as np
  
  comm = MPI.COMM_WORLD()
  rank = comm.Get_rank()
  
  # Create some np arrays on each process:
  # For this dem....o, the arrays have only one
  # entry that is assigned to be the rank of the processor
  value = np.array(rank, 'd')
  
  print(' Rank: ', rank, ' value = ', value)
  
  # Initialize the np arrays that will store the results:
  value_sum = np.array(0.0, 'd') 
  value_max = np.array(0.0, 'd')
  
  # Perform the reductions:
  comm.Reduce(value, value_sum, op=MPI.SUM, root=0)
  comm.Reduce(value, value_max, op=MPI.MAX, root=0)
  
  if rank == 0:
    print(' Rank 0: value_sum ', value_sum)
    print(' Rank 0: value_max ', value_max)
  ```

  *Example: Light-Gather*
  ```python
  # fast-light-gather  
  recvbuf = None
  if rank == 0:
    recvbuf = np.empty([size, len(piece)], dtype='f8')
  comm.Gather(piece   , recvbuf, root=0)
  if rank == 0:
    print(recvbuf)

  # slow-light-gather  
  recvbuf = comm.gather(pieces)
  if rank == 0:
    print(recvbuf)
  ```

**Question for Reader:**
How does parallel programming relate to belief propagation?

<i>Best,
Erick@iig</i>



