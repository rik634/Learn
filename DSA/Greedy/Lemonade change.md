# lemonade change
-  Given an array representing a queue of customers and the value of bills they hold, determine if it is possible to provide correct change to each customer. Customers can only pay with 5$, 10$ or 20$ bills and we initially do not have any change at hand. Return true, if it is possible to provide correct change for each customer otherwise return false.

### Logic
- 

### Solution
```
bool lemonadeChange(vector<int>& bills) {
        
        int five=0;
        int ten =0;
        for(int i=0;i<bills.size();i++)
        {
            if(bills[i]==5)
            {
                five++;
            }
            else if(bills[i]==10)
            {
                if(five>0)
                {
                    five--;
                    ten++;
                }
                else
                {
                    return false;
                }
            }
            else
            {
                if(ten>0 && five>0)
                {
                    five--;
                    ten--;
                }
                else if(five>=3)
                {
                    five-=3;
                }
                else
                {
                    return false;
                }
            }

        }
        return true;
    }
```
- Time Complexity: O(N)
- Space Complexity: O(1)