#### Autoencoder vs Autoregressive models
**PENDING**

Other pending items:
- Model hyperparameters (temperature, generations, likelihood, top-p(nucleus sampling), perplexity)
- Reinforcement learning with human feedback

#### LLMs as prediction engines
Source:  [3 principles of prompt engineering](https://www.linkedin.com/pulse/3-principles-prompt-engineering-gpt-3-ben-whately)
First up, it’s good to have a mental model of just what GPT-3 is. At its simplest, it is a massive prediction engine. It’s been trained on almost all the text on the internet - trillions of pages. It is trained to predict, “if this text came first, what word comes next?”

LLMs capabilities in a nutshell ([source](https://medium.com/data-science-at-microsoft/building-gpt-3-applications-beyond-the-prompt-504140835560)): 
![](https://miro.medium.com/v2/resize:fit:1400/1*Z85QtAyLRH-stR_zOFbEIQ.png)

Prediction ([Source](https://txt.cohere.com/generative-ai-part-1/))
![The model assigns a likelihood number to each of all possible next tokens](https://lh5.googleusercontent.com/I85lG5voUi3KZyIa4IveWMjMwyuC2MrqJBXYOqZoduo39GBwrt7ExylGijW5MoUNZS_hdZQ01CrZkSXx_CvX4XIcaQKVcAda2FdW6DDWY0PQsy5eqT_5HRscuInh2S3F1W6q795p427PEsKWbi2RMrMWTtBmogXhg4n2KaAFqqndPLEkTSq0i2-0yWiP-w)

Prediction 2 ([Source](https://txt.cohere.com/generative-ai-part-3/))
![Each generated token has an associated likelihood value | 500](https://txt.cohere.com/content/images/2023/02/Token-Likelihoods.png)

### GPT family
Source: [ Building GPT-3 applications — beyond the prompt](https://medium.com/data-science-at-microsoft/building-gpt-3-applications-beyond-the-prompt-504140835560)
Please note that GPT-3 is, in reality, a family of models derived from the “[original GPT-3](https://arxiv.org/abs/2005.14165).” There are now variants (and subvariants) that bring different properties, such as being lighter (curie or babbage), responding better to instructions ([InstructGPT](https://openai.com/blog/instruction-following/)), specializing in program generation and comprehension ([Codex](https://platform.openai.com/docs/models/codex)), or being an overall better version (GPT-3.5).
The now very popular ChatGPT system (and underlying model) is close to InstructGPT [according to OpenAI](https://openai.com/blog/chatgpt/), but at the time of writing there are few additional technical details.

#### Goal, task, and domain
Source: [Generative AI with Cohere: Part 2 - Use Case Ideation](https://txt.cohere.com/generative-ai-part-2/)
While the applications of LLMs are broad, the key to ideating a new one is to get specific about what you want to achieve. And to be able to do that, it’s helpful to understand where and how LLMs can be applied.

There are a number of ways that you can look at it, and here we propose three:
-   **Goal**: The _“why”_ — the problems for which LLMs are well-suited
-   **Task**: The “_how”_ — the tasks LLMs can perform, or how you get to the goal
-   **Domain**: The _“what”_ — the domains or industries in which LLMs can be applied

##### Goal
![The “why” — the kinds of goals that LLMs can help fulfill | 500](https://txt.cohere.com/content/images/2023/01/1-goal.png)
_The “why” — the kinds of goals that LLMs can help fulfill_

Before going into prompt creation, a good place to start is to identify the goal, that is the problem you want to solve. Focusing our attention on the problem to solve brings clarity to the potential use cases to be developed.
At the same time, not all problems can be solved by LLMs. So, it’s useful to understand the kinds of problems (and goals) well-suited for LLMs.

The following are some example areas where LLMs are especially helpful:
-   ***Automate repetitive tasks:*** Producing outputs on a consistent basis with a certain format and quality. Example use cases: producing ad copy, creating product descriptions, extracting phone numbers from text.
-   ***Accelerate writing:*** Writing down the first draft, or even the final version of a piece of text. Example use cases: composing emails, writing blog posts, providing customer chat responses.
-   ***Brainstorm ideas:*** Generating a skeleton of a bigger piece to be worked on, instead of working off a blank canvas. Example use cases: generating article outlines, finding business ideas, writing story plots.
-   ***Augment a skill:*** Augmenting the skill of a writer who might not have sufficient proficiency. Example use cases: writing poems, writing fiction stories, formulating product pitches.
-   ***Condense information:*** Getting a summarized version of a document that strips it to its essence. Example use cases: summarizing reports, articles, and podcast transcripts.
-   ***Simplify the complex:*** Rewriting a piece of text into a simpler, more accessible way. Example use cases: simplifying technical explanations, understanding complex text, extracting key concepts from a passage.
-   ***Expand perspectives:*** Adding variety to the voice and idea beyond just the person writing. Example use cases: generating opinions in essays, constructing arguments in debating, adding variety in speech scripts.
-   ***Improve what’s available:*** Turning a piece of text into a better version. Example use cases: correcting spelling errors, making a passage more coherent, rewriting podcast transcripts.

##### Task
![The “how” — The kinds of tasks LLMs can perform | 500](https://txt.cohere.com/content/images/2023/01/2-task.png)
_The “how” — The kinds of tasks LLMs can perform_

While the previous section looks at the kinds of goals you can achieve with LLMs, now we’ll look at _how_ LLMs can help you achieve those goals.

The following are the different kinds of tasks that LLMs can perform in the generative AI space.
-   ***Write:*** We start with the most obvious category, which covers most of the generative AI use cases out there. We can further break down this category into a few ways that you can leverage large language models. For example, an LLM can continue on a piece of text that you have provided as a prompt (see [Prompting by Example in Part 1](https://txt.cohere.com/generative-ai-part-1/) of this series). It can also respond to an instruction, be it a command to write something or a question that asks for an answer (see [Prompting by Instruction in Part 1](https://txt.cohere.com/generative-ai-part-1/) of this series). It can also expand a set of keywords or sentences into a longer piece of text.
-   ***Rewrite:*** While writing is about generating something from scratch, with rewriting, the model can take an existing piece of text and generate an alternative version. We can further break down this category into a few areas, such as rephrasing a piece of text, correcting errors found, or removing certain information from text.
-   ***Converse:*** This category is about using LLMs to generate conversations. One example is creating a chatbot that answers questions, and another is writing screenplay dialogue. Earlier, we talked about LLMs providing answers to questions, but here the context is slightly different. With conversations, the context expands to the previous few lines of a dialogue, and it’s not just a one-off question-answering scenario.
-   ***Summarize:*** This is similar to rewriting in the sense that there is an existing body of text to be worked on. In this case, it’s about generating a shorter version of the original text while maintaining the essence of the message.
-   ***Extract:*** Similar to rewriting and summarization, extraction also is applied on existing pieces of text. Here, the goal is identify certain types of information available in the text, for example, a date from an invoice or the names of the people involved in a story.

##### Domain
![The “what” — The domains and industries LLMs can be applied in | 500](https://txt.cohere.com/content/images/2023/01/3-domain.png)
_The “what” — The domains and industries LLMs can be applied in_

So, we’ve looked at _why_ you want to use LLMs and _how_ to achieve those goals. Now, we look at the final piece, which is the _what_ — the domains where you can apply them.

LLMs are making an impact in the generative AI space across many industries. But at a high level, there are a few broad themes emerging:
-   ***Copywriting:*** Copywriting is the act of writing text for the purpose of advertising or other forms of marketing. The text is designed to persuade the reader or listener to take some form of action, such as making a purchase or subscribing to a service.
-   ***Business Writing:*** Business writing refers to the skills and conventions of writing in the business context. It covers anything related to writing in the workplace and includes documents, memos, letters, and emails. The format and style of writing could differ depending on the situation, such as with clients, vendors, customers, or colleagues.
-   ***Creative Writing:*** Creative writing is any form of writing that expresses thoughts, feelings, and ideas in an imaginative way. It is a way to express oneself creatively and can be used for entertainment or as a form of self-expression. Creative writing can be used to tell stories, create characters, or explore new ideas.


You can also go into specific verticals or industries, each presenting huge opportunities for applying LLMs. For example: Marketing, Productivity, Gaming, Education, Wellbeing, Customer Support, Human Resources, News, Healthcare, E-commerce, Travel, Consulting, Sports, and more.


#### Model hyperparameters
Source: [Generative AI with Cohere: Part 1 - Model Prompting](https://txt.cohere.com/generative-ai-part-1/)
-   `Temperature` — is how the model chooses from its next choice of tokens. Lower temperature will cause the model to output text that is more predictable, while higher temperature means that the output will be more creative. It is a number between 0 and 2, and in most cases, somewhere between 0 and 1 works fine.
![Increasing the temperature makes generating tokens with lower likelihoods more probable, and vice versa. | 500](https://txt.cohere.com/content/images/2022/12/temperature.png)

![Trying out different temperature combinations | 500](https://txt.cohere.com/content/images/2023/05/generate-temp-variations.png)
Image 2: [Source](https://txt.cohere.com/generative-ai-part-3/)

-   `TOP-K` — is the list in which the model can make its choices from. The default is 0, which means the model will consider all possible next tokens out of the thousands of possible tokens. If you change to any other number, of say 100, the model will only consider the top most probable tokens, limited by the number you define.
![Increasing the top-k increases the number of tokens the model can choose from, and vice versa. The same concept applies in top-p, but uses probabilities instead of count. | 500](https://lh3.googleusercontent.com/FH_OwcixEP7gqKcQl5g6xMA-RZVYv0ZPitlFB_Iw8oqxK-qj-wifT6sooqFF7fBtnJLJE9XKU5GGdl00uPrj6RZHaoETS3WmmGiG9bjLDZvQmnTTSJxOA_1sLf2SjyOnbmkHsPR4KpGFgWV_MDDinz5yXPPuZp0cks877In-UPS7ZsnxcC3YDXq_bPWFew)

#### Prompt
Source: [3 principles of prompt engineering](https://www.linkedin.com/pulse/3-principles-prompt-engineering-gpt-3-ben-whately)
You feed GPT-3 with a prompt - written in plain English, not in code - and it prints out its prediction of the words that should be written after that prompt. The prompt is literally just a request written in normal, natural language, asking it want it to do.
input → more programmatic term
prompt → is input but given in plain English

#### 8 things to know about LLMs
Source paper: [here](https://arxiv.org/pdf/2304.00612v1.pdf)
1. LLMs predictably get more capable with increasing investment, even without targeted innovation.  
2. Many important LLM behaviors emerge unpredictably as a byproduct of increasing investment.  
3. LLMs often appear to learn and use representations of the outside world.  
4. There are no reliable techniques for steering the behavior of LLMs.  
5. Experts are not yet able to interpret the inner workings of LLMs.  
6. Human performance on a task isn’t an upper bound on LLM performance.  
7. LLMs need not express the values of their creators nor the values encoded in web text.  
8. Brief interactions with LLMs are often misleading. #critical 

#### Few shot prompting
Source: [A complete introduction to prompt engineering](https://www.mihaileric.com/posts/a-complete-introduction-to-prompt-engineering/)
**Merits**
- [Various research](https://arxiv.org/abs/2005.14165) claims that by providing these demonstrations, the LLM is _learning_ how to perform the task on the fly.
- Recent studies have shown intriguing prompt phenomena in LLMs. [Lu et al.](https://arxiv.org/pdf/2104.08786.pdf) observed that in the few-shot setting, the order in which examples are provided in the prompt can make the difference between near state-of-the-art and random guess performance.
	- This observation is agnostic to the LLM size (i.e. larger models suffer from the same problem as smaller models) and the subset of examples used for the demonstration (i.e. more examples in the prompt doesn’t reduce variance).
	- They then propose an entropy-based probing technique to generate the optimal prompt ordering without a development dataset. The approach is shown to robustly reduce variance for models even across diverse prompt templates.

**Demerits**
[Zhao and Wallace et al.](https://arxiv.org/pdf/2102.09690.pdf) also do an in-depth study of the instability of few-shot prompting. They show that with few-shot prompts, LLMs suffer from three types of biases:
-   **Majority label bias**. They tend to predict training sample labels that appear frequently.
-   **Recency bias**. They tend to predict answers near the end of the prompt.
-   **Common token bias**. They tend to predict answers that appear frequently in the pretraining data.

#### Automated Prompt Generation
Source: [A complete introduction to prompt engineering](https://www.mihaileric.com/posts/a-complete-introduction-to-prompt-engineering/)

Given the finicky nature of manual prompt engineering, there have been a number of promising research efforts to develop automated prompting techniques.
- [Shin, Razeghi, and Logan et al.](https://aclanthology.org/2020.emnlp-main.346.pdf) developed a gradient-guided search technique for automatically producing prompts via a set of trigger tokens. When their technique was applied to masked language models (MLM), they were able to produce impressive performance on tasks such as sentiment analysis, natural language inference, and fact retrieval, even outperforming finetuned models in low-data regimes.
- [Jiang and Xu et al.](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00324/96460/How-Can-We-Know-What-Language-Models-Know) proposed using mining and paraphrasing methods to generate optimal prompts for MLM systems, demonstrating a nearly 10% boost in accuracy of relational knowledge extraction.
- [Li et al.](https://arxiv.org/pdf/2101.00190.pdf) created an alternative technique that uses a learned, continuous vector (called a _prefix_) that is prepended to the input of generative models whose other parameters are held fixed. The researchers used prefix tuning for [GPT2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) and [BART](https://arxiv.org/abs/1910.13461) generation and were able to outperform finetuned models with 1000x parameters in full-data and low-data settings.

#### Perplexity and temperature
#critical 
Source: [A generic framework for chatgpt prompt engineering](https://medium.com/@thorbjoern.heise/a-generic-framework-for-chatgpt-prompt-engineering-7097f6513a0b)
The more complex the input, the harder it is for the language model to find corresponding patterns in its training data, resulting in less accurate responses. The level of perplexity is used to determine the complexity of a prompt. This metric measures how well a language model can predict a given piece of text. [A lower perplexity value indicates that the model can predict the test set better.](https://www.researchgate.net/publication/366136336_Demystifying_Prompts_in_Language_Models_via_Perplexity_Estimation)

So, in order to optimize our output, we must first optimize our input. The table below displays some prompts, the corresponding output, and the level of perplexity as calculated [by the betterprompt tool.](https://github.com/krrishdholakia/betterprompt)

#### Introducing a Generic Framework for Prompt Engineering
Source: [A generic framework for chatgpt prompt engineering](https://medium.com/@thorbjoern.heise/a-generic-framework-for-chatgpt-prompt-engineering-7097f6513a0b)
- Basic Instructions
	- Initiation
	- Functional Instruction
	- Data Input
- Specifiers
	- Format Specifier

#### Model deployment options
Source: [Generative AI with Cohere: Part 3 - The Generate Endpoint](https://txt.cohere.com/generative-ai-part-3/)
![Managed LLMs like Cohere's let you focus on building your applications | 500](https://txt.cohere.com/content/images/2023/02/LLM-Options.png)

Response using the API call:
![The Generate endpoint's input (prompt) and output (response) | 500](https://txt.cohere.com/content/images/2023/05/generate-input-output.png)

#### Model finetuning
Source: [Generative AI with Cohere: Part 4 - Creating Custom Models](https://txt.cohere.com/generative-ai-part-4/)
![Custom models are finetuned on a specific dataset to adapt it to a particular task | 500](https://txt.cohere.com/content/images/2023/02/custom-models.png)

#### Long way for Language models
Source: [Giving GPT-3 a Turing Test](https://lacker.io/ai/2020/07/06/giving-gpt-3-a-turing-test.html)

We have certainly come a long way. The state of the art before modern neural networks was
```
> Hello, I am Eliza. 

Q: Who was president of the United States in 1801?
A: What answer would please you the most? 

Q: Which is heavier, a mouse or an elephant?
A: Come, come, elucidate your thoughts.
```
GPT-3 is quite impressive in some areas, and still clearly subhuman in others. My hope is that with a better understanding of its strengths and weaknesses, we software engineers will be better equipped to use modern language models in real products.