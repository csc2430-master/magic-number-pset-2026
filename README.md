Here is a complete draft of the P-set, structured as a cumulative assignment for CSC 2430.

# CSC 2430 — P-Set: Magic Number

## Overview

In this problem set, you will build a small number-guessing game called **Magic Number**.

The program will begin as a very simple guessing game and will gradually become more flexible and interactive. Each problem builds on the previous one, so you should complete them in order.

Your final program will allow the player to:

- choose the range of possible numbers,
- choose how many attempts to use,
- guess a randomly selected magic number,
- receive feedback about how close each guess is,
- and recover gracefully from invalid input.

The goal of this P-set is not just to produce a working game. You should practice breaking a problem into smaller parts, tracing program behavior, using loops and conditionals correctly, and thinking carefully about edge cases.

---

## Learning Goals

By the end of this P-set, you should be able to:

- use console input and output,
- use variables and arithmetic expressions,
- write conditional statements,
- write loops controlled by counters and user input,
- generate random numbers,
- use `abs()` to calculate distance,
- validate user input,
- trace an algorithm by hand,
- and explain design decisions in your own words.

---

# Problem 1 — The Basic Magic Number Game

Start with a simple version of the game.

The magic number should be an integer between **1 and 10**.

For this first version, you may hard-code the magic number.

The player has exactly **3 attempts** to guess it.

### Requirements

Your program must:

- display a short introduction,
- ask the player to guess a number from 1 to 10,
- allow at most 3 guesses,
- stop immediately if the player guesses correctly,
- tell the player when a guess is incorrect,
- reveal the magic number if the player fails after 3 attempts.

Example interaction:

```text
=== MAGIC NUMBER ===

I'm thinking of a number between 1 and 10.
You have 3 tries to guess it.

Guess #1: 4
Sorry, that's not it.

Guess #2: 7
Sorry, that's not it.

Guess #3: 9
You got it!
```

If the player does not guess correctly:

```text
Guess #1: 3
Sorry, that's not it.

Guess #2: 8
Sorry, that's not it.

Guess #3: 1
Sorry, that's not it.

Game over! The magic number was 6.
```

### Before You Code

Write pseudocode for your solution.

Your pseudocode should clearly show:

- initialization,
- repetition,
- comparison of the guess to the magic number,
- and how the game stops when the player wins.

---

# Problem 2 — Trace the Algorithm by Hand

Before modifying the program, trace your algorithm manually.

Assume:

```text
magicNumber = 7
```

The user enters:

```text
3
9
7
```

Create a table similar to the following:

| Attempt | Guess | Correct? | Continue? |
|---:|---:|---|---|
| 1 | 3 | No | Yes |
| 2 | 9 | No | Yes |
| 3 | 7 | Yes | No |

Then answer:

1. How many times does the loop execute?
2. What causes the loop to stop?
3. What would happen if the user guessed `7` on the first attempt?
4. What values must the program keep track of while it runs?

---

# Problem 3 — Let the Player Choose the Range

Now make the game more flexible.

Instead of always guessing from 1 to 10, ask the player for the maximum possible value.

For example:

```text
Maximum number: 100
```

The guessing range is now:

```text
1 through 100
```

### Requirements

Ask the player for a value named conceptually something like:

```text
maxNumber
```

For this problem, require that:

```text
maxNumber >= 10
```

You may assume the user enters an integer.

The magic number must now be within:

```text
1 ... maxNumber
```

At this stage, you should generate the magic number randomly.

You may use:

```cpp
srand(time(nullptr));
```

and:

```cpp
rand()
```

A possible expression for the magic number is:

```cpp
rand() % maxNumber + 1
```

### Example

```text
=== MAGIC NUMBER ===

Maximum number: 50

I'm thinking of a number between 1 and 50.
You have 3 tries to guess it.
```

---

# Problem 4 — Let the Player Choose the Number of Tries

Remove the fixed 3-attempt limit.

Ask the player how many attempts they want.

Example:

```text
Maximum number: 100
Number of tries: 7
```

The game should now allow exactly that many valid guesses unless the player guesses correctly first.

### Requirements

The number of tries must be at least:

```text
1
```

Your loop must no longer contain a hard-coded value such as:

```cpp
3
```

for the number of attempts.

### Example

```text
Maximum number: 100
Number of tries: 5

I'm thinking of a number between 1 and 100.
You have 5 tries.

Guess #1: 50
Sorry, that's not it.

Guess #2: 75
Sorry, that's not it.
```

---

# Problem 5 — Add Temperature Hints

Now make the program tell the player how close each incorrect guess is to the magic number.

Compute the distance:

```text
distance = absolute value of (guess - magicNumber)
```

In C++, you may use:

```cpp
abs()
```

Because the range can be different for every game, the temperature categories should depend on the selected maximum value.

Use the following rules:

| Distance from Magic Number | Message |
|---|---|
| at most 5% of `maxNumber` | HOT |
| at most 10% of `maxNumber` | WARM |
| at most 25% of `maxNumber` | COLD |
| more than 25% | FREEZING |

Check the conditions from smallest distance to largest.

### Example

Suppose:

```text
maxNumber = 100
magicNumber = 42
```

Then the following guesses might produce:

```text
Guess: 95
FREEZING!

Guess: 65
COLD!

Guess: 51
WARM!

Guess: 45
HOT!

Guess: 42
You found the Magic Number!
```

### Important

Do not display a temperature message when the player guesses correctly.

---

# Problem 6 — Validate the Guess

Now make the program reject guesses outside the allowed range.

If the current range is:

```text
1 through 100
```

then:

```text
0
```

and:

```text
137
```

are invalid guesses.

### Requirements

If the player enters an invalid guess:

- display an error message,
- do not give a temperature hint,
- do not count the guess as an attempt,
- ask again.

Example:

```text
Guess #2: 145
Invalid guess. Enter a number between 1 and 100.

Guess #2: 60
COLD!
```

Notice that the attempt number remains `2`.

---

# Problem 7 — Debugging

The following code is intended to allow five guesses, but it has several problems.

```cpp
int tries = 5;
int guess;
int attempt = 1;

while (attempt < tries) {
    cout << "Guess #" << attempt << ": ";
    cin >> guess;

    if (guess = magicNumber) {
        cout << "You win!" << endl;
    }

    attempt++;
}
```

Identify and correct the problems.

For each correction, briefly explain:

1. what is wrong,
2. why it causes incorrect behavior,
3. how you fixed it.

Your corrected code should:

- allow exactly five guesses,
- compare values correctly,
- stop when the player wins.

---

# Problem 8 — Design and Reflection

Answer each question in 2–4 sentences.

### A.

Why is a loop a better design than writing the guessing code several times manually?

### B.

Why should an invalid guess not count as one of the player's attempts?

### C.

Why are percentage-based temperature ranges better than fixed distances such as:

```text
distance <= 2 means HOT
```

when the player can choose the maximum number?

### D.

Suppose the range is:

```text
1 to 1,000,000
```

and the player is allowed only 3 guesses.

Would the game still be reasonable or fair?

Explain your answer.

### E.

Describe one improvement you would add to the game if you were continuing the project.

---

# Final Program Requirements

Your final version should:

- randomly select a magic number,
- allow the player to choose the maximum number,
- allow the player to choose the number of tries,
- accept guesses from the player,
- detect a correct guess,
- stop immediately when the player wins,
- calculate distance from the magic number,
- display `HOT`, `WARM`, `COLD`, or `FREEZING`,
- reject guesses outside the valid range,
- avoid counting invalid guesses,
- reveal the magic number when the player loses,
- and produce clear, readable console output.

---

# Suggested Program Structure

Your program does not have to follow this exact structure, but you should be able to identify these phases:

```text
1. Initialize random number generator
2. Read game settings
3. Generate magic number
4. Repeat:
      read guess
      validate guess
      compare guess
      calculate distance if necessary
      display hint
5. Display final result
```

---

# Restrictions

For this P-set:

- use standard C++,
- use `cin` and `cout`,
- use conditionals and loops,
- use `rand()` and `srand()` for random numbers,
- do not use arrays, vectors, classes, or other data structures that have not yet been introduced,
- do not copy a complete number-guessing program from an online source or AI tool without understanding and being able to explain every part of it.

---

# Submission

Submit:

```text
magic-number.cpp
```

and a separate document or text file containing:

- your Problem 1 pseudocode,
- your Problem 2 trace,
- your Problem 7 debugging explanation,
- and your Problem 8 reflection answers.

Your source code should compile without errors and should be reasonably formatted and readable.

---
> Disclaimer: P-set generated with assistance of ChatGPT
