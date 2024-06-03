# What's an LLM, Actually?

*Instructor note: once again, build up to a larger diagram by adding these components one at a time.*

## Universal Function Approximation

* Before we talk about the specifics of an LLM, it's important important to know about the Universal Function Approximation Theorem.
* We will not discuss the details or prove the theorem.
* You need to know: 
    * If a function satisfies the requirements of the Universal Function Approximation theorem we call it a universal function approximator. 
    * A universal function approximator can be used to closely approximate *any function that could ever exist*.
    * You can change *which* function the function approximator approximates simply by changing the parameters
    * Therefore, the gradient descent process -- which we use to update the parameters of our neural network -- can be thought of as a process for *searching for and finding a function that maps our input to our output successfully*

* We **always** choose functions for our neural networks that satisfy the UFAT.
* Training a neural network is best thought of as a search for a useful math function.

## LLM Components

* It's critical to remember throughout this that *every one of these components* has learned parameters that change and update through the gradient descent process. 

* Neural Networks are always composed of a series of "layers" 
* Each layer does some kind of mathematical transformation on the output of one or more of the previous layers.
* Layers are basically chosen for n reasons:
    * They transform the output into a useful "shape" with useful properties
        * For example, an LLM's final layer's output has the following properties:
            * 1 number per token in the model's vocabulary
            * Each number is between 0-1
            * All the numbers sum to 1
            * i.e. It's a probability distribution of the words in the model's vocabulary.

    * They accept inputs that we think are relevant to the predictions we want to make
        * For example, in an LLM the "attention" layer takes several words all at once in order to predict the next word.
            * Obviously, the first 6 words in a sentence would be useful when predicting the 7th word.
    
    * They are efficient to compute on specialized hardware. 
        * This aspect is often overlooked by the general public
        * It might also be the most important aspect of them all. 
        * Every major breakthrough in Deep Learning since 2012 has been a layer that does has one of the first two properties, but does it much faster than the previous SOTA method. 
        * Being able to train more rounds in a fixed amount of computational cycles is a *huge* win in neural network research.

### Pre-processing

* Data being fed to neural networks generally undergoes a "preprocessing" step.
* For language models, the most important "preprocessing" step is called "tokenization"
* This turns text into a series of "tokens" which are the smallest unit of data that an LLM will individually process.

* There are multiple competing tokenization schemes with different strengths and weaknesses.

* Whole Word Tokenization
    * Gives the embeddings layer a really good chance of learning semantically useful things.
    * Easy and intuitive for humans to understand
    * **Can you think of any strengths or weaknesses of this approach?**
        * Leads to a really large vocabulary size
            * Every conjugation, suffix, prefix, tense, etc. of a word gets its own token
                * look, looks, looked, looking, looker... 
        * Only works well for languages with clear word boundaries in their textual form
            * English does (spaces) Japanese does not. 
        * Creates problems with "out of vocabulary" words, like proper nouns or slang you didn't encounter during training.


* Word Part Tokenzation
    * instead of each word getting a token, works are broken into relevant grammatical subcomponents. Common prefix and suffix like re- de-, -ed, -ing, -er get their own tokens along with word roots.
    * **Micro Exercise: Can you think of any strengths or weaknesses of this approach?**
    * Similar advantages to whole word tokenzation
    * But makes the vocab size much smaller.
        * Look, gets one token, and common suffixes such as er, ed, ing, s get their own tokens
        * Looking --> look+ing.
        * So all gerunds now share the one -ing suffix token
    * Makes some individual words into multiple tokens
        * This can make the learning process harder in some cases and easier in others.
        * e.g. plurality might be easier to learn since the `s` can just encode that meaning
        * e.g. in English hook + er, the -er suffix changes the meaning of the word hook in a totally different way than it changes the meaning for other words. 
    * Requires specific knowledge of the language and its grammar, which also makes the "tokenization" process more difficult to implement.


* Byte Pair Encoding
    * This is a totally different strategy which is 100% agnostic to the grammar of a language
    * Basically, programmers specify a fixed vocabulary size (call it n) and the BPE algorithm identifies the most common combinations of characters, building up to a vocab size of n.
        * The result is that really common words end up getting their own token, less common words end up being represented by multiple tokens.
    * **Micro Exercise: Can you think of any strengths or weaknesses of this approach?**
        * Which is a strength and a weakness.
        * Can be used to parse any kind of text (english, Python code, Chinese... anything)
        * Allows programmers to specify a fixed vocabulary size.
        * Almost always solves the 'out of vocab' problem because the tokens are flexible enough to represent any combination of text. 
        * Fundamentally disconnects tokens from grammar, relying on the model to figure out how some combinations of tokens end up meaning the same thing.

* Demo the GPT tokenizer: https://platform.openai.com/tokenizer

Some interesting words for the tokenizer:
```
seashell
trees
tree
trepidation
saddle
saddles
trip
trips
tripping
```

* **Micro Exercise: Everyone spend about 2 minutes playing with this tokenizer. Type a few sentences, look at how they are tokenized. Afterwards, open it up for anyone to share anything interesting.**

### Embeddings -- make numbers from words

* Every token is already known from pre-processing.
* Each token is assigned a set of random numbers (in a vector)
    * Typically the dimension is in the low thousands
    * 4096 in LLaMA 3
    * GPT-3 has 12288
    * GPT-4 reduced to ~4000 (according to 'reports')
* These numbers are learned as parameters as the model is trained.
* Each of these numbers can be thought of as encoding some portion of semantic meaning.
    * Although because of how it is learned, it's not generally possible to parse that meaning. 
    * Sometimes it is, though, and some of the fields have clearly discernible meaning.
    * **Micro Exercise: Think of some forms of 'semantic meaning' that could be numerically encoded.**

### Attention

* Two kinds, self attention and cross attention
* Typical LLMs use self attention.
* Models that need to compare 2 different pieces of text use cross attention (e.g. translation)
* Intuitively, attention creates a 2D matrix.
    * One row and one column per word (the row and column labels are the same words for self attention)
    * Each cell in the matrix represents an association between the row-word and the column-word
    * Larger values imply stronger association.

* Attention layers have a fixed "context window" which is the maximum number of tokens that can appear in the row's and columns.
    * LLaMA 2 has a context window size of 4096
    * GPT-4's largest version has a context window of 128k (!!!!), smallest is 8k

* Attention layers are typically "multi-headed"
    * This means each attention layer has more than one of these matrices
    * Each one learns different "parameters" which control the associations between tokens. 
    * Intuitively... 
        * One matrix might learn to associate pronouns with their respective nouns
        * Another might learn to associate adjectives with their respective noun
        * Another might learn to associate opening quotations with closing quotations...
        * Not every relationship will be grammatical, or even interpretable, though.

* These associations are used to produce an output matrix which is the same shape as the input matrix
    * Specifically, one vector for each token in the context window.
    * These outputs are best thought of as an "attention weighted embedding"
        * Each token still has a single embedding in the output
        * However, tokens with high associations (as measured by the attention matrix) are effectively merged
            * If the words Queen and Hearts are strongly associated in a given sentence, the new "attention weighted embedding" for Queen will have incorporated lots of semantic values from the "Hearts" embedding, and vice versa.

* Fun fact, this architecture replaced something called "Recurrent Neural Networks" which had a different mechanism for associating words with each other. 
    * There are a lot of differences, but the most important one is that Attention is several orders of magnitude faster to compute.

* **Micro-Exercise: Think of some relationships between words that attention might model.**


### Multi-layer Perceptron / Dense / Fully Connected / Feed Forward

* FF layers are the most "vanilla" version of the Neural Network paradigm, and were the first to be invented.
* Nevertheless, they are also the layer type that has the highest number of parameters for most models. 
* *Draw a small one of these*
* In general: FF layers take every piece of input and associate it with every other piece of input, while producing a specific number of outputs chosen by the architecture designer.

* Modern LLM's use 2 versions of the feed forward layer one within every "attention block" and another at the very end, which serves a different purpose.

#### Attention FF block

* The purpose of this block is to take the context-updated embeddings and associate every single one of those embeddings with each of the other embeddings on a per-token basis.
* The input is one embedding-length vector per token per head of attention.
* The output is one embedding-length vector per token.
* The purpose of this layer is to compress the output of each "head" of attention for each token into a single value.
* Each token is processed *independently*!
* You can think of these output values as a combination of all the various "attention contextualized embeddings"
    * As we add more and more layers of attention these values become more and more abstract "combinations of combinations" which, we hope, allows them to take on specialized meaning in context
    * Consider two sentences: 
        * The Queen of Hearts was played in the 4th hand
        * The Queen of Hearts called Dorothy to "my pretty."
        * as we get further and further though the layers, more and more context gets added and incorporated to into the embedding associated with "queen" 
            * In the first, the model should learn it's one of 52 cards in a card game.
            * In the second, the model should learn it's a character in a story.

* **Micro-Exercise: Can you think of a phrase that, similar to "queen of hearts," has completely different meaning depending on the context?**
    * *Hint that idioms are a great example of this...*

#### The Prediction FF Block

* Thankfully this one is easier to understand. 
* Input shape is the output of the final attention layer (i.e. one ultra-contextualized embedding per token)
* Output shape is the number of tokens the model knows (the vocabulary size)
* This layer is constructed in a special way such that the all of the output nodes are assigned a value between 0-1 and all the nodes together sum to 1
    * i.e. it's a probability distribution

* This layer takes all the contextualized token embeddings and produces a prediction of which words are the most likely to come next, given that input. 
* We sample the most likely word, and repeat this process to generate more words.

* **Discussion question: How does the choice of tokenization scheme relate to this final layer?**


### Retrieval Augmented Generation

* RAG is a mechanism to use LLMs more like a search engine than strictly a text prediction system.
* There are 2 key differences from the above
    * First, you have to incorporate a second model, the "embedding model" which takes documents and produces an embedding vector for the entire document.
    * Second, use that model to create a "vector database" which maps these embeddings to the raw documents that are relevant to your system (say internal help docs, or a set of laws).
    * Finally, you have an LLM and some application code that allows the LLM to produce raw snippets or links to relevant documents.
        * User queries are run through the embedding model and then vector similarity search finds relevant docs in the vector DB

* The embedding model is also a neural network, typically with very similar architecture to an LLM, but with a few changes:
    * The final layer's output shape changed from a "probability distribution over the vocabulary" to whatever embedding shape is desired.
    * The context window has to be large enough to read the whole document, or the model has to be modified to consume the whole document in a series of steps before producing the final embedding
    * Finally, the training process has to change to fit the task. There are multiple approaches, a couple examples:
        * Manually label documents with a "similarity" score, punish the model for producing very different embeddings on similar documents.
            * Automate this process some by making small perturbations to a single document and labeling them as high similarity...
        * Build the embedding model into another model by adding a few more layers to do something like a sentiment analysis model, then remove the layers following the embedding model.
        * You'll also end up doing something like "instruct" tuning by mapping raw documents to a set of prompts that should reasonably map to that document.

* **Why do this?**
    * *Reduce hallucinations*
    * *Deliver reliable information*
    * *Replace classic "search" capabilities*
    

