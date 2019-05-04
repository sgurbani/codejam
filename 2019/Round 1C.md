## Code Jam 2019 - Round 1C

| Problem   |   Test Set 1 | Test Set 2 |
|----------|:-----:|:----:|
| [Robot Programming Strategy](https://codingcompetitions.withgoogle.com/codejam/round/00000000000516b9/0000000000134c90) |  :white_check_mark: | :white_check_mark: |
| [Power Arrangers](https://codingcompetitions.withgoogle.com/codejam/round/00000000000516b9/0000000000134e91) |    :white_check_mark: | :white_check_mark:|
| [Bacterial Tactics](https://codingcompetitions.withgoogle.com/codejam/round/00000000000516b9/0000000000134cdf) | Not attempted | Not attempted |

## Robot Programming Strategy

For this problem, my solution involves building up a string of plays such that we can beat or tie every other opponent in that round, if possible. The key is to realize that if at any round, opponents play all three possible moves (Rock, Paper, and Scissors), then there is no way we can win and the tournament is therefore impossible to win. Otherwise, if only one or two possible moves are present, we pick the best one - e.g., if Rock and Paper are present, we pick Paper since that will beat some opponents and tie with others. After selecting our move, we then eliminate all opponents we beat, and continue with the ones we tied. Since opponents' moves will cycle once they finish their sequence, we use i % sequence length for each of them. However, for us, because there are a finite number of opponents and this finite number is less than the max possible sequence length, we never need to cycle our sequence.

~~~~
import numpy as np

def findString(strings):
  
  i = 0
  winning = ''
  
  for i in range(0,500):
    # get the i'th modded element of each string
    ith = []
    #print(strings)
    for j in range(0, len(strings)):
      ith.append(strings[j][i%len(strings[j])])
     
    rem = set(ith)
    #print(rem)
    if len(set(ith)) == 3: #cannot be done
      return 'IMPOSSIBLE'
    
    # figure out the two or one
    if 'R' in rem and 'P' in rem:
      winning = winning + 'P'
    elif 'P' in rem and 'S' in rem:
      winning = winning + 'S'
    elif 'R' in rem and 'S' in rem:
      winning = winning + 'R'
    elif 'R' in rem:
      winning = winning + 'P'
    elif 'S' in rem:
      winning = winning + 'R'
    elif 'P' in rem:
      winning = winning + 'S'
    #print(winning)
    
    # now figure out which ones would have won, and not just tied
    inds_to_remove = []
    for j in range(0, len(strings)):
      if winning[i] == 'R' and strings[j][i%len(strings[j])] == 'S':
        inds_to_remove.append(j)
      elif winning[i] == 'S' and strings[j][i%len(strings[j])] == 'P':
        inds_to_remove.append(j)
      elif winning[i] == 'P' and strings[j][i%len(strings[j])] == 'R':
        inds_to_remove.append(j)
        
    strings = np.delete(strings, inds_to_remove)
    
    if len(strings) == 0:
      return winning
    
    # increment i
    i = i+1

t = int(input()) # read a line with a single integer
for i in range(1, t + 1):
  A = int(input())
  
  # read in the strings for each opponent
  opp_strings = []
  
  for a in range(0,A):
    str = input()
    opp_strings.append(str)
  
  # go through each opponent and find which of our permutation strings win
  val = findString(opp_strings)
  
  print("Case #{}: {}".format(i, val))
~~~~


## Power Arrangers

For the interactive problem in this round, I used @Mushinako's [interactive problem helper functions](https://github.com/Mushinako/Google-Code-Jam-2019/blob/master/03-1B/02-Draupnir/solution-PP.py) for ensuring clean buffer input/output to the judge, while using the error buffer for debugging.

My solution uses the more constrained requirements for both Test cases. First, we query the first figure in each of the 119 sets. This lets us figure out what the first figure in the missing set is (because it will have only 23 occurences in the 119, whereas all the others will have 24). Then, we only have to query those 23 in order to see what's missing. Using the same strategy, we find the second figure in the missing set (it has 5 occurences, others have 6), and so on.

We make a total of 119 + 23 + 5 + 1 = 148 queries and are able to identify the missing set.

~~~~
#!/usr/bin/env python3
import numpy as np
import sys



def main():
    t, _ = [int(x) for x in input().split()]
    for _ in range(t):
        powerarrangers()


def powerarrangers():
    final_ans = ''
    letters = ['A','B','C','D','E']
	
    # first, use a 119 length array to track first letter
    first_let = []
    set_start_indices = np.zeros(119,dtype=np.int32)
    for i in range(1,120):
    	fprint( (i-1)*5 + 1 )
    	first_let.append(judge_input())
    	set_start_indices[i-1] = (i-1)*5 + 1
    
    # find the missing letter
    #eprint(first_let)
    let_counts = [first_let.count(x) for x in letters]
    #eprint(let_counts)
    letter_of_interest = letters[np.argmin(let_counts)]
    letters.remove(letter_of_interest)
    final_ans = final_ans + letter_of_interest
    #eprint("First Letter: " + letter_of_interest)
    
    # now, we only want to query the 23 items that have this first letter
    set_start_indices = set_start_indices[np.where(np.array(first_let) == letter_of_interest)[0]]
    #eprint(set_start_indices)
    
    # second letter
    second_let = []
    for i in set_start_indices:
    	fprint( i + 1 )
    	second_let.append(judge_input())
    	
    # find missing letter
    #eprint(second_let)
    let_counts = [second_let.count(x) for x in letters]
    letter_of_interest = letters[np.argmin(let_counts)]
    letters.remove(letter_of_interest)
    final_ans = final_ans + letter_of_interest
    #eprint(letter_of_interest)
    
    # now, we only want to query the 5 items that have this first and second letter
    set_start_indices = set_start_indices[np.where(np.array(second_let) == letter_of_interest)[0]]
    #eprint(set_start_indices)
    
    # third letter
    third_let = []
    for i in set_start_indices:
    	fprint( i + 2 )
    	third_let.append(judge_input())
    	
    # find missing letter
    #eprint(third_let)
    let_counts = [third_let.count(x) for x in letters]
    letter_of_interest = letters[np.argmin(let_counts)]
    letters.remove(letter_of_interest)
    final_ans = final_ans + letter_of_interest
    #eprint(letter_of_interest)
    
    # now, we only want to query the 1 items that have these first 3 letters
    set_start_indices = set_start_indices[np.where(np.array(third_let) == letter_of_interest)[0]]
    #eprint("After three")
    #eprint(set_start_indices)
    
    # fourth letter
    fourth_let = []
    for i in set_start_indices:
    	fprint( i + 3 )
    	fourth_let.append(judge_input())
    	
    # find missing letter
    #eprint(fourth_let)
    let_counts = [fourth_let.count(x) for x in letters]
    letter_of_interest = letters[np.argmin(let_counts)]
    letters.remove(letter_of_interest)
    final_ans = final_ans + letter_of_interest
    final_ans = final_ans + letters[0]
    
    fprint(final_ans)
    if judge_input() == 'Y':
        # #eprint('Success!')
        return
    sys.exit(99)

# Flush printing
def fprint(*args, **kwargs):
    print(*args, **kwargs, flush=True)


# Error printing, for debug only
def eprint(*args, **kwargs):
    print(*args, **kwargs, flush=True, file=sys.stderr)


# Make sense of judge output
def judge_input():
    x = input()
    # #eprint(x)
    if x == 'N':
        sys.exit(99)
    return x


if __name__ == '__main__':
    main()
~~~~
