- This article contains the foundational concepts of PE which are essential in understanding how to structure the prompt so as to make the maximum use out of LLMs ouput.
	- The description of detailed techniques is explained in [[Prompt Engineering - Techniques]] article.
- Source: [PromptingGuide](https://www.promptingguide.ai/)
- Reference(s):
	- [Best practices for prompt engineering with OpenAI API](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)

# ✥ Introduction
- developing and optimizing prompts to efficiently use language models (LMs) for a wide variety of applications and research topics
- help to better understand the capabilities and limitations of large language models (LLMs)
- application based on user base:
	- Researchers use PE to improve the capacity of LLMs on a wide range of common and complex tasks such as question answering and arithmetic reasoning
	- Developers use PE to design robust and effective prompting techniques that interface with LLMs and other tools.
- Application scope: 
	- not just about designing and developing prompts, encompasses a wide range of skills and techniques that are useful for interacting and developing with LLMs
	- an important skill to interface, build with, and understand capabilities of LLMs. You can use prompt engineering to improve safety of LLMs and build new capabilities like augmenting LLMs with domain knowledge and external tools.


# ✥ General terminology
When working with prompts, you interact with the LLM via an API or directly. You can configure a few parameters to get different results for your prompts.

#### `Temperature`
The lower the `temperature`, the more deterministic the results in the sense that the highest probable next token is always picked. Increasing temperature could lead to more randomness, which encourages more diverse or creative outputs.
- Use lower `temperature` for fact-based QA to encourage more factual and concise responses
- Use higher value for creative tasks for essay or poem generation

#### `Top_p`
with `top_p`, a sampling technique with temperature called nucleus sampling, you can control how deterministic the model is at generating a response. If you are looking for exact and factual answers keep this low. If you are looking for more diverse responses, increase to a higher value.


# ✥ Basics of prompting
## Basics prompts
- You can achieve a lot with simple prompts, but the quality of results depends on how much information you provide it and how well-crafted it is. 
- A prompt can contain information like the _instruction_ or _question_ you are passing to the model and include other details such as _context_, _inputs_, or _examples_. 
- Use these elements to instruct the model better and as a result get better results.

_Prompt_
```
The sky is
```
_Output:_
```
blue

The sky is blue on a clear day. On a cloudy day, the sky may be gray or white.
```

As you can see, the language model outputs a continuation of strings that make sense given the context `"The sky is"`. The output might be unexpected or far from the task you want to accomplish.
This basic example also highlights the necessity to provide more context or instructions on what specifically you want to achieve.

Let's try to improve it a bit:
_Prompt:_
```
Complete the sentence: 

The sky is
```

_Output:_
```
so  beautiful today.
```

Is that better? Well, you told the model to complete the sentence so the result looks a lot better as it follows exactly what you told it to do ("complete the sentence"). This approach of designing optimal prompts to instruct the model to perform a task is what's referred to as **prompt engineering**.

### Prompt formatting
You have tried a very simple prompt above. A standard prompt has the following format:
```
<Question>?
```
or
```
<Instruction>
```


You can format this into a question answering (QA) format, which is standard in a lot of QA datasets, as follows:
```
Q: <Question>?
A: 
```
When prompting like the above, it's also referred to as ===_zero-shot prompting_===, i.e., you are directly prompting the model for a response without any examples or demonstrations about the task you want it to achieve. Some large language models do have the ability to perform zero-shot prompting but it depends on the complexity and knowledge of the task at hand.

Given the standard format above, one popular and effective technique to prompting is referred to as ===_few-shot prompting_=== where you provide exemplars (i.e., demonstrations). You can format few-shot prompts as follows:
```
<Question>?
<Answer>

<Question>?
<Answer>

<Question>?
<Answer>

<Question>?
```

The QA format version would look like this:
```
Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A:
```

Keep in mind that it's not required to use QA format. The prompt format depends on the task at hand. For instance, you can perform a simple classification task and give exemplars that demonstrate the task as follows:

_Prompt:_
```
This is awesome! // Positive
This is bad! // Negative
Wow that movie was rad! // Positive
What a horrible show! //
```
_Output:_
```
Negative
```

Few-shot prompts enable in-context learning, which is the ability of language models to learn tasks given a few demonstrations.


# ✥ Elements of a Prompt
A prompt may contain following elements:
- Instruction
	- a specific task or instruction you want the model to perform
- Context
	- external information or additional context that can steer the model to better responses
- Input data
	- the input or question that we are interested to find a response for
- Output indicator
	- the type or format of the output

PS: You do not need all the four elements for a prompt and the format depends on the task at hand. 


# ✥ General tips for designing prompts
### Start simple
As you get started with designing prompts, you should keep in mind that it is really an iterative process that requires a lot of experimentation to get optimal results. Using a simple playground from [_OpenAI_](https://platform.openai.com/playground) or [_Cohere_](https://docs.cohere.com/docs/playground-overview) is a good starting point.
You can start with simple prompts and keep adding more elements and context as you aim for better results. Iterating your prompt along the way is vital for this reason. As you read the guide, you will see many examples where specificity, simplicity, and conciseness will often give you better results.
When you have a big task that involves many different subtasks, you can try to break down the task into simpler subtasks and keep building up as you get better results. This avoids adding too much complexity to the prompt design process at the beginning.

### The instruction
You can design effective prompts for various simple tasks by using commands to instruct the model what you want to achieve, such as "Write", "Classify", "Summarize", "Translate", "Order", etc.
Try different instructions with different keywords, contexts, and data and see what works best for your particular use case and task. Usually, the more specific and relevant the context is to the task you are trying to perform, the better.
Others recommend that you place instructions at the beginning of the prompt. Another recommendation is to use some clear separator like "###" to separate the instruction and context.

For instance:
_Prompt:_
```
### Instruction ###
Translate the text below to Spanish:

Text: "hello!"
```
_Output:_
```
¡Hola!
```

### Specificity
Be very specific about the instruction and task you want the model to perform. The more descriptive and detailed the prompt is, the better the results. There aren't specific tokens or keywords that lead to better results. It's more important to have a good format and descriptive prompt. In fact, providing examples in the prompt is very effective to get desired output in specific formats.
When designing prompts, you should also keep in mind the length of the prompt as there are limitations regarding how long the prompt can be. Thinking about how specific and detailed you should be. The details should be relevant and contribute to the task at hand.
As an example, let's try a simple prompt to extract specific information from a piece of text.

_Prompt:_
```
Extract the name of places in the following text. 

Desired format:
Place: <comma_separated_list_of_company_names>

Input: "Although these developments are encouraging to researchers, much is still a mystery. “We often have a black box between the brain and the effect we see in the periphery,” says Henrique Veiga-Fernandes, a neuroimmunologist at the Champalimaud Centre for the Unknown in Lisbon. “If we want to use it in the therapeutic context, we actually need to understand the mechanism.""
```
_Output:_
```
Place: Champalimaud Centre for the Unknown, Lisbon
```
Input text is obtained from [this Nature article](https://www.nature.com/articles/d41586-023-00509-z).

### Avoid Impreciseness
Given the tips above about being detailed and improving format, it's easy to fall into the trap of wanting to be too clever about prompts and potentially creating imprecise descriptions. It's often better to be specific and direct.
For example, you might be interested in learning the concept of prompt engineering. You might try something like:
```
Explain the concept prompt engineering. Keep the explanation short, only a few sentences, and don't be too descriptive.
```
It's not clear from the prompt above how many sentences to use and what style. You might still somewhat get good responses with the above prompts but the better prompt would be one that is very specific, concise, and to the point. Something like:
```
Use 2-3 sentences to explain the concept of prompt engineering to a high school student.
```

### To do or not to do?
Another common tip when designing prompts is to avoid saying what not to do but say what to do instead. This encourages more specificity and focuses on the details that lead to good responses from the model.
Here is an example of a movie recommendation chatbot failing at exactly what I don't want it to do because of how I wrote the instruction -- focusing on what not to do.
_Prompt:_
```
The following is an agent that recommends movies to a customer. DO NOT ASK FOR INTERESTS. DO NOT ASK FOR PERSONAL INFORMATION.

Customer: Please recommend a movie based on my interests.
Agent: 
```
_Output:_
```
Sure, I can recommend a movie based on your interests. What kind of movie would you like to watch? Do you prefer action, comedy, romance, or something else?
```

Here is a better prompt:
_Prompt:_
```
The following is an agent that recommends movies to a customer. The agent is responsible to recommend a movie from the top global trending movies. It should refrain from asking users for their preferences and avoid asking for personal information. If the agent doesn't have a movie to recommend, it should respond "Sorry, couldn't find a movie to recommend today.".

Customer: Please recommend a movie based on my interests.
Agent:
```
_Output:_
```
Sorry, I don't have any information about your interests. However, here's a list of the top global trending movies right now: [list of movies]. I hope you find something you like!
```


# ✥ Examples of Prompts
The previous section introduced a basic example of how to prompt LLMs.
This section will provide more examples of how to use prompts to achieve different tasks and introduce key concepts along the way. The few examples below illustrate how you can use well-crafted prompts to perform different types of tasks.

Topics:
	1. Text summarization
	2. Information extraction
	3. Question answering
	4. Text classification
	5. Conversation
	6. Code generation
	7. Reasoning

#### ✱ Text summarization
One of the standard tasks in natural language generation is text summarization. Text summarization can include many different flavors and domains. In fact, one of the most promising applications of language models is the ability to summarize articles and concepts into quick and easy-to-read summaries. Let's try a basic summarization task using prompts.

Let's say you are interested to learn about antibiotics, you could try a prompt like this:
_Prompt:_
```
Explain antibiotics
A:
```
_Output:_
```
Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.
```
The "A:" is an explicit prompt format that you use in question answering. You used it here to tell the model that there is an expected further. In this example, it's not clear how this is useful vs not using it but we will leave it that for later examples. Let's just assume that this is too much information and you want to summarize it further. In fact, you can instruct the model to summarize into one sentence like so:

_Prompt:_
```
Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.

Explain the above in one sentence:
```
_Output:_
```
Antibiotics are medications used to treat bacterial infections by either killing the bacteria or stopping them from reproducing, but they are not effective against viruses and overuse can lead to antibiotic resistance.
```
Without paying too much attention to the accuracy of the output above, the model tried to summarize the paragraph in one sentence. 

#### ✱ Information extraction
While language models are trained to perform natural language generation and related tasks, it's also very capable of performing classification and a range of other natural language processing (NLP) tasks.
Here is an example of a prompt that extracts information from a given paragraph.

_Prompt:_
```
Author-contribution statements and acknowledgements in research papers should state clearly and specifically whether, and to what extent, the authors used AI technologies such as ChatGPT in the preparation of their manuscript and analysis. They should also indicate which LLMs were used. This will alert editors and reviewers to scrutinize manuscripts more carefully for potential biases, inaccuracies and improper source crediting. Likewise, scientific journals should be transparent about their use of LLMs, for example when selecting submitted manuscripts.

Mention the large language model based product mentioned in the paragraph above:
```
_Output:_
```
The large language model based product mentioned in the paragraph above is ChatGPT.
```
There are many ways you can improve the results above, but this is already very useful.
By now it should be obvious that you can ask the model to perform different tasks by simply instructing it what to do. That's a powerful capability that AI product developers are already using to build powerful products and experiences.
Paragraph source: [ChatGPT: five priorities for research](https://www.nature.com/articles/d41586-023-00288-7)

####  ✱ Question answering
One of the best ways to get the model to respond to specific answers is to improve the format of the prompt. As covered before, a prompt could combine instructions, context, input, and output indicators to get improved results. While these components are not required, it becomes a good practice as the more specific you are with instruction, the better results you will get. Below is an example of how this would look following a more structured prompt.

_Prompt:_
```
Answer the question based on the context below. Keep the answer short and concise. Respond "Unsure about answer" if not sure about the answer.

Context: Teplizumab traces its roots to a New Jersey drug company called Ortho Pharmaceutical. There, scientists generated an early version of the antibody, dubbed OKT3. Originally sourced from mice, the molecule was able to bind to the surface of T cells and limit their cell-killing potential. In 1986, it was approved to help prevent organ rejection after kidney transplants, making it the first therapeutic antibody allowed for human use.

Question: What was OKT3 originally sourced from?

Answer:
```
_Output:_
```
Mice.
```
Note the framing of the prompt i.e. use of header, formatters like context, question, and answer.
Context obtained from [Nature](https://www.nature.com/articles/d41586-023-00400-x).

#### ✱ Text classification
As a prompt engineer, you need to get better at providing better instructions. But that's not all! You will also find that for harder use cases, just providing instructions won't be enough. This is where you need to think more about the context and the different elements you can use in a prompt. Other elements you can provide are `input data` or `examples`.

Let's try to demonstrate this by providing an example of text classification.
_Prompt:_
```
Classify the text into neutral, negative or positive. 

Text: I think the food was okay. 
Sentiment:
```
_Output:_
```
Neutral
```
You gave the instruction to classify the text and the model responded with `'Neutral'`, which is correct. Nothing is wrong with this but let's say that what you really need is for the model to give the label in the exact format you want. So instead of `Neutral`, you want it to return `neutral`. How do you achieve this? There are different ways to do this. You care about specificity here, so the more information you can provide the prompt, the better results. You can try providing examples to specify the correct behavior. Let's try again:
_Prompt:_
```
Classify the text into neutral, negative or positive. 

Text: I think the vacation is okay.
Sentiment: neutral 

Text: I think the food was okay. 
Sentiment:
```
_Output:_
```
neutral
```

Perfect! This time the model returned `neutral` which is the specific label you were looking for. It seems that the example provided in the prompt helped the model to be specific in its output.

To highlight why sometimes being specific is important, check out the example below and spot the problem:
_Prompt:_
```
Classify the text into nutral, negative or positive. 

Text: I think the vacation is okay.
Sentiment:
```
_Output:_
```
Neutral
```
What is the problem here? As a hint, the made-up `nutral` label is completely ignored by the model. Instead, the model outputs `Neutral` as it has some bias towards that label. But let's assume that what you really want is `nutral`. How would you fix this? Maybe you can try adding descriptions to the labels or add more examples to the prompt?

#### ✱ Conversation
Perhaps one of the more interesting things you can achieve with prompt engineering is instructing the LLM system on how to behave, its intent, and its identity. This is particularly useful when you are building conversational systems like customer service chatbots.
For instance, let's create a conversational system that's able to generate more technical and scientific responses to questions. Note how you are explicitly telling it how to behave through the instruction. This is sometimes referred to as _role prompting_.
_Prompt:_
```
The following is a conversation with an AI research assistant. The assistant tone is technical and scientific.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of blackholes?
AI:
```
_Output:_
```
Sure! Black holes are regions of spacetime where the gravitational force is so strong that nothing, not even light, can escape from it. They are created when a very massive star dies and its core collapses in on itself, forming a singularity of infinite density. The intense gravity of the singularity pulls in all the matter and radiation around it, creating the black hole.
```

Our AI research assistant sounds a bit too technical, right? Okay, let's change this behavior and instruct the system to give more accessible answers.
_Prompt:_
```
The following is a conversation with an AI research assistant. The assistant answers should be easy to understand even by primary school students.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of black holes?
AI: 
```
_Output:_
```
Sure! Black holes are created when a very massive star runs out of fuel and collapses in on itself. The gravity of the star is so strong that nothing, not even light, can escape its pull. This creates a point in space-time with an intense gravitational force called a black hole.
```
You made some progress. You can continue improving it. If you add more examples, you might get even better results.

#### ✱ Code generation
One application where LLMs are quite effective is code generation. ==Copilot== is a great example of this. There are a vast number of code-generation tasks you can perform with clever prompts. Let's look at a few examples below.

First, let's try a simple program that greets the user.
_Prompt:_
```
/*
Ask the user for their name and say "Hello"
*/
```
_Output:_
```
let name = prompt("What is your name?");
console.log(`Hello, ${name}!`);
```
Notice that you didn't even need to specify the language to use.

Let's switch levels a bit. The example below shows how powerful LLMs can be with a little more effort in designing the prompts.
_Prompt:_
```
"""
Table departments, columns = [DepartmentId, DepartmentName]
Table students, columns = [DepartmentId, StudentId, StudentName]
Create a MySQL query for all students in the Computer Science Department
"""
```
_Output:_

```
SELECT StudentId, StudentName 
FROM students 
WHERE DepartmentId IN (SELECT DepartmentId FROM departments WHERE DepartmentName = 'Computer Science');
```

This is very impressive. In this case, you provided data about the database schema and asked it to generate a valid MySQL query.

#### ✱ Reasoning
Perhaps one of the most difficult tasks for an LLM today is one that requires some form of reasoning. Reasoning is one of most interesting areas due to the types of complex applications that can emerge from LLMs.
There have been some improvements in tasks involving mathematical capabilities. That said, it's important to note that current LLMs struggle to perform reasoning tasks so this requires even more advanced prompt engineering techniques. 
_Prompt:_
```
What is 9,000 * 9,000?
```
_Output:_
```
81,000,000
```

Let's try something more difficult.
_Prompt:_
```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 

A: 
```
_Output_
```
No, the odd numbers in this group add up to an odd number: 119.
```
That's incorrect! Let's try to improve this by improving the prompt.
_Prompt:_
```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 

Solve by breaking the problem into steps. First, identify the odd numbers, add them, and indicate whether the result is odd or even. 
```
_Output:_
```
Odd numbers: 15, 5, 13, 7, 1
Sum: 41 
41 is an odd number.
```

Much better, right? By the way, I tried this a couple of times and the system sometimes fails. If you provide better instructions combined with examples, it might help get more accurate results.


# ✥ Prompt refinement
Source: [ChatGPT3-Free-Prompt-List](https://github.com/mattnigh/ChatGPT3-Free-Prompt-List)
### Prompt Refinement: Fixing 'Soulless Writing'
-   **Encourage creativity:** `"Rewrite the existing document to make it more imaginative, engaging, and unique."`
-   **Focus on storytelling:** `"Transform the existing document into a compelling story that highlights the challenges faced and the solutions provided."
-   **Use persuasive language:** `"Refine the existing document by incorporating persuasive language and techniques to make it more convincing and impactful."
-   **Emphasize emotion:** `"Add emotional language and sensory details to the existing document to make it more relatable and engaging."
-   **Utilize sensory details:** `"Refine the existing document by adding sensory details and descriptive language to bring it to life and engage the reader."
-   **Make the content concise:** `"Refine the existing document by removing unnecessary information and making it more concise and to-the-point."
-   **Highlight key points:** `"Rewrite the existing document to emphasize the key points and make them more impactful."
-   **Use vivid language:** `"Refine the existing document by using vivid language and descriptive adjectives to make it more engaging."
-   **Create a sense of urgency:** `"Refine the existing document by adding a sense of urgency and emphasizing the need for immediate action."`
-   **Address objections:** `"Refine the existing document by anticipating and addressing potential objections to the content."`
-   **Personalize the content:** `"Refine the existing document by personalizing the language and making it more relatable to the reader."`

### Prompt Refinement: Increase Readability
-   **Use clear and concise language:** `"Explain technical concepts in simple terms."`
-   **Add visual aids:** `"Using mermaid.js you can include diagrams to illustrate complex concepts (low reliability)."`
-   **Use headings and subheadings:** `"Divide the document into sections with clear headings and subheadings."`
-   **Highlight key points:** `"Emphasize important information using bold or italic text."`
-   **Add real-life examples:** `"Include case studies or real-world examples to make concepts more relatable."`
-   **Use clear and consistent formatting:** `"Use a consistent font, font size, and layout throughout the document."`
-   **Include analogies and comparisons:** `"Explain complex ideas using analogies or comparisons."`
-   **Use active voice:** `"Write in active voice to make sentences more engaging and easier to follow."`




--- 

# Legends:
| Short hand | Full form          |
| ---------- | ------------------ |
| PT         | Prompt engineering |
|            |                    |

