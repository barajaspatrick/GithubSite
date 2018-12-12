Data Structures: Tools for Problem Solving
==========================================

For a algorithms class a classmate was working on the problem:

### Find the Missing Element

*Consider an array of non-negative integers. A second array is formed by
shuffling the elements of the first array and deleting a random element.
Given these two arrays, find which element is missing in the second
array.*

*Here is an example input, the first array is shuffled and the number 5
is removed to construct the second array.*

**Input:** *finder(\[1,2,3,4,5,6,7\],\[3,7,2,1,4,6\])* **Output:** *5 is
the missing number*

This is the solution we came up with.

We create a python dictionary that stores the occurrence of each number
from the first array, for example 'cat' would be stored as {c:1, a:2,
t:1}. the algorithm would then take the second array and subtract the
number of occurrences from the dictionary. For example if the second
array was 'at', the ending dictionary would result in {c:1, a:0, t:0}.
The algorithm would then return any non-zero value as the missing
element: "c is the missing element in the second array".

    import collections
    def finder2(arr1, arr2):
        d = collections.defaultdict(int)

        for num in arr2:
            d[num] += 1

        for num in arr1:
            if d[num] == 0:
                return num
            else:
                for d[num] -= 1

The Missing Data Structure
--------------------------

The question then came up: *How would you solve this problem in R being
that there is no built in dictionary data structure?*

A Data structure is just a tool, to store and organize data. Some data
structures are more efficient than others for particular tasks. But its
important not to be bogged down by trying to fit your problem to a
predetermined data structure. Rather your approach shoud be to find a
data structure that fits the problem you are trying to solve.

In what follows will be an example of how we can solve the exact same
problem but without a dictionary data structure.

The first thing we can do is create a data frame object with a column to
store the numbers that occur in our array. R's 'unique' function allows
us to take all the unique values in our first array. We can then create
a second column 'count' to keep track of the number of occurrences of
each number in our array. For now we will set the values to '0'.

    a = c(0,1,2,3,4)
    b = c(0,1,2,4)

    df <- data.frame(arr = unique(a),
                     count = 0)
    df

    ##   arr count
    ## 1   0     0
    ## 2   1     0
    ## 3   2     0
    ## 4   3     0
    ## 5   4     0

Just like we did with the python dictionary, we go though the first
array, and add +1 to the count column in our data frame each time we
read a value.

    for (i in 1:length(a)){
                    df[i,2] <- df[i,2] + 1
    }
    df

    ##   arr count
    ## 1   0     1
    ## 2   1     1
    ## 3   2     1
    ## 4   3     1
    ## 5   4     1

Next we do the same thing with our second array, but instead subtract
from the count column every time we read a value.

    for (i in 1:length(b)){
                    df[which(df$arr == b[i]),2] <- df[which(df$arr == b[i]),2] - 1
    }
    df

    ##   arr count
    ## 1   0     0
    ## 2   1     0
    ## 3   2     0
    ## 4   3     1
    ## 5   4     0

Finally we can read through our count column and return the value of any
non-zero. The row with a non-zero in the count column will be the
missing value.

    missingNum <- df[which(df$count != 0),1]
    print(paste0("the missing number is ", missingNum))

    ## [1] "the missing number is 3"

Bringing it all together
------------------------

When we bring everything together in one neat little function we can see
that our solution is not drastically different from the way we solved
the problem in python. While there are plenty of resources online
<https://stackoverflow.com/questions/22084338/pandas-dataframe-performance>
that explain why using a dictionary object in python is more efficient
than a data frame, but the point remains that with a few extra lines of
code and a slightly different view on the problem we can arrive at the
same solution.

    finder <- function(arr1, arr2){
            df <- data.frame(arr = unique(arr1),
                     count = 0)

            for (i in 1:length(arr1)){
                    df[i,2] <- df[i,2] + 1
            }

            for (i in 1:length(arr2)){
                    df[which(df$arr == arr2[i]),2] <- df[which(df$arr == arr2[i]),2] - 1
            }

            missingNum <- df[which(df$count != 0),1]
            print(paste0("the missing number is ", missingNum))
    }

    a = c(0,1,2,3,4)
    b = c(0,1,2,4)
    finder(arr1 = a,  arr2 = b)

    ## [1] "the missing number is 3"
