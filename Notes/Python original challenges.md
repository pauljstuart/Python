# Python Challenges





### Python — Add the first 5 elements from two lists



Question:

You have two lists stored in variables `First` and `Last`. The length of the first list is five whereas the length of the second list is six. How can you add the first five elements from each list?



```
# add the first 5 elements from each list

First = [8, 5, 7, 8, 2]
Last = [77, 41, 22, 65, 7, 5]

result = zip(First, Last)

print(list(result))

result = [i + j for i, j in zip(First, Last)]

print(result)
```



 

### Python — Find all prime numbers up to 100

 

 ````
 # Find all prime numbers up to 100
 
 def isPrime(X):
     if X == 0:
         return False
     if X == 1:
         return False
 
     for I in range(2, X):
         if (X % I) == 0:
             return False
     return True
 
 
 # print(isPrime(5))
 
 primes_list = [i for i in range(1, 100) if isPrime(i) == True]
 
 print(primes_list)
 ````



 

### Python — Fibonacci sequence (two approaches)

 

 Part A: Fibonacci using a list and a `while` loop

````
# First Fibonacci :

def fibonacci(N):
    """Return all fibonacci numbers up to N. """

    result = [0]
    next_n = 1

    while next_n <= N:
        result.append(next_n)
        next_n = sum(result[-2:])

    return result


# print(fibonacci(0)) # [0]

# print(fibonacci(1)) # [0, 1, 1]

# print(fibonacci(50)) # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
````



Part B: Fibonacci using a generator (yield)

  ````
  
  # Second Fibonacci with Generator
  
  def fibonacci(N):
      """Return all fibonacci numbers up to N. """
  
      yield 0
  
      if N == 0:
          return
  
      a = 0
      b = 1
  
      while b <= N:
          yield b
          a, b = b, a + b
  
  
  # old_a = a
  # a = b
  # b = old_a + b
  
  
  # print(list(fibonacci(1)))
  # [0, 1, 1]
  
  # print(list(fibonacci(50)))
  #
  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
  
  
  print(list(fibonacci(50)))
  ````



 

### Python — Sentiment Analysis Using TF-IDF and Logistic Regression

 

This code uses Scikit-learn to train a simple machine learning model that classifies movie reviews as Positive or Negative

````


from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline

texts = [
    "this is a good movie",
    "this is a bad movie",
    "I love this movie",
    "I do not like this movie",
]

labels = ["Positive", "Negative", "Positive", "Negative"]

new_reviews = [
    "I really enjoyed this movie",
    "This movie was not great",
]

model = make_pipeline(
    TfidfVectorizer(),
    LogisticRegression()
)

model.fit(texts, labels)

predictions = model.predict(new_reviews)
print(predictions)
````







 

### Python - Count Occurrences and word frequencies

  

Part A : Count occurrences of elements in a list


````
item_list = ["a", "b", "a", "c", "b", "a"]
 
# or
 
item_list = [1, 2, 2, 3, 1, 1]
  
freq = {}
 
for value in item_list:
  if value in freq:
      freq[value] += 1
  else:
      freq[value] = 1
 
 
print(freq)
````

````
# alternatively
freq = {}
 
for value in item_list:
    freq[value] = freq.get(value, 0) + 1
 
 
print(freq)
````

 

Part B : Flatten text into words and count frequencies

````
# Flatten into list of words, and get rid of characters, Using list comprehension

document = [
    "the cat in the hat",
    "the quick, brown fox.",
    "jumped over the lazy dog?",
    "the dog sat in the hat."
]

# Flatten into words, strip punctuation, lowercase

words = [
    w.strip("!,.?").lower()
    for line in document
    for w in line.split()
]

print(words)


# Find the frequency of each word, using a comprehension

freq_dict = {}
freq_dict = {w: words.count(w) for w in set(words)}

print(freq_dict)
````



 

### Python - De-duplicating a List (remove duplicates)

 


items = [ 1,2,2,2,3,4,1,5,2,7,8,15,12,3]




##### solution 1 -  Fast, order not preserved

```
unique = list(set(items))

print( unique)
```

#### solution 2 - Fast, order preserving

```
# notes :
# Use when: you must keep first occurrence order.
# Works for: hashable elements (str, int, tuple, etc.).
#
# Time O(n) 


seen = set()
unique = []

for x in items:
    if x not in seen:
        seen.add(x)
        unique.append(x)

# print( unique )
```






### The “Maximum Subarray Sum” challenge


On HackerRank it usually appears under “Algorithms → Dynamic Programming” (sometimes in the warmup/interview prep kit).


Problem (typical wording)

Given an integer array, find the maximum possible sum of a contiguous subarray.


Input: [-2, 1, -3, 4, -1, 2, 1, -5, 4]

Output: 6
```
# simplest 

# remember  : At each number, either extend the previous subarray or start a new one. 
# If the previous subtotal is negative, it is always better to start again.

def max_subarray_sum(arr):
    current_sum = arr[0]
    max_sum = arr[0]

    for x in arr[1:]:

        if current_sum < 0:
            current_sum = x
        else:
            current_sum += x

        max_sum = max(max_sum, current_sum)

    return max_sum
```




 ````
# Kadane's Algorithm
Input = [-2, 1, -3, 4, -1, 2, 1, -5, 4]


def max_subarray_sum(arr):

  current_sum = max_sum = arr[0]

  for x in arr[1:]:
    current_sum = max( x, current_sum + x)
    max_sum = max(max_sum, current_sum)

  return max_sum


max_sum = max_subarray_sum( Input )

print( f"max sum is {max_sum}")

 ````



### Python - Fizz Buzz

 

#### solution 1 - proc

```
def fizzBuzz(n):

        if I % 3 == 0 and I % 5 == 0:
            return("FizzBuzz")
        if I % 3 == 0:
            return("Fizz")
        if I % 5 == 0:
            return("Buzz")

        else:
            return( str(I)  )
        
f_list = []

for I in range(1,101):
    f_list.append(fizzBuzz(I))
        
string_f ="\n".join(f_list)
print(string_f)
```

#### solution 2 - comprehension

```
result_t = [ ((i % 3 ==0)*"Fizz" + (i % 5 == 0)*"Buzz") or str(i)      for i in range(1,101)]

print( "\n".join( result_t ) )
```

