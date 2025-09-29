---
layout: default
title: "My Very First Haskell Post"
date: 2025-07-22
---

# Hello everyone! 

This will be my blog as I journey into the wonderful world of Haskell.

I want to learn a functional programming language for use in developing low-latency trading software like the ones used by the world at large. Today is my first day of setting up my blog and getting acquainted with my newly beloved lazy, functional, consistently-typed partner. I've looked very briefly at Haskell in the past week or so, and I'm ready to really dig in. For reference, I'm beginning with the lessons posted publicly by the University of Pennsylvania (shoutout! Never been though) until either I have completed it or it is no longer useful. It is posted online [here](https://www.cis.upenn.edu/~cis1940/spring13/).

Today I want to solidify in my mind the compilation/execution process and get into the basics of working with the statically-typed system and function applications. A good chunk of time on my first day has gone to getting all of the blog stuff set up and looking nice, so I don't have a ton of interesting things to share. First and foremost, I think I'll just note the big takeaways from the day:

- Typing is denoted with double colons `::`

- A few of the basic, most notable pre-defined types that will be useful to me are `Int`, `Integer` (for arbitrary-digit ints), `Double`, `Bool` and `Char`.

- 'Regular' Haskell files have the extension `.hs`, while 'legible' Haskell files use the extension `.lhs`. Legible Haskell files are a bit like jupyter notebooks - they contain a mixture of code and writing, although I don't believe one can execute individual blocks in the same way that `.ipynb` files allow. I believe legible files mainly serve to assist with documentation and instruction. I probably won't use them too frequently, since this blog will essentially be exactly that, but it could be useful for people developing instructional Haskell pages where transfer between parties is more important.

- To compile a Haskell program:

    1. Create your Haskell program 

    2. Use the Glasgow Haskell Compiler (GHC) similarly to how you would use a standard C compiler: `ghc -o exec_name source_file.hs`

        - Note that this is best utilized for single-file programs or programs with one main file and maybe a few auxiliary files. If you are using it with multiple files, one needs to specify only the source file containing the `main` block.

        - For larger projects, it is instead wiser to use cabal, but that will come later, I'm sure.

## Experimentation

As with all new things, one of the best parts is learning things by trying new combinations of facts you've been given (or just crazy ideas if you happen to have them) and seeing how the world responds. So here are some things I learned about Haskell while playing around with it:

- In the REPL interpreter for Haskell, GHCi (`ghci` from the command line), one cannot introduce a variable without 'binding' it. That is, you must give a variable a value for the interpreter to allow it. Simple type declaration is not enough. So `y :: Int` or even `let y :: Int` will not be accepted by itself in GHCi. You must either assign ('bind') the variable in the same line (e.g. `let y :: Int; y = mod 3 2`) or you must simply forgo the type declaration altogether (e.g. `y = mod 3 2`).

I didn't have too much time tonight after getting the blog all running and fiddling with my files and such, so here I will just include the first Haskell file I wrote beyond "Hello, World!". It shows max integers at each end, since Haskell's `Int`s vary in size based on the compilation system:

# hello.hs

Here's what my original hello.hs file looked like after I moved on from just a "Hello, World!". First, there were some type definitions and assignments to some Haskell pre-defined values:

```
biggestInt, smallestInt :: Int
biggestInt = maxBound
smallestInt = minBound
```

Then, there are some strings created to hold the string that represents these two numbers. They are assigned from the original integers using the function `show`:

```
bI, sI :: String
bI = show biggestInt
sI = show smallestInt
```

Finally, we create the `main` block and print the two integers to the command line:

```
main :: IO()
main = putStrLn bI
       >> putStrLn sI
```

And that's it! That was my first Haskell file! I also added this text to it, so now it's been turned into a legible Haskell file.