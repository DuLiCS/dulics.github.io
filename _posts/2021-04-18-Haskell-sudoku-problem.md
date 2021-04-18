---


layout: post
title: Haskell Sudoku Problem
subtitle:
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



```haskell
import Data.List

{-
The input is a matrix, represented as a list
of rows, where each row is a list of the numbers
in the grid. The numbers are either 1, 2, 3, 4 and
0 represent the field is empty.

According to the description above，I construct the
type of the
-}
type Grid = Matrix Value
type Matrix a = [Row a]
type Row a = [a]
type Value = Int

--The box size of the 4x4 sudoku is 2
boxSize :: Int
boxSize = 2

--the element of the 4x4 sudoku is 1,2,3,4
values :: [Value]
values = [1 .. 4]

--the value 0 indi­cat­ing an empty cell.
empty :: Value -> Bool
empty = (== 0)

--sigle function we will use later to optimize our method
single :: [a] -> Bool
single [_] = True
single _   = False


--The sample input here
examplePuzzle :: Grid
examplePuzzle = [[3,4,0,0],
         [2,0,3,0],
         [0,3,0,2],
         [0,0,1,3]]

multipleSolutionsPuzzle :: Grid
multipleSolutionsPuzzle =
        [[3,0,0,0],
         [2,0,0,0],
         [0,3,0,0],
         [0,0,1,3]]


{-determing wether a grid is solved or not
our rows,columns and boxes must have all the digits(1,2,3,4)
without duplicates.Valid function extract rows,columns and boxes and
check this criteria.
-}
valid :: Grid -> Bool
valid g = all noDups (rows g) &&
          all noDups (cols g) &&
          all noDups (boxes g)

--noDups function iterate the list and determine whether the
--list have duplicate elements or not
noDups :: (Eq a, Num a) => [a] -> Bool
noDups [] = True
--list like [1,2,0,0] should be True
noDups (x : xt) = if x == 0 then  (noDups xt) else (not (elem x xt) && noDups xt)


--The next three is extrac­tion func­tion
rows :: Matrix a -> [Row a]
rows =  id

--columns are the transpose of the rows
cols :: Matrix a -> [Row a]
cols = transpose


--let the element from one box in one sublist,transpose the element in pairs
boxes :: Matrix a -> [Row a]
boxes = unpack . map transpose . pack
        where
 -- pack the puzzle list in pairs
 --[[[[3,4],[0,0]],[[2,0],[3,0]]],[[[0,3],[0,2]],[[0,0],[1,3]]]]
 --map the transpose to the subelements like [[[3,4],[0,0]],[[2,0],[3,0]]]
 --then concat the subelement
          pack   = split . map split
          split  = chop boxSize
          unpack = map concat . concat

--recombine the list
--chop 2 [1,2,3,4] = [[1,2],[3,4]]
chop :: Int -> [a] -> [[a]]
chop n [] = []
chop n xs = take n xs : chop n (drop n xs)


--a list of grids with every possibility for the empty squares filled in.
--a combination of deter­min­is­tic and non-deter­min­is­tic elements
collapse =  sequence . map sequence

--map the choice function into every list
choices g = map (map choice) g
            where
             --retain non-zero element and replace the 0 with [1..4]
              choice v = if empty v then values else [v]

--iterate all the possible grids,get all grids that meet the criterion of 4x4 sudoku
--The brute force solution
solveBrute :: Grid -> [Grid]
solveBrute =  filter valid . collapse . choices

--Optimise

--Enu­mer­at­ing all dig­its for empty spaces doesn’t take into account the con­straints of the puz­zle itself.
{-It take so long to solve the problem like
        [[3,0,0,0],
         [2,0,0,0],
         [0,3,0,0],
         [0,0,1,3]]
For each non-deter­min­is­tic cell, we can throw away any incon­sis­tent choice.
-}
prune =  pruneBy boxes . pruneBy cols . pruneBy rows
         where pruneBy f = f . map reduce . f

reduce xss =  [xs `minus` singles | xs <- xss]
              where singles = concat (filter single xss)

xs `minus` ys = if single xs then xs else xs \\ ys

solvePrune :: Grid -> [Grid]
solvePrune =  filter valid . collapse . prune . choices


solveFixPrune :: Grid -> [Grid]
solveFixPrune = filter valid . collapse . fix prune . choices

fix :: Eq a => (a -> a) -> a -> a
fix f x =  if x == x' then x else fix f x'
           where x' = f x
```
