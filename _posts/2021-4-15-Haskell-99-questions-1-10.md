---


layout: post
title: Haskell 99 questions 1 to 10
subtitle: 1-10
tags: [Haskell, Code]

---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>



## Problem 1
(*) Find the last element of a list.

(Note that the Lisp transcription of this problem is incorrect.)

Example in Haskell:

```
λ> myLast [1,2,3,4]
4
λ> myLast ['x','y','z']
'z'
```

### solution

```haskell
myLast :: [a] -> a
myLast [] = "empty"
myLast [x] = x
myLast (_:xs) = myLast xs
```

## Problem 2
(*) Find the last but one element of a list.

(Note that the Lisp transcription of this problem is incorrect.)

Example in Haskell:
```
λ> myButLast [1,2,3,4]
3
λ> myButLast ['a'..'z']
'y'
```

```haskell
--solution1
myButLast :: [a] -> a
myButLast x = reverse x !! 1

--solution2
myButLast' [x,_] = x
myButLast' [_:xs] = myButLast' xs
```
## Problem 3
(*) Find the K'th element of a list. The first element in the list is number 1.

Example:
```
* (element-at '(a b c d e) 3)
c
```

Example in Haskell:

```
λ> elementAt [1,2,3] 2
2
λ> elementAt "haskell" 5
'e'
```


```haskell
--solution1
elementAt :: [a] -> Int -> a
elementAt list n = list !! (n - 1)

--solution2
elementAt' (x:_) 1 = x
elementAt' [] _ = error "out of bounds"
elementAt' (_:xs) k = elementAt' xs (k - 1)
```

## Problem 4
(*) Find the number of elements of a list.

Example in Haskell:

```
λ> myLength [123, 456, 789]
3
λ> myLength "Hello, world!"
13
```

```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
```haskell

```
