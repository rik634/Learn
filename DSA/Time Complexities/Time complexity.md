
# Time complexity:

1.	Worst case O()- Big O: It represent the worst possible scenario. The algorithm will never be slower than this bound. 
2.	Best case - Omega: It represent the best possible scenario. The algorithm cannot o better than this. 
3.  Tight/exact bound - Theta: It describes the actual growth of the algorithm when upper and lower bound meet. It represent the most accurate description of how the algorithm behaves as the input grows.
     We can think of Theta as the true behavior zone, where the algorithm’s performance is consistently bounded both above and below.


O() - upper bound (worst case)

Omega() - lower bound (best case)

Theta() - Tight bound (actual growth)

## Common algorithms TC:
<img width="1920" height="1008" alt="image" src="https://github.com/user-attachments/assets/b0c560e3-32c2-4bd7-8c8b-eefdc29e399c" />


## Amortized analysis:
⦁	When we push an iten in stack, it is super fast O(1). But sometimes, when the stack runs out of space, it has to resie itself, and that takes longer. 
⦁	During resizing: This copying takes O(n) time. So, thr actual cost of push is O(1) or O(n)? -> Amortized analysis helps to figure it out.
- ⦁	In many algorithms, most operations are cheap, but a few are expensive. 
⦁	If we only look at the worst single operation, the algorithm looks bad. 
⦁	But, if we zoom out and look at many operations together, the picture changes completely.
- ⦁	Amortized analysis says: “Don’t judge one operation, look at the average cost across many operations.”
⦁	So, amortized analysis spreads the cost of expensive operations across all the cheap ones, no randomness.
- Amortized analysis doesn’t care about random inputs. It just looks at how the algorithm behaves over a long run.
- ⦁	So, in case of stack example we saw, even if the  cost of resizing when stack is full is O(n), the average cost per push stays close to O(1). 
⦁	Amortized cost per push = O(1)

### Ways to calculate it:
1.	Aggregate method (Simplest): Add up the total cost of all operations, then divide by the number of operations. 
2.	Accounting method (Token/Credit approach):
   - Give each operation some “extra credit” upfront.
   - Cheap operations save their extra credits.
   - Expensive operatiosn use those saved credits. 
3. 	Potential method (most formal):
    - Think of the data structure as having “stored energy” called potential (phi).
    - Amortized cost = actual cost  + (phi after - phi before)
    - When energy builds up, it pays for future expensive operations. This method is used in advanced topics like fibonacci heaps and union find. 

