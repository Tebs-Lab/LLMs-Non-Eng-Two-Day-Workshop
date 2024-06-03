# How Do Models Learn?

* *Instructor Note: Go To The Blackboard if you're not already there*
    * *Diagram this process step by step.*


* For now I want you to think of LLMs as a really fancy math function.
    * This is literally true, and we'll do some detail in a minute. 
    * For now just pretend that each word is represented by a set of numbers, where each number represents a score for a particular kind of meaning.
        * e.g. "Queen" is high on royalty, femininity, and noun-ness, it's low on plurality and poverty
        * e.g. "Cat" is high on animal-ness and sneakyness, low on humanity, and compassion
        * (This is called an embedding, more on this later)


* Our model:
    * Takes input as a list of these word vectors (so a 2D matrix, where each row is a vector representing a word).
    * Outputs a probability distribution of every word it knows. 
    * To select a word we generally sample from that distribution, but we might also just select the most likely word
    * To get multiple words of output, we append the predicted word to the input and repeat that process over and over. 


* First I'm going to tell you a very useful mental model that is mostly true, but vague. 
    * Every time the model makes a prediction, we already know what the "right" next word should be (this part is true, that comes from the training data)
    * If the model picks the right word, we don't change anything about the model. 
    * If it's wrong, we "punish" it by changing it's parameters.
        * The more wrong it is, the more we change the parameters. 
    * But... we can't just change the numbers randomly and expect to get good results. We need (and have) a rigorous process that is likely to adjust the parameters in a way that improves them!
        * **How do we do that?!**


## Calculus Based Optimization

* Okay, we have to take a small tangent.
* If you ever took a calculus class, you learned that the derivative can be used to tell you the slope of a function at any given point. 
    * *instructor note: draw a parabola* 
    * So, we can use calculus to select any point on this function, and it will tell us the slope.
    * *pick a point where the slope could be ~2*
    * This slope also tells us if we increase x by 1, it will increase y by ~2, and conversely if we decrease x by 1 it will decrease y by ~2.
    * Imagine we in fact decrease x by 1, y decreased, and we're at a new point on this parabola.
    * If we repeat this process over and over again, we'll take small steps towards the bottom of the parabola
        * If we overshoot, the derivative tells us to go in the other direction.
    * This algorithm is called Gradient Descent, and it's the fundamental underpinning of all neural networks.

    * **Can anyone think of some ways this can go wrong?**
        * *local minima / maxima will be a common example.*
        * *Thrashing back and forth with big steps is another.*
        * *Complex functions with erratic changes in slope*
        * *But do impress upon the students that this strategy IS what is REALLY used, so it seems to work quite well, any problems aside.* 
        * *Maybe mention that there are "advanced" versions of gradient descent that mitigate some of these issues.*

    * **CRITICAL TAKE AWAY:** Calculus can be used to find the "minimum" of any differentiable function. 
        * (the "analytical" solution isn't computationally feasible for very complex functions)
        * Gradient Descent is an iterative method to approximate that minimum value, which is more computationally feasible. 

* So, as long as our model is a differentiable function we can use this method.
    * All we need to do is augment it a little bit to make it a function we want to minimize.
    * The "trick" to do that is to use a "loss function" which quantifies how right or wrong our model is for each guess. 
    * We use loss functions that take both the output from our model, and the "true" answer (called the label) to produce a number that is 0 if the model was perfect, and gets bigger the more wrong the model is
    * Just to make things concrete, we usually pick a function called "Categorical Cross Entropy" which compares probability distributions
        * The label is encoded as a distribution where the right answer is 1.0 and all other values are 0.
        * Our model produces a more nuanced distribution.
        * CCE looks at our models output value for the correct word.
            * The smaller our models number, the larger the "loss" or error, and vice versa.

* **The fundamental hypothesis of LLMs is that we can find a math function which maps input text to output text in a way that is fundamentally useful/meaningful... To what extent do you buy that hypothesis?**
    * This is a good way to start some arguments, which are valuable.
    * "The proof of the pudding is in the eating" though, LLM's are clearly useful to some extent, so the hypothesis must be sound to the same extent. 
    * "All models are wrong, some are useful"
        * The model doesn't have to represent some kind of fundamental truth about writing or language or grammar to be useful!


## Stages and Types of Training

* LLMs typically undergo at least two stages of training.
    * Pretraining
    * Fine-Tuning

### Pretraining

* Done on crazy huge amounts of data.
    * We're talking literally every book that's ever been written.
    * Every NYTimes article that's ever been published
    * Every reddit comment thats ever been made
    * I'm not exaggerating, they've harvested EVERYTHING the internet has to offer.

* Then they take this huge corpus and just do "next word" prediction over and over and over again. 
* This process can take literally months on a supercomputing cluster.
* This explains why most of the major advances in "Deep Learning" are about how to make this process faster and better optimized for parallel computation.

* **After this process, would you expect an LLM to be good at answering questions?**
    * *The right answer is no, but let the class wrestle with it a bit, probe:*
    * **Why? Why not?**

### Fine Tuning

* After your model has pre-trained it knows "language" in a general way.
    * But specifically only how to predict the next word given a list of words. 
* Models can also be fine tuned to do next word prediction but in a specific style.
    * By reading only scientific papers, to write like a scientist.
    * By reading just legal documents, to write like a lawyer.

* To do specific tasks, like question / answer formats it needs to be fine-tuned to do exactly that.
    * Gemini, ChatGPT ...

* Or fine tuning can be used to incorporate or prioritize specific knowledge, for example:
    * By reading internal documents, to gain knowledge about the specific company.
    * Etc.

* The fine-tuning process is generally still gradient descent based, but might impose additional constraints... for example:

#### Instruct Tuning

* During pretraining, models read and make a prediction after every single word. Consider this passage:
    * "The boy and his dog ran into town to fetch a loaf of bread."
        * The model sees "the" then predicts, 
        * then "the boy" then predicts,
        * then "the boy and" and predicts ... and so on.

* With instruct tuning, models take as input only complete instruction prompts and only start producing output after the entire prompt. Consider the prompt:
    * "Write a story about a boy and his dog." 
    * The system needs a "correct" answer that follows this prompt to instruct train.
        * (Companies are actively paying people to produce answers to prompts)
        * For this example assume the previous example is the "right" answer.
    * Then, to train, the model gets "Write a story about a boy and his dog." and hopefully predicts "The"
    * Then it gets "Write a story about a boy and his dog. The" and predicts "boy"
    * Then it gets "Write a story about a boy and his dog. The boy" then predicts... and so on.

#### Reinforcement Learning With Human Feedback

* Reinforcement learning is another type of machine learning.
* Applying RL to LLMs is done to help the models in two critical ways:
    * Incorporate human feedback about the quality of the models outputs
    * Reinforcement learning is better for "agents" because it generally allows for a notion of taking actions over time, whereas the "(semi)supervised learning" process we just described does not. 
        * So agents like ChatGPT benefit from RLHF

* RLHF works by asking LLMs to produce multiple outputs to the same prompt, and having humans rank them.
    * **How can the model produce different results to the same prompt?**
        * The answer is sampling from the probability distribution.
        * The point of this question is to get folks thinking about the deterministic nature of the actual math function powering the models.
