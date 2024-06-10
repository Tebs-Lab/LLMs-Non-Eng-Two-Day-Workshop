# Prompt Engineering Tactics

This section is a collection of tips and tactics for getting better results from GenAI and LLM systems. 

Here's a great resource that has documentation and examples of all of these tactics: [https://www.promptingguide.ai/](https://www.promptingguide.ai/) (and more).

## General Tips

* Things are changing quickly, and as soon as really effective techniques are invented, they are quickly incorporated into model training, at which point they become obsolete.
* Because models are effectively "black boxes" the training manuals have to be produced by trial and error.
* Because models "hallucinate" it's critical to approach their outputs with reasonable skepticism.

* Because models train on truly massive datasets, they "know" a LOT of things.
    * But, that also means that domain knowledge is incredibly useful in getting AI's to generate quality outputs.

* Because the models are not "intuitive" you should specify clearly what results you want.
    * Most Generative AI systems are pedantic, so be detailed and specific.
    * Sometimes giving them more flexibility can be interesting, but usually it doesn't give you what you want.

* Prompting for things that are likely in the training data generally yields better results.
    * The more obscure and esoteric you get, the less likely the LLM is to do a good job.

* Iterative prompting with small changes usually works better than completely rewriting prompts.
    * If you get something close, but not quite, make one small change to get closer... then repeat.

### Remember to be skeptical!!

* Evaluate the outputs.
* Be especially wary of using GenAI in domains where you are not well versed.
* Check for common forms of social bias.
* Check for common misconceptions, statistical models internalize these.

### Be detailed in your prompts.

* Set explicit limits.
    * 500 words or less
    * Aspect ratios
    * 6th grade reading level
    * ...
* Ask for specific formatting
    * HTML
    * Bullet Lists
    * MLA Style Citations
    * ...
* Ask for specific styles
* Consider defining things like race, gender, ethnicity explicitly to avoid GenAI's inherent social biases.
* Long prompts work well, don't be afraid of using them.
* Utilize domain knowledge: 
    * GenAI's often know esoteric jargon.
    * They respond well to domain specific prompting.

### Embrace The Mimicry

* LLMs are good mimics, use phrases like...
    * "in the style of cubism"
    * "like an Axios article"
    * "inspired by Pixar"

### Be iterative.

* Many models allow you to generate variations, or ask for modifications.
* Working in steps can be easier than defining everything in one prompt.
* Sometimes small changes to a prompt can result in large changes in output.
* Don't be afraid to do some things manually. 
    * If you're a talented photoshop user, let AI start and add your own final touches. 
    * Let ChatGPT produce a rough draft or an outline, and you write the rest.

### Give the model samples first.

* e.g. upload samples of your own writing or your companies style and ask it to imitate that style.
    * Sample prompt: Analyze the text below for style, voice, and tone. Then write a new paragraph in 800 words in the same style, voice, and tone on Topic [Topic]. [Paragraph]


### Find Out What Works For Others!

* We're all learning more about how to better use these tools every day.
* Find other people who are using GenAI and ask them what works
    * Or look online for similar resources.

### Some Tasks that GenAI can help with

* Ask for edits or proofreading on your writing. 
* Ask it to find bugs in code, or write tests for a function
* Ask for a summary of input text.
* Generating custom images for articles, presentations, etc. 
* Generating slides for a slideshow
* Acting like a research assistant, generic Q&A (careful of hallucinations!)

## Chain of thought

**Note: Many LLMs have incorporated this technique into their training data and via RLHF, so you may not get much additional benefit on such systems.**

* Key idea: ask the LLM to show its work. 
    * Some research has demonstrated LLMs succeed better when it has to do things step by step, and that more errors are introduced when it does things "all at once".

* This can be done in a "few shot" or a "zero shot" way. 

Here's a "few shot" example from a 2022 paper:

Instead of asking:

```
True or False: The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
```

They give the bot some examples of answering in a step by step style, then leave the final question for the LLM to fill in:

```
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: Adding all the odd numbers (9, 15, 1) gives 25. The answer is False.

The odd numbers in this group add up to an even number: 17,  10, 19, 4, 8, 12, 24.
A: Adding all the odd numbers (17, 19) gives 36. The answer is True.

The odd numbers in this group add up to an even number: 16,  11, 14, 4, 8, 13, 24.
A: Adding all the odd numbers (11, 13) gives 24. The answer is True.

The odd numbers in this group add up to an even number: 17,  9, 10, 12, 13, 4, 2.
A: Adding all the odd numbers (17, 9, 13) gives 39. The answer is False.

The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 
A:
```

Here's a "zero shot" example. Instead of this word problem:

```
I went to the market and bought 10 apples. I gave 2 apples to the neighbor and 2 to the repairman. I then went and bought 5 more apples and ate 1. How many apples did I remain with?
```

Give the LLM the same prompt but add a tip to "think step by step:

```
I went to the market and bought 10 apples. I gave 2 apples to the neighbor and 2 to the repairman. I then went and bought 5 more apples and ate 1. How many apples did I remain with?

Let's think step by step.
```

## Meta prompts

**Note: Many LLMs have incorporated this technique into their training data and via RLHF, so you may not get much additional benefit on such systems.**

* Meta prompting is when you ask the LLM to make a prompt for you to use.
* I find this is especially useful in image generation contexts, for example:

```
Write a prompt to generate an image about the following:

Content: an elephant
Setting: the savanah
Style: digital illustration, HD
```

Yields:

```
Create a high-resolution, hyperrealistic digital illustration showcasing a majestic elephant standing in the middle of an expansive savannah. The image should capture the elephant flaring its wide, large ears against the sweltering heat. The detailed skin texture should embody the earthy grayish hue and exhibit the numerous wrinkles representative of its battle survival and age. Its long, curved ivory tusks should appear striking white, creating a significant contrast against its surroundings. The savannah should be portrayed as an infinite blanket of dusty, cracked earth with sporadic acacia trees. The sky should be colored a vivid, intense blue, with possible effects of heat waves rising from the ground. The golden, waving grasses and the merging of the horizon with the sky should be captured to represent the beauty and harshness of the Savannah. Manipulate shapes and forms, and features like layering effects to achieve a lifelike, realistic effect. The end result should be a vibrant, detailed representation of wildlife in its natural habitat, full of crisp, clear imagery.
```

Which yields:

![](assets/elephant.webp)

Whereas, if I simply prompt 

## System prompts

* System prompts give ChatGPT directions for the entire session.
    * Use them for stuff you know you always want the LLM to do.
    * This is especially useful when you're doing several samples of a similar task, like generating social media posts or a particular kind of writing.

* ChatGPT: use "system:" to make a system prompt, which tells the machine to use that for the whole conversation
    * System: Write like an axios article
    * System: Take the opposite view of claims I make, as if this were a debate
    * System: Write using language that is accessible at a 6th grade reading level
    * System: Act like a research assistant, give me details and citations about all the questions I ask.

* Other systems have similar capabilities, try to find documentation for the systems you use.

## Configuration Settings (ChatGPT Playground)



