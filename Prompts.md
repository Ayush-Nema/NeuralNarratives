Source: [Brex's Prompt Engineering Guide](https://github.com/brexhq/prompt-engineering)

**Table of contents:**
-   [What is a prompt?](https://github.com/brexhq/prompt-engineering#what-is-a-prompt)
    -   [Hidden Prompts](https://github.com/brexhq/prompt-engineering#hidden-prompts)
    -   [Tokens](https://github.com/brexhq/prompt-engineering#tokens)
    -   [Token Limits](https://github.com/brexhq/prompt-engineering#token-limits)
    -   [Prompt Hacking](https://github.com/brexhq/prompt-engineering#prompt-hacking)
        -   [Jailbreaks](https://github.com/brexhq/prompt-engineering#jailbreaks)
        -   [Leaks](https://github.com/brexhq/prompt-engineering#leaks)
-   [Why do we need prompt engineering?](https://github.com/brexhq/prompt-engineering#why-do-we-need-prompt-engineering)
    -   [Give a Bot a Fish](https://github.com/brexhq/prompt-engineering#give-a-bot-a-fish)
        -   [Semantic Search](https://github.com/brexhq/prompt-engineering#semantic-search)
    -   [Teach a Bot to Fish](https://github.com/brexhq/prompt-engineering#teach-a-bot-to-fish)
        -   [Command Grammars](https://github.com/brexhq/prompt-engineering#command-grammars)
        -   [ReAct](https://github.com/brexhq/prompt-engineering#react)
        -   [GPT-4 vs GPT-3.5](https://github.com/brexhq/prompt-engineering#gpt-4-vs-gpt-35)
-   [Strategies](https://github.com/brexhq/prompt-engineering#strategies)
    -   [Embedding Data](https://github.com/brexhq/prompt-engineering#embedding-data)
        -   [Simple Lists](https://github.com/brexhq/prompt-engineering#simple-lists)
        -   [Markdown Tables](https://github.com/brexhq/prompt-engineering#markdown-tables)
        -   [JSON](https://github.com/brexhq/prompt-engineering#json)
        -   [Freeform Text](https://github.com/brexhq/prompt-engineering#freeform-text)
        -   [Nested Data](https://github.com/brexhq/prompt-engineering#nested-data)
    -   [Citations](https://github.com/brexhq/prompt-engineering#citations)
    -   [Programmatic Consumption](https://github.com/brexhq/prompt-engineering#programmatic-consumption)
    -   [Chain of Thought](https://github.com/brexhq/prompt-engineering#chain-of-thought)
        -   [Averaging](https://github.com/brexhq/prompt-engineering#averaging)
        -   [Interpreting Code](https://github.com/brexhq/prompt-engineering#interpreting-code)
        -   [Delimiters](https://github.com/brexhq/prompt-engineering#delimiters)
    -   [Fine Tuning](https://github.com/brexhq/prompt-engineering#fine-tuning)
        -   [Downsides](https://github.com/brexhq/prompt-engineering#downsides)
-   [Additional Resources](https://github.com/brexhq/prompt-engineering#additional-resources)


# What is a prompt?
A prompt, sometimes referred to as context, is the text provided to a model before it begins generating output. It guides the model to explore a particular area of what it has learned so that the output is relevant to your goals. As an analogy, if you think of the language model as a source code interpreter, then a prompt is the source code to be interpreted. Somewhat amusingly, a language model will happily attempt to guess what source code will do:
![350](https://user-images.githubusercontent.com/89960/231946874-be91d3de-d773-4a6c-a4ea-21043bd5fc13.png "The GPT-4 model interpreting Python code.")
And it _almost_ interprets the Python perfectly!

Frequently, prompts will be an instruction or a question, like:
![350](https://user-images.githubusercontent.com/89960/232413246-81db18dc-ef5b-4073-9827-77bd0317d031.png)

On the other hand, if you don’t specify a prompt, the model has no anchor to work from and you’ll see that it just **randomly samples from anything it has ever consumed**:
###### Examples: 
**From GPT-3-Davinci:**
![image | 250](https://user-images.githubusercontent.com/89960/232413846-70b05cd1-31b6-4977-93f0-20bf29af7132.png)

[![image | 250](https://user-images.githubusercontent.com/89960/232413930-7d414dcd-87e5-431a-91c8-bb6e0ef54f42.png)

![image | 250](https://user-images.githubusercontent.com/89960/232413978-59c7f47d-ec20-4673-9458-85471a41fee0.png)

**From GPT-4:**
![image |250](https://user-images.githubusercontent.com/89960/232414631-928955e5-3bab-4d57-b1d6-5e56f00ffda1.png)

![image | 250](https://user-images.githubusercontent.com/89960/232414678-e5b6d3f4-36c6-420f-b38f-2f9c8df391fb.png)

![image | 250](https://user-images.githubusercontent.com/89960/232414734-c8f09cad-aceb-4149-a28a-33675cde8011.png)






## Hidden Prompts
```ad-important
⚠️ Always assume that any content in a hidden prompt can be seen by the user.
```
In applications where a user is interacting with a model dynamically, such as chatting with the model, there will typically be portions of the prompt that are never intended to be seen by the user. These hidden portions may occur anywhere, though there is almost always a hidden prompt at the start of a conversation.

Typically, this includes an initial chunk of text that sets the tone, model constraints, and goals, along with other dynamic information that is specific to the particular session – user name, location, time of day, etc...

The model is static and frozen at a point in time, so if you want it to know current information, like the time or the weather, you must provide it.

If you’re using [the OpenAI Chat API](https://platform.openai.com/docs/guides/chat/introduction), they delineate hidden prompt content by placing it in the `system` role.

Here’s an example of a hidden prompt followed by interactions with the content in that prompt:
![450](https://user-images.githubusercontent.com/89960/232416074-84ebcc10-2dfc-49e1-9f48-a240102877ee.png "A very simple hidden prompt.")
In this example, you can see we explain to the bot the various roles, some context on the user, some dynamic data we want the bot to have access to, and then guidance on how the bot should respond.

In practice, hidden prompts may be quite large. Here’s a larger prompt taken from a [ChatGPT command-line assistant](https://github.com/manno/chatgpt-linux-assistant/blob/main/system_prompt.txt):
From: [https://github.com/manno/chatgpt-linux-assistant](https://github.com/manno/chatgpt-linux-assistant)
<details> <summary>Click here</summary> <p>

We are a in a chatroom with 3 users. 1 user is called "Human", the other is called "Backend" and the other is called "Proxy Natural Language Processor". I will type what "Human" says and what "Backend" replies. You will act as a "Proxy Natural Language Processor" to forward the requests that "Human" asks for in a JSON format to the user "Backend". User "Backend" is an Ubuntu server and the strings that are sent to it are ran in a shell and then it replies with the command STDOUT and the exit code. The Ubuntu server is mine. When "Backend" replies with the STDOUT and exit code, you "Proxy Natural Language Processor" will parse and format that data into a simple English friendly way and send it to "Human". Here is an example:

I ask as human:
Human: How many unedited videos are left?
Then you send a command to the Backend:
Proxy Natural Language Processor: @Backend {"command":"find ./Videos/Unedited/ -iname '*.mp4' | wc -l"}
Then the backend responds with the command STDOUT and exit code:
Backend: {"STDOUT":"5", "EXITCODE":"0"}
Then you reply to the user:
Proxy Natural Language Processor: @Human There are 5 unedited videos left.

Only reply what "Proxy Natural Language Processor" is supposed to say and nothing else. Not now nor in the future for any reason.

Another example:

I ask as human:
Human: What is a PEM certificate?
Then you send a command to the Backend:
Proxy Natural Language Processor: @Backend {"command":"xdg-open 'https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail'"}
Then the backend responds with the command STDOUT and exit code:
Backend: {"STDOUT":"", "EXITCODE":"0"}
Then you reply to the user:
Proxy Natural Language Processor: @Human I have opened a link which describes what a PEM certificate is.


Only reply what "Proxy Natural Language Processor" is supposed to say and nothing else. Not now nor in the future for any reason.

Do NOT REPLY as Backend. DO NOT complete what Backend is supposed to reply. YOU ARE NOT TO COMPLETE what Backend is supposed to reply.
Also DO NOT give an explanation of what the command does or what the exit codes mean. DO NOT EVER, NOW OR IN THE FUTURE, REPLY AS BACKEND.

Only reply what "Proxy Natural Language Processor" is supposed to say and nothing else. Not now nor in the future for any reason.
```
</p> </details> 

You’ll see some good practices there, such as including lots of examples, repetition for important behavioral aspects, constraining the replies, etc…
```ad-summary 
⚠️ Always assume that any content in a hidden prompt can be seen by the user.
```


## Tokens
If you thought tokens were 🔥 in 2022, tokens in 2023 are on a whole different plane of existence. The atomic unit of consumption for a language model is not a “word”, but rather a “token”. You can kind of think of tokens as syllables, and on average they work out to about 750 words per 1,000 tokens. They represent many concepts beyond just alphabetical characters – such as punctuation, sentence boundaries, and the end of a document.

Here’s an example of how GPT may tokenize a sequence:
![450](https://user-images.githubusercontent.com/89960/232417569-8d562792-64b5-423d-a7a2-db7513dd4d61.png "An example tokenization. You can experiment here: https://platform.openai.com/tokenizer")

You can experiment with a tokenizer here: [https://platform.openai.com/tokenizer](https://platform.openai.com/tokenizer) #critical 

Different models will use different tokenizers with different levels of granularity. You could, in theory, just feed a model 0’s and 1’s – but then the model needs to learn the concept of characters from bits, and then the concept of words from characters, and so forth. Similarly, you could feed the model a stream of raw characters, but then the model needs to learn the concept of words, and punctuation, etc… and, in general, the models will perform worse.

To learn more, [Hugging Face has a wonderful introduction to tokenizers](https://huggingface.co/docs/transformers/tokenizer_summary) and why they need to exist. #critical

There’s a lot of nuance around tokenization, such as vocabulary size or different languages treating sentence structure meaningfully different (e.g. words not being separated by spaces). Fortunately, language model APIs will almost always take raw text as input and tokenize it behind the scenes – _so you rarely need to think about tokens_.
Except for one important scenario, which we discuss next: token limits.


## Token limits
Prompts tend to be append-only, because you want the bot to have the entire context of previous messages in the conversation. Language models, in general, are ==stateless== and won’t remember anything about previous requests to them, so you always need to include everything that it might need to know that is specific to the current session. #critical 

A major downside of this is that the leading language model architecture, the Transformer, has a fixed input and output size – at a certain point the prompt can’t grow any larger. The total size of the prompt, sometimes referred to as the “context window”, is model dependent. For GPT-3, it is 4,096 tokens. For GPT-4, it is 8,192 tokens or 32,768 tokens depending on which variant you use.

If your context grows too large for the model, the most common tactic is the truncate the context in a sliding window fashion. If you think of a prompt as `hidden initialization prompt + messages[]`, usually the hidden prompt will remain unaltered, and the `messages[]` array will take the last N messages.

You may also see more clever tactics for prompt truncation – such as discarding only the user messages first, so that the bot's previous answers stay in the context for as long as possible, or asking an LLM to summarize the conversation and then replacing all of the messages with a single message containing that summary. There is no correct answer here and the solution will depend on your application.

Importantly, when truncating the context, you must truncate aggressively enough to **allow room for the response as well**. OpenAI’s token limits include both the length of the input and the length of the output. If your input to GPT-3 is 4,090 tokens, it can only generate 6 tokens in response.

```ad-note
🧙‍♂️ If you’d like to count the number of tokens before sending the raw text to the model, the specific tokenizer to use will depend on which model you are using. OpenAI has a library called [==tiktoken==](https://github.com/openai/tiktoken/blob/main/README.md) that you can use with their models – though there is an important caveat that their internal tokenizer may vary slightly in count, and they may append other metadata, so consider this an approximation.

If you’d like an approximation without having access to a tokenizer, `input.length / 4` will give a rough, but better than you’d expect, approximation for English inputs.
```


## Prompt hacking
#critical 
(This topic is also discussed in [[Risks and misuses]] page)
Prompt engineering and large language models are a fairly nascent field, so new ways to hack around them are being discovered every day. The two large classes of attacks are:
1.  Make the bot bypass any guidelines you have given it.
2.  Make the bot output hidden context that you didn’t intend for the user to see.

There are no known mechanisms to comprehensively stop these, so it is important that you assume the bot may do or say anything when interacting with an adversarial user. Fortunately, in practice, these are mostly cosmetic concerns.
Think of prompts as a way to improve the normal user experience. **We design prompts so that normal users don’t stumble outside of our intended interactions – but always assume that a determined user will be able to bypass our prompt constraints.**

### Jailbreaks
Typically hidden prompts will tell the bot to behave with a certain persona and focus on specific tasks or avoid certain words. It is generally safe to assume the bot will follow these guidelines for non-adversarial users, although non-adversarial users may accidentally bypass the guidelines too.

For example, we can tell the bot:
```
You are a helpful assistant, but you are never allowed to use the word "computer".
```
If we then ask it a question about computers, it will refer to them as a “device used for computing” because it isn’t allowed to use the word “computer”.
![500](https://user-images.githubusercontent.com/89960/232420043-ebe5bcf1-25d9-4a31-ba84-13e9e1f62de2.png "GPT-4 trying hard to not say the word 'computer'.")

It will absolutely refuse to say the word:
![500](https://user-images.githubusercontent.com/89960/232420306-6fcdd6e2-b107-45d5-a1ee-4132fbb5853e.png)

But we can bypass these instructions and get the model to happily use the word if we trick it by asking it to translate the pig latin version of “computer”.
![500](https://user-images.githubusercontent.com/89960/232420600-56083a10-b382-46a7-be18-eb9c005b8371.png)

There are [a number of defensive measures](https://learnprompting.org/docs/prompt_hacking/defensive_measures/overview) you can take here, but typically the best bet is to reiterate your most important constraints as close to the end as possible. For the OpenAI chat API, this might mean including it as a `system` message after the last `user` message. Here’s an example:
![image | 350](https://user-images.githubusercontent.com/89960/232421097-adcaace3-0b21-4c1e-a5c8-46bb25faa2f7.png)
![image | 350](https://user-images.githubusercontent.com/89960/232421142-a47e75b4-5ff6-429d-9abd-a78dbc72466e.png)
Despite OpenAI investing a lot into jailbreaks, there are [very clever work arounds](https://twitter.com/alexalbert__/status/1636488551817965568) being [shared every day](https://twitter.com/zswitten/status/1598088267789787136). #critical 

### Leaks
#critical 
If you missed the previous warnings in this doc, **you should always assume that any data exposed to the language model will eventually be seen by the user**.

As part of constructing prompts, you will often embed a bunch of data in hidden prompts (a.k.a. system prompts). **The bot will happily relay this information to the user**:
![500](https://user-images.githubusercontent.com/89960/232422860-731c1de2-9e77-4957-b257-b0bbda48558c.png "The bot happily regurgitating the information it knows about the user.")
Even if you instruct it not to reveal the information, and it obeys those instructions, there are millions of ways to leak data in the hidden prompt.
Here we have an example where the bot should never mention my city, but a simple reframing of the question get’s it to spill the beans.
![500](https://user-images.githubusercontent.com/89960/232423121-76568893-fa42-4ad8-b2bc-e1001327fa1e.png "The bot refuses to reveal personal information, but we convince it to tell me what city I’m in regardless.")

Similarly, we get the bot to tell us what word it isn’t allowed to say without ever actually saying the word:
![500](https://user-images.githubusercontent.com/89960/232423283-1718f822-59d0-4d18-9a4d-22dd3a2672c0.png "Technically, the bot never said 'computer', but I was still able to get it to tell me everything I needed to know about it.")
You should think of a hidden prompt as a means to make the user experience better or more inline with the persona you’re targeting. **Never place any information in a prompt that you wouldn’t visually render for someone to read on screen**.

---

# Why do we need Prompt Engineering?
Up above, we used an analogy of prompts as the “source code” that a language model “interprets”. **Prompt engineering is the art of writing prompts to get the language model to do what we want it to do** – just like software engineering is the art of writing source code to get computers to do what we want them to do.

When writing good prompts, you have to account for the idiosyncrasies of the model(s) you’re working with. The strategies will vary with the complexity of the tasks. You’ll have to come up with mechanisms to constrain the model to achieve reliable results, incorporate dynamic data that the model can’t be trained on, account for limitations in the model’s training data, design around context limits, and many other dimensions.

There’s an old adage that computers will only do what you tell them to do. **Throw that advice out the window**. Prompt engineering inverts this wisdom. It’s like programming in natural language against a non-deterministic computer that will do anything that you haven’t guided it away from doing.
There are two broad buckets that prompt engineering approaches fall into.

## Give a bot a fish
The “give a bot a fish” bucket is for scenarios when you can explicitly give the bot, in the hidden context, all of the information it needs to do whatever task is requested of it.

For example, if a user loaded up their dashboard and we wanted to show them a quick little friendly message about what task items they have outstanding, we could get the bot to summarize it as
```
You have 4 receipts/memos to upload. The most recent is from Target on March 5th, and the oldest is from Blink Fitness on January 17th. Thanks for staying on top of your expenses!
```
by providing a list of the entire inbox and any other user context we’d like it to have.

![500](https://user-images.githubusercontent.com/89960/233465165-e0c6b266-b347-4128-8eaa-73974e852e45.png "GPT-3 summarizing a task inbox.")

Similarly, if you were helping a user book a trip, you could:
-   Ask the user their dates and destination.
-   Behind the scenes, search for flights and hotels.
-   Embed the flight and hotel search results in the hidden context.
-   Also embed the company’s travel policy in the hidden context.

And then the bot will have real-time travel information + constraints that it can use to answer questions for the user. Here’s an example of the bot recommending options, and the user asking it to refine them:
![500](https://user-images.githubusercontent.com/89960/233465425-9e06320c-b6d9-40ef-b5a4-c556861c1328.png "GPT-4 helping a user book a trip.")
<details> <summary>(Full prompt)</summary> <p>
Brex is a platform for managing business expenses. 

The following is a travel expense policy on Brex:

- Airline highest fare class for flights under 6 hours is economy.
- Airline highest fare class for flights over 6 hours is premium economy.
- Car rentals must have an average daily rate of $75 or under.
- Lodging must have an average nightly rate of $400 or under.
- Lodging must be rated 4 stars or higher.
- Meals from restaurants, food delivery, grocery, bars & nightlife must be under $75
- All other expenses must be under $5,000.
- Reimbursements require review.

The hotel options are:
| Hotel Name | Price | Reviews |
| --- | --- | --- |
| Hilton Financial District | $109/night | 3.9 stars |
| Hotel VIA | $131/night | 4.4 stars |
| Hyatt Place San Francisco | $186/night | 4.2 stars |
| Hotel Zephyr | $119/night | 4.1 stars review |

The flight options are:
| Airline | Flight Time | Duration | Number of Stops | Class | Price |
| --- | --- | --- | --- | --- | --- |
| United | 5:30am-7:37am | 2hr 7 min | Nonstop | Economy | $248 |
| Delta | 1:20pm-3:36pm | 2hr 16 min | Nonstop | Economy | $248 |
| Alaska | 9:50pm-11:58pm | 2hr 8 min | Nonstop | Premium | $512 |

An employee is booking travel to San Francisco for February 20th to February 25th.

Recommend a hotel and flight that are in policy. Keep the recommendation concise, no longer than a sentence or two, but include pleasantries as though you are a friendly colleague helping me out:
</p> </details>

This is the same approach that products like Microsoft Bing use to incorporate dynamic data. When you chat with Bing, it asks the bot to generate three search queries. Then they run three web searches and include the summarized results in the hidden context for the bot to use.

Summarizing this section, the trick to making a good experience is to change the context dynamically in response to whatever the user is trying to do.

```ad-summary
🧙‍♂️ Giving a bot a fish is the most reliable way to ensure the bot gets a fish. You will get the most consistent and reliable results with this strategy. **Use this whenever you can.**
```

### Semantic search
If you just need the bot to know a little more about the world, [a common approach is to perform a semantic search](https://github.com/openai/openai-cookbook/blob/main/examples/Question_answering_using_embeddings.ipynb).

A semantic search is oriented around a document embedding – which you can think of as a fixed-length array[5](https://github.com/brexhq/prompt-engineering#user-content-fn-5-c469ce9a530323051f820a429526d8df) of numbers, where each number represents some aspect of the document (e.g. if it’s a science document, maybe the 843rd number is large, but if it’s an art document the 1,115th number is large – this is overly simplistic, but conveys the idea).[6](https://github.com/brexhq/prompt-engineering#user-content-fn-6-c469ce9a530323051f820a429526d8df)

In addition to computing an embedding for a document, you can also compute an embedding for a user query using the same function. If the user asks “Why is the sky blue?” – you compute the embedding of that question and, in theory, this embedding will be more similar to embeddings of documents that mention the sky than embeddings that don’t talk about the sky.

To find documents related to the user query, you compute the embedding and then find the top-N documents that have the most similar embedding. Then we place these documents (or summaries of these documents) in the hidden context for the bot to reference.

Notably, sometimes user queries are so short that the embedding isn’t particularly valuable. There is a clever technique described in [a paper published in December 2022](https://arxiv.org/pdf/2212.10496.pdf) called a “Hypothetical Document Embedding” or HyDE. Using this technique, you ask the model to generate a hypothetical document in response to the user’s query, and then compute the embedding for this generated document. The model fabricates a document out of thin air – but the approach works! #critical 
The HyDE technique uses more calls to the model, but for many use cases has notable boosts in results.

## Teach a Bot to Fish
Sometimes you’ll want the bot to have the capability to perform actions on the user’s behalf, like adding a memo to a receipt or plotting a chart. Or perhaps we want it to retrieve data in more nuanced ways than semantic search would allow for, like retrieving the past 90 days of expenses.
In these scenarios, we need to teach the bot how to fish.
### Command grammars
Read from source link → https://github.com/brexhq/prompt-engineering#command-grammars

### ReAct
#critical 
In March of 2023, Princeton and Google released a paper “[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/pdf/2210.03629.pdf)”, where they introduce a variant of command grammars that allows for fully autonomous interactive execution of actions and retrieval of data.
The model is instructed to return a `thought` and an `action` that it would like to perform. Another agent (e.g. our client) then performs the `action` and returns it to the model as an `observation`. The model will then loop to return more thoughts and actions until it returns an `answer`.
This is an incredibly powerful technique, effectively allowing the bot to be its own research assistant and possibly take actions on behalf of the user. Combined with a powerful command grammar, the bot should rapidly be able to answer a massive set of user requests.

In this example, we give the model a small set of commands related to getting employee data and searching wikipedia:
| Command       | Argument    | Description                                                                               |
| ------------- | ----------- | ----------------------------------------------------------------------------------------- |
| find_employee | name        | Retrieves an employee by name                                                             |
| get_employee  | id          | Retrieves an employee by ID                                                               |
| get_location  | id          | Retrieves a location by ID                                                                |
| get_reports   | employee_id | Retrieves a list of employee ids that report to the employee associated with employee_id. |
| wikipedia     | article     | Retrieves a wikipedia article on a topic.                                                 |

We then ask the bot a simple question, “Is my manager famous?”.
We see that the bot:
1.  First looks up our employee profile.
2.  From our profile, gets our manager’s id and looks up their profile.
3.  Extracts our manager’s name and searches for them on Wikipedia.
    -   I chose a fictional character for the manager in this scenario.
4.  The bot reads the wikipedia article and concludes that can’t be my manager since it is a fictional character.
5.  The bot then modifies its search to include (real person).
6.  Seeing that there are no results, the bot concludes that my manager is not famous.
![image | 550](https://user-images.githubusercontent.com/89960/233506839-5c8b2d77-1d78-464d-bc33-a725e12f2624.png)
![image | 550](https://user-images.githubusercontent.com/89960/233506870-05fc415d-efa2-48b7-aad9-b5035e535e6d.png)
Full prompt: [Check here](https://github.com/brexhq/prompt-engineering#react)


### GPT-4 vs GPT-3.5
In most of the examples in this doc, the difference between GPT-3.5 and GPT-4 is negligible, but for “teaching a bot to fish” scenarios the difference between the models is notable.

None of the above examples of command grammars, for example, work without meaningful modifications for GPT-3.5. At a minimum, you have to provide a number of examples (at least one usage example per command) before you get any reasonable results. And, for complex sets of commands, it may hallucinate new commands or create fictional arguments.

With a sufficiently thorough hidden prompt, you should be able to overcome these limitations. GPT-4 is capable of far more consistent and complex logic with far simpler prompts (and can get by with zero or small numbers of examples – though it is always beneficial to include as many as possible).

---

# Strategies
This section contains examples and strategies for specific needs or problems. For successful prompt engineering, you will need to combine some subset of all of the strategies enumerated in this document. Don’t be afraid to mix and match things – or invent your own approaches.

## Embedding data
In hidden contexts, you’ll frequently want to embed all sorts of data. The specific strategy will vary depending on the type and quantity of data you are embedding.
Read the detailed description from the source [doc](https://github.com/brexhq/prompt-engineering#embedding-data):
- Simple lists
- Markdown tables
- JSON
- Freeform text
- Nested data

## Citations
Frequently, a natural language response isn’t sufficient on its own and you’ll want the model’s output to cite where it is getting data from.

One useful thing to note here is that anything you might want to cite should have a unique ID. The simplest approach is to just ask the model to link to anything it references:
![550](https://user-images.githubusercontent.com/89960/233509069-1dcbffa2-8357-49b5-be43-9791f93bd0f8.png "GPT-4 will reliably link to data if you ask it to.")

## Programmatic consumption
By default, language models output natural language text, but frequently we need to interact with this result in a programmatic way that goes beyond simply printing it out on screen. You can achieve this by asking the model to output the results in your favorite serialization format (JSON and YAML seem to work best).
Read more on this: [here](https://github.com/brexhq/prompt-engineering#programmatic-consumption)

## Chain-of-thought
#critical 
Sometimes you will bang your head on a prompt trying to get the model to output reliable results, but, no matter what you do, it just won’t work. This will frequently happen when the bot’s final output requires intermediate thinking, but you ask the bot only for the output and nothing else.

The answer may surprise you: ask the bot to show its work. In October 2022, Google released a paper “[Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/pdf/2201.11903.pdf)” where they showed that if, in your hidden prompt, you give the bot examples of answering questions by showing your work, then when you ask the bot to answer something it will show its work and produce more reliable answers.

Just a few weeks after that paper was published, at the end of October 2022, the University of Tokyo and Google released the paper “[Large Language Models are Zero-Shot Reasoners](https://openreview.net/pdf?id=e2TBb5y0yFf)”, where they show that you don’t even need to provide examples – **you simply have to ask the bot to think step-by-step**.

##### Averaging
Here is an example where we ask the bot to compute the average expense, excluding Target. The actual answer is $136.77 and the bot almost gets it correct with $136.43.
![500](https://user-images.githubusercontent.com/89960/233509534-2b32c8dd-a1ee-42ea-82fb-4f84cfe7e9ba.png "The model **almost** gets the average correct, but is a few cents off.")

If we simply add “Let’s think step-by-step”, the model gets the correct answer:
![500](https://user-images.githubusercontent.com/89960/233509608-6e53995b-668b-47f6-9b5e-67afad89f8bc.png "When we ask the model to show its work, it gets the correct answer.")

##### Interpreting Code
Let’s revisit the Python example from earlier and apply chain-of-thought prompting to our question. As a reminder, when we asked the bot to evaluate the Python code it gets it slightly wrong. The correct answer is `Hello, Brex!!Brex!!Brex!!!` but the bot gets confused about the number of !'s to include. In below’s example, it outputs `Hello, Brex!!!Brex!!!Brex!!!`:
![500](https://user-images.githubusercontent.com/89960/233509724-8f3302f8-59eb-4d3b-8939-53d7f63b0299.png "The bot almost interprets the Python code correctly, but is a little off.")

If we ask the bot to show its work, then it gets the correct answer:
![500](https://user-images.githubusercontent.com/89960/233509790-2a0f2189-d864-4d27-aacb-cfc936fad907.png "The bot correctly interprets the Python code if you ask it to show its work.")

##### Delimiters
In many scenarios, you may not want to show the end user all of the bot’s thinking and instead just want to show the final answer. You can ask the bot to delineate the final answer from its thinking. There are many ways to do this, but let’s use JSON to make it easy to parse:
![500](https://user-images.githubusercontent.com/89960/233509865-4f3e7265-6645-4d43-8644-ecac5c0ca4a7.png "The bot showing its work while also delimiting the final answer for easy extraction.")
Using Chain-of-Thought prompting will consume more tokens, resulting in increased price and latency, but the results are noticeably more reliable for many scenarios. It’s a valuable tool to use when you need the bot to do something complex and as reliably as possible.

## Finetuning
Sometimes no matter what tricks you throw at the model, it just won’t do what you want it to do. In these scenarios you can **sometimes** fallback to fine-tuning. This should, in general, be a last resort.
[Fine-tuning](https://platform.openai.com/docs/guides/fine-tuning) is the process of taking an already trained model and then giving it thousands (or more) of example `input:output` pairs
It does not eliminate the need for hidden prompts, because you still need to embed dynamic data, but it may make the prompts smaller and more reliable.

## Downsides
#critical 
There are many downsides to fine-tuning. If it is at all possible, take advantage of the nature of language models being [zero-shot, one-shot, and few-shot learners](https://en.wikipedia.org/wiki/Few-shot_learning_(natural_language_processing)) by teaching them to do something in their prompt rather than fine-tuning.

Some of the downsides include:
-   **Not possible**: [GPT-3.5/GPT-4 isn’t fine tunable](https://platform.openai.com/docs/guides/chat/is-fine-tuning-available-for-gpt-3-5-turbo), which is the primary model / API we’ll be using, so we simply can’t lean in fine-tuning.
-   **Overhead**: Fine-tuning requires manually creating tons of data.
-   **Velocity**: The iteration loop becomes much slower – every time you want to add a new capability, instead of adding a few lines to a prompt, you need to create a bunch of fake data and then run the finetune process and then use the newly fine-tuned model.
-   **Cost**: It is up to 60x more expensive to use a fine-tuned GPT-3 model vs the stock `gpt-3.5-turbo` model. And it is 2x more expensive to use a fine-tuned GPT-3 model vs the stock GPT-4 model.

```ad-warning
⛔️ If you fine-tune a model, **never use real customer data**. Always use synthetic data. The model may memorize portions of the data you provide and may regurgitate private data to other users that shouldn’t be seeing it.
If you never fine-tune a model, we don’t have to worry about accidentally leaking data into the model.
```

---

# Additional Resources

-   🌟 [OpenAI Cookbook](https://github.com/openai/openai-cookbook) 🌟
-   🧑‍💻 [Prompt Hacking](https://learnprompting.org/docs/category/-prompt-hacking) 🧑‍💻
-   📚 [Dair.ai Prompt Engineering Guide](https://github.com/dair-ai/Prompt-Engineering-Guide) 📚

# References
#critical 
- Some great examples of prompts: [awesome-chatgpt-prompts](https://github.com/f/awesome-chatgpt-prompts) 
- [Best practices for prompt engineering with OpenAI API](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)
- 