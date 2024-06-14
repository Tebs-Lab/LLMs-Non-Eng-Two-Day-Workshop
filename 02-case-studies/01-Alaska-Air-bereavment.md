# Air Canada Airlines Chatbot Invents a Discount

Article: 
[https://www.forbes.com/sites/marisagarcia/2024/02/19/what-air-canada-lost-in-remarkable-lying-ai-chatbot-case/](https://www.forbes.com/sites/marisagarcia/2024/02/19/what-air-canada-lost-in-remarkable-lying-ai-chatbot-case/)

## At A Glance

An LLM-powered chatbot told an Air Canada customer wrong information about a "bereavement reimbursement." The airline does offer bereavement discounts, but not for travel that has already occurred; but the chatbot indicated that this customer was entitled to a reimbursement even though their travel had already occurred.

Canadian courts decided that Air Canada had to honor the ~$800 reimbursement.

## Some Additional Considerations

* This system actually *was* RAG powered, and the response to the user included a link to the real bereavement policy...
    * Why do you supposed the LLM hallucinated, then?
    * Could you change the way the RAG application works to prevent this, without modifying the LLM component?
