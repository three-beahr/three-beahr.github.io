---
layout: default
title: "A: What do you do when I'm gone? B: ....wait for you to get back"
date: 2025-09-28
---

### Absence

I fell off the Haskell wagon it would seem, but now I'm back and better than never (in reference to being late, that is). Expect more regular posts and updates from this point on. I also started from a new [lecture/reference set](https://haskell.mooc.fi/) for no particular reason other than it's been a while, I couldn't find the old link (even though I realized it's in the last post...), and I recently swapped to a linux install on my main desktop. 

# A New Kind of Problem

An interesting thing I wanted to bring up is the concept of **kinds**. I ran across it today when erroneously trying to annotate a function by including parameters rather than simply including types - something common to languages with which I am familiar (e.g. Python, C). After using `:t` on pretty much everything in GHCI (partially because I'm still not quite clear on the whole `=>` vs. `->` business), I think my brain started associating **type annotations** with parameters. Since I only really knew Prelude functions involving lists to start, I saw `[a]` is used in place of inputs regarding lists. As a result, I tried something like:

```emailwrapper username site = concat (concat (concat username "@") site) ".com"
     where concat :: String str -> String str ->  String str
           concat base addition = base ++ addition 
```

and was confronted with an error from the VS Code Haskell extension: `Expected kind ‘k0 -> *’, but ‘String’ has kind ‘*’`. So obviously I decided to investigate what a kind was to better understand this. After searching around a bit, it seems like kinds are abstractions meant to better describe classes of  constructors, specifically **type constructors**. From the [Wikipedia page](https://en.wikipedia.org/wiki/Kind_(type_theory)), we get the sense that there are enough various constructors in functional programming that this is at least slightly important. Of particular note for me was the form of classifying these type constructors:

0. **'Nullary'** type constructors that require no external type information to construct an object, represented by kind `*`, the simplest kind. Think of constructors like `Int`, `Double`, `String`, etc.
1. **'Unary'** type constructors that take a single external type as information to construct an object, represented by kind `* -> *`. `List` is a good example of this and `Set` would be if it were in Haskell by default.
2. **'Binary'** type constructors that take two external types as information to construct an object, represented by `* -> 

Some of this still doesn't make total sense to me given that Haskell seems to not have issue with functions with multiple inputs. Although the fact that that is typically represented as chains of single input functions (e.g. `(* -> *) -> *` for a two input function) may be related to the left associativity inherent to Haskell mentioned briefly in the lesson and something in the `kind` wiki page called **currying**. I'm not sure... I could probably ask one of the GPT spawn for an answer in less than a minute, but since I'm in the learning phase AND this is MY thing AND I like finding things out for MYSELF... I'm not gonna do that! I read the page on currying, and it looks like it could have it's own post later on as I really get into all this. But I'm going to avoid getting bogged down right now.

# I'm a Tail Recursion Guy, Myself

**Tail recursion** seems to be an interesting topic. The idea is that a recursive function saves a lot of hassle and computation time if its final operation is to do a single call of itself, thereby continuing a sort of chain at the 'tail'. In Lecture 2 Section 1, it describes changing a calculation of the Fibonacci sequence from a tree-based recursive calculation to a tail-recursive calculation via storing data for the calculation in parameters of the function. In Lecture 2 Exercise 2b.1 (creating a function for the calculation of the binomial coefficient), I was trying to thing of how to do such a thing. The referenced formula for the binomial coefficient was `binomial n k = binomial (n-1) (k) + binomial (n - 1) (k - 1)`, but I kept getting stuck on the idea that there needed to again be a way to store information related to this calculation in parameters in some way to make the function tail recursive. Had I made better use of my silly little math degrees tonight, I would have remembered that the binomial coefficient formula has another form in terms of an iterative numerator and denominator calculation. But since I was doing this on a later day and later in the evening, I caved, asking GPT if it was possible to even write the formula in a tail-recursive way (which ended up spoiling the answer). It did agree with my expectation in that the it is essential to write a given formula by quantifying a few fixed, iteratively updated variables in order to write it in a tail-recursive way. I will look more into this when I have some free time. I ended up changing 

```
binomial n 0 = 1
binomial 0 k = 0 -- Don't need k > 0 check since after n 0 case
binomial n k = binomial (n-1) (k) + binomial (n - 1) (k - 1)  
```

into the particularly longer yet computationally fficient:
```
binomial n k = binomial' n k n k 1
binomial' :: Integer -> Integer -> Integer -> Integer -> Integer -> Integer
binomial' n k num denom i 
   | k == 0 = 1
   | n == 0 = 0
   | i == k = div num denom
   | otherwise = binomial' n k (num * (n - i)) (denom * (k - i)) (i + 1)
```

## Snippet Notes

- Using the `Either` type in an annotation requires later specification of `Left` or `Right` within the subsequent binding to alert Haskell as to which type provided in `Either` a given definition's type actually is.
- Similarly, using the `Maybe` type will later require a specification of one of the two subtypes (either `Just a`, parameterized by `Maybe a`, or `Nothing`). I believe this along with `Either` are my first taste of the **parametrized types** I read about.
- Using these types in a function's annotation requires a specification later based on what was used and where. If a parametrized type is used in an input, then a choice must be made for pattern matching/cases, etc. regarding each individual input for which such a type (`Maybe`, `Either`, etc.) was used in the annotation. If a parametrized type is used in the annotation as an output, you must also once specify at output the type used for that particular case. See the following exercise from lecture 2 in the page linked at the top of this post:

      - Ex 11: implement the function addEithers, which combines two values of type `Either String Int` into one like this:
            * If both inputs were Ints, sum the Ints
            * Otherwise, return the first argument that was not an Int

            ```addEithers :: Either String Int -> Either String Int -> Either String Int
            addEithers (Left a) _ = Left a
            addEithers (Right a) (Left b) = Left b
            addEithers (Right a) (Right b) = Right (a + b)```

* **Note** the specification for each input in each (non `_`) case, as well as the single specification for the output that encompasses the whole expression.

## Investigate Later:

- Generalized Algebraic Data Types (GADTs) and handling inputs of uncertain types.
- Currying and de-currying
- Parametric Polymorphism

### Relevant Links:

- [Wikipedia: (Type Theory) Kinds](https://en.wikipedia.org/wiki/Kind_(type_theory))
- [Haskell Wiki: Kinds](https://wiki.haskell.org/index.php?title=GHC/Kinds)

- [WikiBooks: GADTs in Haskell](https://en.wikibooks.org/wiki/Haskell/GADT)
- [Wikipedia: Currying](https://en.wikipedia.org/wiki/Currying)

- A fun little math thing I thought about when making recursive functions. I am curious about its convergence [Continued Fractions](https://en.wikipedia.org/wiki/Continued_fraction)