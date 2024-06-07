# Different Interfaces For Using LLMs

There are many different ways to interact with LLMs. From building and training a bespoke system entirely from scratch to the ChatGPT web interface. The various options have different benefits and drawbacks. This research task is about sussing out the differences between these options, as well as determining what organizations and products are most relevant in each category. 

The following sections are organized from "low level" to "high level" -- where low level means closer to the raw engineering and high level means more levels of abstraction. As a general rule, very "low level" options are only accessible to engineers and very "high level" options are accessible to just about everyone. 

One heads up, the following terms are poorly defined, have multiple definitions, and are also sometimes used interchangeably. It sucks, but there's no way around it. Just keep that in mind as you perform this research.

* Application Program Interface (API)
* Source Development Kit (SDK)
* Software Library (or just library)
* Software Module
* Software Package
* Framework

## Raw Models

### 1) Tools For Working With Raw Models

It's extremely uncommon for any engineers to actually develop LLM code completely from scratch. Instead, we normally use existing software libraries designed to do some portion of the task. The three most popular such libraries are:

* Tensorflow
* PyTorch
* JAX

Compare and contrast those three libraries, and specifically consider the following:

* Who maintains the library?
* How popular is the library?
* Are there any prominent companies or organizations using or supporting the library?
* Why might an engineering team prefer one over another?

Another popular LLM framework called LangChain is substantially different in purpose and function, but is nevertheless an "LLM Library."

* Compare and contrast LangChain with the other three libraries. Specifically consider:
    * What capabilities does LangChain offer that the other libraries do not?
        * And vice versa.
    * Under what circumstances might an organization use LangChain over TF, PyTorch, or JAX?


### 2) Options When Using Raw Models

When using raw models, engineering teams have roughly 3 options:

1) Built it entirely from scratch.
2) Use a pre-made model, but train it some (either fine-tuning or train from scratch).
3) Use a pre-made pre-trained model, with no additional training.

Considering option 1:

* Which prominent companies are building these so-called "foundation models"?
* Are there any interesting philosophical differences between these various companies and how they build their models?
* What are some advantages and disadvantages to this strategy?

Considering options 2 and 3:

* When and why might an organization choose not to perform any additional training?
* Can you identify some organizations who offer pre-built and pre-trained models to be used this way?
* What are some advantages and disadvantages of these two strategies?

## Web APIs

Web API's allow developers to use LLMs without building or hosting one themselves. Typically these interfaces allow developers to send a web request with specific formatting and requirements and receive a response. In the case of an LLM API a request might contain the prompt, and the response will contain the LLM's output for that prompt.

For more background of Web API's in general, [read this](https://medium.com/@TebbaVonMathenstien/what-is-an-api-and-why-should-i-use-one-863c3365726b)

* Can you identify at least 3 organizations providing Web APIs powered by LLMs? 
    * Try to identify their pricing structure.
    * Try to identify which specific models are "exposed" by the API.
    * Try to identify what configuration options are available to developers using the API.

* Describe how an engineer would use a web API to integrate an LLM into their own applications.
    * What portion of the code does the engineer write?
    * What portion is handled by the API?
    * What constraints does this place on the developer?

* What are some advantages and disadvantages of using Web APIs?
    * First, answer this question as a user of the API.
    * Second, answer this question as the developer/publisher of LLMs.

## End User Facing Applications

User applications come in many forms, but in every case they are designed to give users a way to directly interact with LLMs without any engineering effort.

* Compare and contrast the following three applications:
    * OLLaMA
    * LM Studio
    * ChatGPT
    * What models do they each expose?
    * How are the actual user interfaces different?
    * When might you prefer one over the other?
    * Are there any special capabilities that some of these applications do and do not offer?


* As a publisher of LLMs, why might you prefer to publish a user interface over a web API or your raw models?
    * (There's a lot to consider!!)

* As a consumer of LLMs, what are some advantages and disadvantages to using an application over a web API or raw models?
    * (There's still a lot to consider!!)