# Haskell - how to read
[https://wiki.haskell.org/How_to_read_Haskell]

http://learn.hfm.io/fundamentals.html

Symbol | Meaning
:---|:---
\| | can be read as 'evaluates to’ or  'such that’ or 'or’
-> | can be read as 'returns’ eg a -> a can be read as: take a parameter 'a' and return 'a'
<- | can be read as 'is drawn from’ eg [x*2 \| x <- [1..10]]. x is drawn from [1..10]
<- | List comprehension generator, Single assignment operator in do-constr.
=> | can be read as 'evaluates to’
!! | can be read as 'extract element at a specific location’ - eg [0, 1, 2, 3] !! 2  ⇒   2
` | can be read as 'infix’ used to put functions before arguments rather than prefix them
f . g | can be read as 'f composed with g’
::  | can be read as 'has type'
++ | can be read as 'concatenate'
: | can be read as 'cons' - adds elements at the front - right associative operator
-- | Start of comment line
{- | Start of short comment
-} | End of short comment
+ | Add operator
- | Subtract/negate operator
* | Multiply operator
/ | Division operator, Substitution operator, as in e{f/x}
^ | Raise-to-the-power operators 
^^ | Raise-to-the-power operators
** | Raise-to-the-power operators
&& | And operator
|| | Or operator
< | Less-than operator
<= | Less-than-or-equal operator 
== | Equal operator
/= | Not-equal operator  
>= | Greater-than-or-equal operator
> | Greater-than operator
\ | Lambda operator 
. | Function composition operator, Name qualifier
| | Guard and case specifier, Separator in list comprehension, Alternative in data definition (enum type) 
++ |  List concatenation operator 
: | Append-head operator (“cons”)   
!! | Indexing operator
.. | Range-specifier for lists
\\ | List-difference operator 
; | Definition separator
-> | Function type-mapping operator., Lambda definition operator, Separator in case construction
= | Type- or value-naming operator
:: | Type specification operator, “has type of” 
=> | Context inheritance from class
() | Empty value in IO () type
>> | Monad sequencing operator
>>= | Monad sequencing operator with value passing 
>@> | Object composition operator (monads) 
(..) | Constructor for export operator (postfix) 
[and] | List constructors, “,” as separator
(and) | Tuple constructors, “,” as separator, Infix-to-prefix constructors
'and’ | Prefix-to-infix constructors 
’and’ | Literal char constructors   
"and" | String constructors
_ | Wildcard in pattern
~ | Irrefutable pattern 
! | Force evaluation (strictness flag) 
@ | “Read As” in pattern matching 

### The following table lists a number of tuple types and their names:

\# | Expression | Name
:---|:---|:---
0 | () | Unit
1 | n/a |  | n/a
2 | (x_1, x_2) | Pair
3 | (x_1, x_2, x_3) | Triple
4 | (x_1, x_2, x_3, x_4) | Quadruple
5 | (x_1, x_2, x_3, x_4, x_5) | Quintuple
 |  | ⋮  
n | (x_1, …, x_n) | n-tuple

### Some common pattern matches:

Expression | Matches | What it does
:---|:---|:---
[1,2,3] | 1:2:3:[]
x:xs | x:xs:[] | Binds the head of the list to x and the rest of it to xs, even if there's only one element so xs ends up being an empty list
x:y:z:zs | | Bind the first three elements to variables and the rest of the list to another variable

##### Examples:
*NB Backtick is to distinguish from the Prelude version...*
```haskell
head' :: [a] -> a  
head' [] = error "Can't call head on an empty list, dummy!"  
head' (x:_) = x  // NB _ = 'wildcard'
```
NB  if you want to bind to several variables (even if one of them is just _ and doesn't actually bind at all), we have to surround them in parentheses
```haskell
length' :: (Num b) => [a] -> b  
length' [] = 0  // Edge condition, matches empty list
length' (_:xs) = 1 + length' xs 


sum' :: (Num a) => [a] -> a  
sum' [] = 0  
sum' (x:xs) = x + sum' xs 
```
### As patterns:
Allows you to break something up according to a pattern and binding it to names whilst still keeping a reference to the whole thing. You do that by putting a name and an @ in front of a pattern. For instance, the pattern xs@(x:y:ys). This pattern will match exactly the same thing as x:y:ys but you can easily get the whole list via xs instead of repeating yourself by typing out x:y:ys in the function body again.
##### Example:
```haskell
capital :: String -> String  
capital "" = "Empty string, whoops!"  
capital all@(x:xs) = "The first letter of " ++ all ++ " is " ++ [x]  
```


### More on patterns:


                                     cons - add to front of 
                                             |
notation [a, b, c] is just a convenient way of using (:) to build a list:
a : (b : (c : [] )) => 	“abc” 		

*NB The round brackets. Pattern matching. [] would be cons.*

This is how it works
[‘h’,’i’] 		=> “hi”		is the same as
‘h” : ( ‘i’ : [] ) 	=> “hi”		is the same as
‘h’ :  ‘i’ : [ ] 	=> “hi”

##### Example:
```haskell
import Data.Char
import Prelude

allToUpper :: String -> String
allToUpper [] = []    -- case with empty String

--Step 1
--allToUpper [c1] = [toUpper c1]
--allToUpper [c2, c1] = [toUpper c2, toUpper c1]
--allToUpper [c3, c2, c1] = [toUpper c3, toUpper c2, toUpper c1]

--Step 2  replace expressions with function calls
--allToUpper (c1 : [])            = toUpper c1 : allToUpper []   add c1 to the front of [] 
--allToUpper (c2 : c1 : [])       = toUpper c2 : allToUpper (c1 : [])
--allToUpper (c3 : c2 : c1 : [])  = toUpper c3 : allToUpper (c2 : c1 : [])

-- so chr and restString always repeats, with toUpper applying to chr and alltoUpper to restString each time
-- the pattern that is matched is like head : 
-- this comes from 
-- head :: [a] -> a
-- head (x : xs ) = x
-- and
-- tail :: [a] -> [a]
-- tail ( x : xs ) = [xs]    -- works from innermost first? 

allToUpper (chr : restString) = toUpper chr : allToUpper restString    // toUpper is from Prelude

--eg
allToUpper "a"
allToUpper "ab"
allToUpper "abc"

allToUpper "this is a string"
```

(==) :: (Eq a) => a -> a -> Bool  
=> symbol

Everything before the => symbol is called a class constraint. We can read the previous type declaration like this: the equality function takes any two values that are of the same type and returns a Bool. The type of those two values must be a member of the Eq class (this was the class constraint). The Eq typeclass provides an interface for testing for equality. Any type where it makes sense to test for equality between two values of that type should be a member of the Eq class. All standard Haskell types except for IO (the type for dealing with input and output) and functions are a part of the Eq typeclass.

Ord is for types that have an ordering.
ie:
```
ghci> :t (>)  
(>) :: (Ord a) => a -> a -> Bool
```

Ord covers all the standard comparing functions such as >, <, >= and <=. The compare function takes two Ord members of the same type and returns an ordering. Ordering is a type that can be GT, LT or EQ, meaning greater than, lesser than and equal, respectively. To be a member of Ord, a type must first have membership in the prestigious and exclusive Eq club.

### Guards
 A guard is basically a boolean expression. If it evaluates to True, then the corresponding function body is used. If it evaluates to False, checking drops through to the next guard and so on, until it reaches 'otherwise'
##### Example:
```haskell
bmiTell :: (RealFloat a) => a -> a -> String  
bmiTell weight height  
    | weight / height ^ 2 <= 18.5 = "You're underweight, you emo, you!"  
    | weight / height ^ 2 <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | weight / height ^ 2 <= 30.0 = "You're fat! Lose some weight, fatty!"  
    | otherwise                   = "You're a whale, congratulations!"
    
max' :: (Ord a) => a -> a -> a  
max' a b   
    | a > b     = a  
    | otherwise = b
    
myCompare :: (Ord a) => a -> a -> Ordering  
a `myCompare` b           // NB backticks can be in the definition - easier to read
    | a > b     = GT  
    | a == b    = EQ  
    | otherwise = LT 
```
### 'where' blocks
##### Example:
```haskell

bmiTell :: (RealFloat a) => a -> a -> String  
bmiTell weight height  
    | bmi <= skinny = "You're underweight, you emo, you!"  
    | bmi <= normal = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | bmi <= fat    = "You're fat! Lose some weight, fatty!"  
    | otherwise     = "You're a whale, congratulations!"  
         where bmi = weight / height ^ 2  
               skinny = 18.5  
               normal = 25.0  
               fat = 30.0 
```
You can also use where bindings to pattern match
```haskell
    ...  
    where bmi = weight / height ^ 2  
          (skinny, normal, fat) = (18.5, 25.0, 30.0) 

```
```haskell
initials :: String -> String -> String  
initials firstname lastname = [f] ++ ". " ++ [l] ++ "."  
    where (f:_) = firstname  
          (l:_) = lastname 
          
-- or match directly in the function's parameters
initials' :: String -> String -> String  
initials' (f:_) (l:_) = [f] ++ "." ++ [l] ++ "."  
```
### 'where' blocks, - also can define functions
##### Example:
```haskell
calcBmis :: (RealFloat a) => [(a, a)] -> [a]  
calcBmis xs = [bmi w h | (w, h) <- xs]  
    where bmi weight height = weight / height ^ 2 
```

### Recursion
The function is applied inside its own definition, until an edge case(s) is reached, otherwise the recursion won't terminate.
##### Example 1:
```haskell
minimum' :: (Ord a) => [a] -> a
minimum' [] = error "Error"
minimum' [x] = x
minimum' (x:xs) 
    | x < minTail = x
    | otherwise = minTail
    where minTail = minimum' xs
```

The basic recipe:
1. Define one or more edge cases, eg:
```haskell
maximum' [] = error "maximum of empty list"  
maximum' [x] = x  
```
2. We want to determine the largest item in the array. If the head is larger than any value in the tail, then the head is the maximum. We use pattern matching (x:xs) == head & tail. Recursion means that if we had an array of [1,2,3,4] the caluculation would not be performed until the end case was reached:


```haskell
maximum' (x:xs) 
    | x > maximum' xs = x
```
3. But what is the largest value in the tail? Use maximum' to find out!
```haskell
    | otherwise =  maximum' xs
```
4. We can define a where clause
```haskell
maximum' (x:xs)
    | x > maxTail = x
    | otherwise = maxTail
    where maxTail = maximum' xs
 ```
5. Don't forget the definition!
```haskell
maximum' :: (Ord a) => [a] -> a    -- Ord == Bool
 ```
##### Example 2:
replicate 3 5 should return [5,5,5], so:
```haskell
replicate' :: (Num i, Ord i) => i -> a -> [a]    -- Ord == Bool
 ```
We can't replicate something zero times or a negative number of times, so those are the edge condtions. The key is to find the head:tail pattern that satifies the requirements. So we will be adding an x so long as the n is > 0...
```haskell
replicate' n x
     | n <= 0           -- edge condition
     | otherwise = x : replicate' (n-1) x     -- head:tail pattern
```
The guard tests for the boolean condition and appends an empty list if true. Else return a list that has x as the first element and x replicated n-1 times for the tail.

*__NB__: Num is not a subclass of Ord. That means that what constitutes for a number doesn't really have to adhere to an ordering. So that's why we have to specify both the Num and Ord class constraints when doing addition or subtraction and also comparison.*

##### Example 3:
take 3 [3,4,5,6,7,8] should return [3,4,5], so:
```haskell
take' :: (Num i, Ord i) => i -> [a] -> [a]    -- Ord == Bool
 ```
No point in taking 0 elements from an array, so that is one edge condition. Nor can we take n elements from an empty array:
```haskell
take' n _               -- _ = something undefined that doesn't actually exist...
    | n <= 0 = []
take' _ n = []          -- second edge contition
```
So we need to put the head element into an array and then take the next element, repeating n-1 times each iteration, until an edge condition is reached:
```haskell
take' :: (Num i, Ord i) => i -> [a] -> [a]  
take' n _  
    | n <= 0   = []         -- no otherwise clause means the matching just falls through...
take' _ []     = [] 
take' n x = x : take' (n-1) xs 
 ```
 
