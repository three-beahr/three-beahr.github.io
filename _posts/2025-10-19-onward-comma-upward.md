---
layout: default
title: "Onward, upward"
date: 2025-10-19
---

## Reminder

Still I read from [The Great MOOC of Finland](https://haskell.mooc.fi/part1). In this post I have completed up through Lecture 3.

## Partial application is wholly exciting

And now we see why the first class citizenship of functions can be so exciting! I have been having a great time just putting my functions wherever (minding the type-police, of course) and leaving blanks where I want. Who thought that not knowing things could be so exciting! I think I am coming to appreciate in part the wonderful idea of functions mattering more than inputs and outputs, as I grew to love in mathematics. The structure of mappings (shoutout cohomology) is wholly more exciting (and deliciously difficult) than what things you start with and what things you end with. It's all about the journey folks.

### A note on currying

I would like to point out that I have in part understood the connection between **currying** and **left-associativity** that was confusing me a few days ago. This revelation is thanks to the notes in Section 3.2 of the MOOC reference document. In mathematics, it happens that writing a mapping diagram proceeds left to right (shout-out Category Theory) because of our nature to read in this way. Because Haskell is functional and its type annotations look identical to such a structure, I confounded the two. This led me to believe that I should be thinking of the currying structure as _function composition_ which has the almost opposite form, wherein functions are successively applied to the left hand side, so that reading successive function applications _ends_ with the innermost function, like peeling away layers of an onion (or cake, or parfait, shout-out Donkey). So, an evaluation `h(g(f(x)))` show us that `domain(f) -> domain(g) -> domain(h)`. 

But in Haskell, this is not the case. These chains of types are definining multi-input functions, and so it naturally follows that given the existence of **partial application** in Haskell, the left-most expression is always treated as the function which makes the topsy-turvy right-to-left function evaluation of mathematics (which is beautiful and elegant and perfect, mind you) still accurate, although Haskell is stealing the notation to instead do something else. When viewing the whole deal as a multivariate function in which you fill in various inputs in a particular order via convention, this all makes much more sense. You are, in essence, turning a function like $ f : X \times Y \times Z \to A $ to a function $ f_x: Y \times Z \to A $. This more accurately fits the form defined by partial application, although I kinda wish they didn't use arrows for this. 

All this to say that, viewing it as a multivariate function rather than a sequence of univariate function applications, the left-associativity is much more reasonable. In haskell `f :: Integer -> Integer -> Integer ` combined with the partial application `f x` can be thought of as restricting $ f $ to $ f_x $. This matters more (and was much less clear for me) in cases where functions were inputs to other functions, e.g. `f :: (Integer -> Integer) -> Integer`

## There's no point...

A large investment of time in the third module of my MOOC-along coding has been spent understanding the form and value of **point-free** programming. The primary notion of this style is that one wishes to avoid referring to arguments of functions (and somewhat less importantly, large gaggles of parentheses). This is primarily done in Haskell via the composition operator `.` and the function-application operator `$`. I personally have been tending to avoid the latter, as I am trying to limit my usage of arguments in the spirit of programming *functionally*. But the composition operator has been interesting to play with. One can convert something like 

``hasEvenWords x = even (length (words x) `mod` 2)``

into something that avoids naming a parameter and removes the need to visually unpack parentheses in a sometimes chaotic order:

``hasEvenWords = even . (`mod` 2) . length . words``.

This solution, while a bit more streamlined, will definitely take some getting used to. I appreciate that parentheses are 'visually cluttering', but also value their straight-forward structural hints. It will take some time for me to appreciate and get comfortable with reading functions more similar to the second version above.

## Listy business

One thing I saw that was a striking example in the Section 3 material was this simultaneous consumption and creation of list elements here:

```
doubleList :: [Int] -> [Int]
doubleList [] = []
doubleList (x:xs) = 2*x : doubleList xs
```

This seems quite fun. After some research, it looks like with Haskell's garbage handler it will be pretty memory efficient and is just nice to know that there can sometimes be what looks like a form of immutability if computations on a list are relatively independent of other list elements. I imagine there is a way to still maintain this good form even with a computation that involved looking at one or both list neighbors. This also led me to learn about the term **thunks**, which looks like it goes the other direction - a concept resulting from Haskell's deferred computation style that can sometimes lead to memory *issues*. But that's a topic for another day.

One thing that also appeared pretty important for the future was the distinction made between tail-recursive definitions of functions and non-tail-recursive definitions. For example, the lecture presents:

```
-- Not tail recursive!
doubleList :: [Int] -> [Int]
doubleList [] = []
doubleList (x:xs) = 2*x : doubleList xs
```

contrasted with 

```
-- Tail recursive version
doubleList :: [Int] -> [Int]
doubleList xs = go [] xs
    where go result [] = result
          go result (x:xs) = go (result++[2*x]) xs
```

Noting that the first completes in $ O(n) $ time while the second requires walking the whole list at each call due to the implementation of `++`, meaning that it would run in $ O(n^2) $ time. This subtle distinction is something I am excited to see (and be concerned with) in the future.

## Investigate Later:

- **thunks** and forced non-laziness via foldl'