# Automatically Modifying Prompts For Safety

Two articles this time:

Safety: [https://www.theverge.com/2024/2/21/24079371/google-ai-gemini-generative-inaccurate-historica](https://www.theverge.com/2024/2/21/24079371/google-ai-gemini-generative-inaccurate-historica)

Copyright: [https://blog.tebs-lab.com/p/intellectual-property-vs-artificial-intelligence](https://blog.tebs-lab.com/p/intellectual-property-vs-artificial-intelligence)

and [https://blog.tebs-lab.com/p/automated-copyright-infringement](https://blog.tebs-lab.com/p/automated-copyright-infringement)

## At A Glance

In the first article, Google's Gemini created controversy when it started automatically capturing and rewriting prompts for its image generation subsystem. Specifically, when a prompt asked for images of people to be generated, Gemini automatically modified the prompt to include randomized demographic information.

This was done to address previous allegations of bias. For example, a [Bloomberg study](https://www.bloomberg.com/graphics/2023-generative-ai-bias/) found that if you asked similar systems to produce a picture of a "doctor" it was almost always a white man, but if you asked for a "housekeeper" it was almost always a woman of color. 

The controversy came when this prompt filtering and rewriting system caused Gemini to rewrite a prompt asking for "images of Nazis" to include a racially diverse cast of Nazis, which is... a-historical to say the least. In an unexpected twist, the system also refused specific queries to generate an image of "a white nazi" for a time, on grounds that it might be insensitive or offensive to do so.

Similar filtering and rewriting occurs when ChatGPT receives requests it thinks will violate copyright law -- although as the author of the articles found, those systems were easy to get around in many cases.

## Some Specific Considerations

* How do you think these filters and prompt replacement systems work?
    * Hint: it's turtles all the way down...

* Why might system designers try this tactic, rather than attempting to eliminate the problems at the level of the underlying LLM system?
    * Try to answer this question separately for copyright and social bias. 