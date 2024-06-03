# Augmenting Google Search With LLM Written Overviews

Article: [https://www.technologyreview.com/2024/05/31/1093019/why-are-googles-ai-overviews-results-so-bad/](https://www.technologyreview.com/2024/05/31/1093019/why-are-googles-ai-overviews-results-so-bad/)

## At A Glance

Google has started introducing "overviews," which are summaries of the linked webpage. These summaries are written by an LLM, and are based on the content of the link. And ... well here's a snip from the article:

> Unfortunately, AI systems are inherently unreliable. Within days of AI Overviews’ release in the US, users were sharing examples of responses that were strange at best. It suggested that users add glue to pizza or eat at least one small rock a day, and that former US president Andrew Johnson earned university degrees between 1947 and 2012, despite dying in 1875. 

## Some Additional Considerations

This system is a RAG system, but unlike previous RAG examples the vector database is — more or less — the entire internet. Or at least Google's index of the internet. This introduced some interesting failure modes. Here's another snip:

> In the case of AI Overviews’ recommendation of a pizza recipe that contains glue—drawing from a joke post on Reddit—it’s likely that the post appeared relevant to the user’s original query about cheese not sticking to pizza, but something went wrong in the retrieval process, says Shah. “Just because it’s relevant doesn’t mean it’s right, and the generation part of the process doesn’t question that,” he says.

And, a more concerning example:

> Melanie Mitchell, an artificial-intelligence researcher at the Santa Fe Institute in New Mexico, googled “How many Muslim presidents has the US had?’” AI Overviews responded: “The United States has had one Muslim president, Barack Hussein Obama.” 

> While Barack Obama is not Muslim, making AI Overviews’ response wrong, it drew its information from a chapter in an academic book titled Barack Hussein Obama: America’s First Muslim President? So not only did the AI system miss the entire point of the essay, it interpreted it in the exact opposite of the intended way, says Mitchell. “There’s a few problems here for the AI; one is finding a good source that’s not a joke, but another is interpreting what the source is saying correctly,” she adds. “This is something that AI systems have trouble doing, and it’s important to note that even when it does get a good source, it can still make errors.”

* With these two examples in mind...
    * Is it possible for LLMs to understand satire? 
    * How might you go about improving LLMs' conceptual understanding of non-literal training data?