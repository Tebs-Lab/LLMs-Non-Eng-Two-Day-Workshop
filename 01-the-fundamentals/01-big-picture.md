# LLMs: Foundations and the Big Picture

* Start with some probing questions, get the students to describe some specific things they already know.
    
* **So what are these ... Lets start with the name's components: "Large Language Model" ...**
    * **What's a "Model" in this context?**
        * Mathematical/Statistical model.
        * Takes input, produces output.
        * Each input/output session is (typically) independent.
        * a "language model" is a model focused on language data, rather than say weather data, financial data, image data, or anything else that could be modeled.
        * In very literal terms these models are a math function. 

    * **Let's try to contrast that with another common AI term: Whats the difference between a model and an agent?**
        * Models just take input and produce output.
        * Agents take actions over time in an environment.
            * Agents might use or be composed of one or more models.

        * Talk through a couple examples:
            * GPT-4 is a model, ChatGPT is an agent.
                * ChatGPT relies heavily on GPT-4 to read and write text.
                * ChatGPT produces text repeatedly in the chat environment.
                * ChatGPT has some memory of the various prompt/responses in a session.
                * ChatGPT can do additional things like perform web searches and execute code snippets sent to the service. 

            * Siri is a simple agent. 
                * "Set a timer for 10 minutes"
                * Speech to text model first, then text to possible actions model (likely an LLM). 
                * Finally some code that causes the actions to happen. (set timer, send text)

            * Self Driving Cars are more complex agents
                * Many models for processing the various inputs (visual, audio, LiDAR, etc.)
                * An agent then makes decisions about how to drive at each time step based on the various models.

    
    * **What about "Large?"**
        * These models size is usually measured by the number of "parameters" they have. 
            * *Goto the whiteboard* imagine an extremely simple model that predicts a person's weight from their height and age:

            `a*height + b*age = predicted_weight`

            * This model has two "parameters": `a` and `b`.
            * Modern "LLMs" have billions to trillions of parameters.

    * Finally, Language just means that the primary input and output datatypes are text representing some kind of language (English, Python, ... )
        * Generally the term "LLM" is used to mean models that take text and produce text, but the same models can be very slightly changed to take text input and produce different kinds of output like
            * Classifications: this text is 1-of-n possible outcomes:
                * binary: "this violates terms of service or not"
                * multiclass: sentiment analysis, "happy, sad, angry, ..." / some fixed set of emotions. This ticket represents an angry customer vs a satisfied customer vs a sad customer.
                * multi-label: multiclass but the model can output multiple classifications for a single output. This ticket is angry and violates terms of service.

            * Regressions: make a numerical prediction from text input.
                    * This review represents a 4.5/5 rating.


## Models, Agents, and Applications

* A final thing to keep in mind is the difference between an "agent" a "model" and an "application"

* *Reference the above about agents vs models if it didn't come up before*
    * *Recap it briefly if it did come up, and discuss "applications" and how that might be different*

* Models just take input and turn it into output.
    * It's highly transactional
    * Each input -> output sample is independent.
    * We're going to look at how models are built in significant depth in just a moment.

* Agents take action over time in an environment
    * Some agents are "trained," similar to how neural networks are trained
    * Others are more simplistic rules followers
    * They may incorporate one or more models.
    * Refer to the above examples (Siri, self driving, ChatGPT)

* Both "agent" and "model" mean something pretty specific to AI experts.

* Applications are far more general. Anything is an application.
    * ChatGPT's interface is an application that includes an agent
    * Game playing AI's are agents, but to actually enact the game itself (so a human can play too) there would need to be more application code.


* **Can you tell me a few things you know LLM's have been used for? OR examples of LLMs?**
    * Students should have tons of examples.
    * Use some probing questions about their examples:
        * *What portion of this is the 'model' vs 'agent' or 'application'?
        * *How risky is this use? What could go wrong? Why?*
        * **


* **Perhaps some of you have heard of some ways these models can fail, or cause problems?**
    * Again, there ought to be a lot of examples. Some big ones to discuss a bit:
    * *Hallucination**
    * *Bias, sexism, racism, etc...*
    * *Poor reasoning*
    * Again, for each, consider some probing questions:
        * *Why might this happen?*
        * *What sorts of input data could cause this issue?*
        * *How could you fix or mitigate this issue?*