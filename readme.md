# A Gentle Introduction to Reading a Program Without Its Source Code

## Introduction

Usually, when we want to understand a program, we read its source code. But what if we don't have the source code?

Well, we still have the executable. And the CPU somehow understands what to do with it, doesn't it? So perhaps we can understand it too.

In this lab you'll receive a small program called `challenge`. It asks for a password. Unfortunately, I've forgotten the password, and I seem to have lost the source code as well. (:

Your task is to find the password **without guessing thousands of numbers**. You'll look at the machine instructions and figure out what calculation the program performs.

You won't need to write any C or assembly code today. You'll be *reading* assembly instead.

## Let's see what we have

Log in to our server, `the.hell.am`, clone this repo and go to the directory containing your lab files.

First, run the program:

```sh
./challenge
```

Try entering a number. Did it work? Perhaps try one more. No luck?

You could keep guessing, of course. But we have better tools.

Let's ask Unix what kind of file this is:

```sh
file challenge
```

What architecture is it built for? Does `file` say it's an executable? Open `answers.md` (create it if it doesn't exist) and write down what you learned.

## Looking inside

The CPU doesn't execute C code. It executes machine instructions, which are just bytes. Thankfully, we don't have to read those bytes directly: a program called `objdump` can translate them into assembly instructions for us.

Try:

```sh
objdump -d challenge
```

Quite a lot of text, isn't it? Don't worry. You don't have to understand all of it.

At the left you see addresses, then the bytes of machine instructions, then their assembly representation. You might notice function names enclosed in angle brackets, such as `<main>`.

Try finding the function called `check_password`. You can ask `objdump` to show just that function:

```sh
objdump -d --disassemble=check_password challenge
```

Ah, this is much shorter. And this is the interesting part of our program.

## What should you submit?

Open **`answers.md`** in your lab directory and answer the following questions. Please explain your reasoning; don't just write the password.

1. What kind of executable is `challenge`, and which CPU architecture is it for? How did you find out?
2. What hexadecimal constant did you find in `check_password`? What is its decimal value?
3. At which stack offset (relative to `rbp`) is that constant stored?
4. Which arithmetic instructions transform the constant? Describe what each one does, in order.
5. What is the final calculated number? Show your calculation.
6. At which stack offset is the calculated number stored? Where is the number entered by the user stored?
7. Which instruction compares the entered number with the calculated number? What do `sete` and `movzbl` do afterwards?
8. Did the program accept your answer? Copy the relevant program output into `answers.md`.

Also save the disassembly of the function in a file:

```sh
objdump -d --disassemble=check_password challenge > disassembly.txt
```

The `>` symbol redirects the output to a file. Useful, isn't it? Now you don't have to copy the output from your terminal.

Submit **`answers.md`** and **`disassembly.txt`** to your lab repository:

```sh
git add answers.md disassembly.txt
git commit -m "Solve the binary challenge"
git push
```

## A little extra, if you're curious

Look at the bytes of the instruction which stores the initial constant. Can you identify the bytes representing the constant itself?

Why might those bytes appear in a different order than you'd expect when reading a hexadecimal number from left to right? Think about *little-endian* representation.

You can write your thoughts under a **Bonus** heading in `answers.md`.

That's it. You have just done a little reverse engineering. And the nice part is that there wasn't any magic involved: only registers, memory, and a few ordinary CPU instructions.
