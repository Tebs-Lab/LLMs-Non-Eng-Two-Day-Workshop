# ChatGPT Doesn't Understand Randomness

Article: [https://blog.tebs-lab.com/p/gpt-doesnt-understand-randomness](https://blog.tebs-lab.com/p/gpt-doesnt-understand-randomness)

## At A Glance

The author used the OpenAI API to query GPT-3.5-Turbo to generate random numbers using a prompt in this format:

```
Generate [a number] of random numbers.
```

Using 10, 100, and 1000 in place of `[a number]`. GPT failed quite badly. Here are two general trends the author highlighted:

> GPT was bad at producing the correct quantity of numbers: It never correctly generated 1000 numbers, and it only generated exactly 100 numbers 3 out of 20 times. It correctly generated 10 numbers in 20 of 20 attempts.

and 

> GPT tended towards extremely uniform distributions — much more uniform than randomly drawing from a uniform distribution. In fact, When asked for 10 numbers, GPT never repeated a number in any of the samples. 

## Some Additional Considerations

In one example, the LLM produced the following repeating sequence until the API cut off the model for producing a response that was too long:

```
12, 34, 56, 78, 90, 23, 45, 67, 89
```

* Can you figure out where this pattern came from?
    * Hint: Remember this is a statistical language model...
    * Hint 2: look in the article for this pattern if you're stumped. 