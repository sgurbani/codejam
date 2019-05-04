## Code Jam 2019 - Round 1B

| Problem   | Test Set 1 | Test Set 2 |
|----------|:------:|:----:|
| [Manhattan Crepe Cart](https://codingcompetitions.withgoogle.com/codejam/round/0000000000051706/000000000012295c) |  :white_check_mark: | :x: |
| [Draupnir](https://codingcompetitions.withgoogle.com/codejam/round/0000000000051706/0000000000122837) |   :x: | :x:|
| [Fair Fight](https://codingcompetitions.withgoogle.com/codejam/round/0000000000051706/0000000000122838) | :white_check_mark: | :x: |

I did not make it into the top 1500 and thus did not advance to the next round in this attempt.


### Manhattan Crepe Cart
I used a brute-force method here looking at all possible grid squares. As a result, while it worked fine for Test Set 1, generating the grid in Test Set 2 yielded a Memory Limit Exceeded error.
~~~~
import numpy as np
# input() reads a string with a line of input, stripping the ' ' (newline) at the end.
# This is all you need for most Code Jam problems.
t = int(input()) # read a line with a single integer
for i in range(1, t + 1):
  P,Q = [int(s) for s in input().split(" ")] # read a list of integers, 2 in this case

  # create a Q+1xQ+1 matrix representing the grid
  grid = np.zeros((Q+1,Q+1), dtype=np.int)
  
  # go through the P people and figure out where they are
  for p in range(0,P):
    x,y,d = [s for s in input().split(" ")] 
    x = int(x)
    y = int(y)
    d = d.lower()
    
    #depending on direction, update the grid with possible locations for the cart
    if d == 'n':
      grid[:,y+1:Q+1] += 1
    
    if d == 's':
      grid[:,0:y] += 1
      
    if d == 'e':
      grid[x+1:Q+1,:] += 1
    
    if d == 'w':
      grid[0:x,:] += 1
      
    
  x,y = np.unravel_index(grid.argmax(), grid.shape)
  print("Case #{}: {} {}".format(i, x, y))
~~~~


## Draupnir
My attempt involved only trying this problem with the constraints of Test Set 1, e.g. we are allowed 6 queries. I set up a set of 6 linear equations to solve based on the number of rings on days 2-7, and solve them using numpy.
However, this solution resulted in a **Time Limit Error** even for Test Set 1.
~~~~
import numpy as np
import sys

def queryWell(D):
  print(int(D))
  sys.stdout.flush()
  s = input()
  if s == '-1':
    sys.exit(0)
  return s

t,W = [int(s) for s in input().split(" ")] # read a list of integers, 2 in this case
for i in range(1, t + 1):
    # prepare coeff matrix for 6 time points
    A = np.array( [[128,8,4,2,2,2], [4,2,1,1,1,1], [8,2,2,1,1,1], [16,4,2,2,1,1], [32,4,2,2,2,1], [64,8,4,2,2,2]])
    
    # create empty array for ring totals
    b = np.zeros((6))
    b = b.transpose()
    
    # query the well W times
    for w in range(2,W+1):
      b[w-1] = queryWell(w)
    b[0] = queryWell(7)
    
    # solve equation
    x = np.matmul(np.linalg.inv(A),b).astype(int)
    xstr = " ".join(map(str, x))
    
    print(xstr)
    sys.stdout.flush()
    resp = input()
    
    if resp == '-1':
      sys.exit(0)
~~~~

## Fair Fight
I used a brute-force method to check every possible combination, which worked well enough for Test Set 1, but yielded a **Time Limit Error** for Test Set 2.

~~~~
import numpy as np
# input() reads a string with a line of input, stripping the ' ' (newline) at the end.
# This is all you need for most Code Jam problems.
t = int(input()) # read a line with a single integer
for i in range(1, t + 1):
  N,K = [int(s) for s in input().split(" ")] # read a list of integers, 2 in this case

  # read the C and D arrays
  C = [int(s) for s in input().split(" ")]
  D = [int(s) for s in input().split(" ")]

  # number of fair fights
  num_fair = 0
  
  # go through every possible L,R combination
  for L in range(0,N):
    for R in range(L,N):
      
      # get the L:R elements of C and D
      c = C[L:R+1]
      d = D[L:R+1]
      
      # find the diff between the max of each
      if abs(np.amax(c) - np.amax(d)) <= K:
        num_fair += 1
        
  print("Case #{}: {}".format(i, num_fair))
  ~~~~
