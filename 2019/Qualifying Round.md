## Code Jam 2019 - Qualifying Round

| Problem   | Test Set 1 | Test Set 2 | Test Set 3|
|----------|:------:|:----:|:---:|
| [Foregone Solution](https://codingcompetitions.withgoogle.com/codejam/round/0000000000051705/0000000000088231) |  :white_check_mark: | :white_check_mark: | :white_check_mark: |
| [You Can Go Your Own Way](https://codingcompetitions.withgoogle.com/codejam/round/0000000000051705/00000000000881da) |   :white_check_mark: | :white_check_mark:| :white_check_mark: |
| [Cryptopangrams](https://codingcompetitions.withgoogle.com/codejam/round/0000000000051705/000000000008830b) | Not attempted | Not attempted |  |
| [Dat Bae](https://codingcompetitions.withgoogle.com/codejam/round/0000000000051705/00000000000881de) | Not attempted | Not attempted |  |

I earned 41 points from the first two solutions, meeting the minimum 40 needed to continue, and therefore did not attempt the third and fourth problem.

## Foregone Solution
The goal is here to take one number, N, and create two numbers, P and Q, such that P + Q = N. Furthermore, P and Q cannot contain the digit 4, while N contains at least one 4.
My solution is to go digit-wise through N. Let N(i) be the i-th digit of N. For each i:
If N(i) equals 4, then set P(i) = 2 and Q(i) = 2
Else set P(i) = N(i) and Q(i) = 0

Clean and simple!

~~~~
# input() reads a string with a line of input, stripping the ' ' (newline) at the end.
# This is all you need for most Code Jam problems.
t = int(input()) # read a line with a single integer
for i in range(1, t + 1):
  n = input() # the total number
  
  #get n as a string
  ns = str(n)
  
  #length of string
  l = len(ns)
  
  #first number with 4 removed
  p = 0
  
  #second number
  q = 0
  
  multiplier = 1
  for j in range(l-1,-1,-1):
    #get letter at position l-iteration
    let = int(ns[j])
    if let == 4:
      p += 2*multiplier
      q += 2*multiplier
    else:
      p += int(ns[j])*multiplier
      
    multiplier *= 10
    
  print("Case #{}: {} {}".format(i, p, q))
  ~~~~
  
  
## You Can Go Your Own Way

My solution to this problem is the most trivial one, and works because the maze is guaranteed to be a square. Since we want to make sure we never use the same move as Lydia, but it's okay to be on the same spot, we can just "mirror" her moves on a transposed grid.
If she moves E, we move S, and vice versa. It is guaranteed to work on square grids.

~~~~
# input() reads a string with a line of input, stripping the ' ' (newline) at the end.
# This is all you need for most Code Jam problems.
t = int(input()) # read a line with a single integer
for i in range(1, t + 1):
  n = int(input()) # read the length of the maze
  
  ls = input() #read Lydia's steps

  #replace all instances of E with S and vice versa
  os = ls.replace("E","-").replace("S","E").replace("-","S")
  
  print("Case #{}: {}".format(i, os))
  ~~~~
