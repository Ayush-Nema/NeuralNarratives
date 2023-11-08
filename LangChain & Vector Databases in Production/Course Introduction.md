Course link: [LangChain & Vector Databases in Production](https://learn.activeloop.ai/courses/langchain)

### **Table of contents**
- [[#Why This Course?|Why This Course?]]
- [[#What you will learn?|What you will learn?]]
- [[#Modules covered|Modules covered]]
- [[#Required API tokens|Required API tokens]]
- [[#Coding Environment and Packages|Coding Environment and Packages]]
- [[#The Power and Limitations of Large Language Models|The Power and Limitations of Large Language Models]]
- [[#Building Efficient Retrievers with Deep Lake|Building Efficient Retrievers with Deep Lake]]

---
# Why This Course?
This LangChain course will equip you with the knowledge and practical skills to build products and apps using Large Language Models (LLMs). We place heavy emphasis on the hands-on application, striving to guide you through a deep, practical introduction to leveraging the power of LLMs through LangChain.
One of the tools we extensively cover in this course is ==Activeloop's Deep Lake==. It amalgamates the best features of data lakes and vector databases, facilitating companies to create their own data flywheels for refining their Large Language Models. Combined with LangChain, Deep Lake can seamlessly connect datasets to foundational models for various applications, from [understanding GitHub repositories](https://www.activeloop.ai/resources/lang-chain-gpt-4-for-code-understanding-twitter-algorithm/) to [analyzing financial statements](https://www.activeloop.ai/resources/ultimate-guide-to-lang-chain-deep-lake-build-chat-gpt-to-answer-questions-on-your-financial-data/).

# What you will learn?
- Deep understanding of LLMs
- Effective use of LangChain
- Relevant concepts including prompting
- Managing output
- Giving memory to LLMs using vector stores
- Integration of LLMs with external tools
- Use of LLMs as reasoning engines with agents
- Current limitations of LLMs
	- hallucinations
	- limited memory
	- limited context lengths


# Modules covered
#### 1. From Zero to Hero
This introductory module serves as a quick guide, swiftly bringing you up to speed with all the fundamental concepts. It includes hands-on code snippets covering library installation, OpenAI credentials, deriving predictions from LLMs, and more. You'll also take a peek at Deep Lake and its applications.
#### 2. Large Language Models and LangChain
This module provides a comprehensive overview of Large Language Models, including their capabilities, limitations, and use cases. You'll dive deep into LLMs like ChatGPT and GPT-4, explore these models' emergent abilities and scaling laws, and gain insights into phenomena like hallucinations and bias. This module also introduces LangChain and its role in integrating LLMs with other data sources and tools. You will also undertake a project to build a News Articles Summarizer.
#### 3. Learning How to Prompt
Learning how to craft effective prompts is a key skill in working with LLMs. This module delves into the nuances of prompt engineering and teaches you to develop prompts that are easy to maintain. You'll learn techniques such as role prompting, few-shot prompting, and chain of thought. Towards the end of this module, you'll take your learning further by enhancing the News Articles Summarizer built in the previous module and undertaking a project to extract a knowledge graph from news articles.
#### 4. Keeping Knowledge Organized with Indexes
The final module focuses on how to effectively leverage documents as a base for LLMs using LangChain's indexes and retrievers. You'll learn about data ingestion through various loaders, the importance of text splitters, and delve into the concept of embeddings and vector stores. The module ends with a project where you'll build a Customer Support Question Answering Chatbot using ChatGPT, Deep Lake, and LangChain.
#### 5. Combining Components Together with Chains
In this module, you will get a handle on LangChain's chains - a concept that enables the creation of a single, coherent application. You will understand why chains are used and have the opportunity to work on multiple projects. These include creating a YouTube Video Summarizer, building a Jarvis for your Knowledge Base, and exploring code understanding with GPT-4 and LangChain. You will also learn about the Self-Critique Chain and how to guard against undesirable outputs.
#### 6. Giving Memory to LLMs
This module emphasizes the importance of memory in maintaining context over a conversation. You will master the different types of memory in LangChain, including ConversationBufferMemory, ConversationBufferWindowMemory, ConversationSummaryMemory, and ConversationChain. Various exciting projects await you, such as creating a chatbot that interacts with a Github Repo, building a question-answering chatbot, and working with financial data.
#### 7. Making LLMs Interact with the World Using Tools
In this module, you'll explore LangChain's tools and their diverse applications, including Google Search, requests, Python REPL, Wikipedia, and Wolfram-Alpha. Projects in this module revolve around enhancing blog posts with LangChain and Google Search, recreating the Bing chatbot, and leveraging multiple tools simultaneously. You'll also learn how to define custom tools for your specific needs.
#### 8. Using Language Model as Reasoning Engines with Agents
The final module introduces you to the concept of agents in LangChain, with a particular emphasis on using a language model as a reasoning engine. You'll explore autonomous agents, their projects, and the application of AutoGPT with LangChain. The module culminates with a project on building autonomous agents to create comprehensive analysis reports.


# Required API tokens
The course involves practical projects and exercises that will require the use of various API keys. These will be thoroughly guided in the individual lessons. However, the two main API tokens that you will use throughout the course are:
1. **The OpenAI API token**: This will be used to query LLMs like ChatGPT and GPT-4.
2. **The Deep Lake API token**: Essential for creating Deep Lake datasets as vector stores for the projects we’ll build during the course.

These are the steps you should take to get the OpenAI API token.
1. If you don't have an account yet, create one by going to [**https://platform.openai.com/**](https://platform.openai.com/). If you already have an account, skip to step 5.
2. Fill out the registration form with your name, email address, and desired password.
3. OpenAI will send you a confirmation email with a link. Click on the link to confirm your account.
4. Please note that you'll need to verify your email account and provide a phone number for verification.
5. Log in to [**https://platform.openai.com/**](https://platform.openai.com/).
6. Navigate to the API key section at [**https://platform.openai.com/account/api-keys**](https://platform.openai.com/account/api-keys).
7. Click "Create new secret key" and give the key a recognizable name or ID.

```ad-important
Note the above process will only give you the API keys for a Free account. If the codes run using Free account it will yield errors regarding not enough credits.  
We have to convert the Free account to Paid. To do so use the link [here](https://www.maisieai.com/help/how-to-get-an-openai-api-key-for-chatgpt).
```

You should take these steps to get the Deep Lake API token.
1. Sign up for an account on Activeloop's platform. You can sign up at [Activeloop's website](https://app.activeloop.ai/register). After specifying your username, click on the “Sign up” button. You should now see your homepage.
2. You should now see a “Create API token” button at the top of your homepage. Click on it, and you’ll get redirected to the “API tokens” page. This is where you can generate, manage, and revoke your API keys for accessing Deep Lake.
3. Click on the "Create API token" button. You should see a popup asking for a token name and an expiration date. By default, the token expiration date is set so that the token expires after one day from its creation, but you can set it further in the future if you want to keep using the same token for the whole duration of the course. Once you’ve set the token name and its expiration date, click the “Create API token” button.
4. You should now see a green banner saying that the token has been successfully generated, along with your new API token, on the “API tokens” page. To copy your token to your clipboard, click the square icon on its right.

```ad-note
#### Expected Cost of OpenAI Usage
By running the code samples from this course you'll make requests to the OpenAI API, incurring in costs. We expect the total cost of running all the lessons in this course, along with some experimentations, to be under _$3_.
```


# Coding Environment and Packages
Before embarking on this course, you need to ensure that you have the appropriate coding environment ready. Please make sure to use a Python version equal to, or later than **3.8.1**, which is the minimum requirement to utilize the LangChain library. You can set up your environment by choosing one of the following options:
1. Having a code editor installed on your computer. A popular coding environment is Visual Studio Code.
2. Using Python virtual environments to manage Python libraries.
3. Alternatively, you could use Google Colab notebooks.

You will need the following packages to successfully execute the sample codes provided in each lesson. They can be installed using the `pip` package manager.
```
langchain==0.0.208
deeplake==3.6.5
openai==0.27.8
tiktoken==0.4.0
```

While we strongly recommend installing the latest versions of these packages, please note that the codes have been tested with the versions specified in parentheses. as the `langchain` library is still evolving rapidly, we suggest to install a specific version of it, while installing the latest versions of the other libraries. You can do that with the following command: `pip install langchain==0.0.208 deeplake openai tiktoken`. Moreover, specific lessons may require the installation of additional packages, which will be explicitly mentioned. The following code will demonstrate how to install a package using pip.
```bash
pip install deeplake
# Or: (to install an specific version)
# pip install deeplake==3.6.5
```

# The Power and Limitations of Large Language Models
- LLMs are trained on huge amounts of text with the aim of learning the conditional distribution of words in a language. Doing so allows them to generalize and generate meaningful text without directly memorizing the training data. This means they can accurately recall widely disseminated information, such as historical events or popular cultural facts.
- However, the LLM's knowledge is restricted to its training set. So, suppose the model was trained on data up to 2021 and is asked about a company founded in 2023. In that case, it may generate a plausible but entirely fabricated description - a phenomenon known as "_hallucination_.” 
	- Managing hallucinations is tricky, especially in applications where accuracy and reliability are paramount, such as customer-service chatbots, knowledge-base assistants, or AI tutors.
- One promising strategy to mitigate hallucination is the use of retrievers in tandem with LLMs. A retriever fetches relevant information from a trusted knowledge base (like a search engine), and the LLM is then specifically prompted to rearrange the information without inventing additional details.
- LLMs' large context window sizes facilitate the inclusion of multiple documents in a single prompt. Models like GPT-4 and Claude can handle context windows of up to 32k and 100k tokens, respectively, equating to approximately 20k words or 40 pages of text. However, the cost of execution rises with the number of tokens used, hence the need for an efficient retriever to find the most relevant documents.


# Building Efficient Retrievers with Deep Lake
- Efficient retrievers are built using embedding models that map texts to vectors. These vectors are then stored in specialized databases called **vector stores**.
	- This is where **Deep Lake** comes in. 

As a data lake that doubles as a vector store for multiple data types, Deep Lake provides several advantages:
1. **Multimodal**: Deep Lake can store items of diverse modalities - text, images, audio, and video - along with their vector representations.
2. **Serverless**: The serverless nature of Deep Lake allows for the creation and management of cloud datasets without the need for a dedicated database instance. This streamlines the setup process and accelerates project development.
3. **Data Loader**: Deep Lake makes creating a streaming data loader from the loaded dataset easy, which is particularly useful for fine-tuning machine learning models using frameworks like PyTorch and TensorFlow.
4. **Querying and Visualization**: Data can be queried and visualized easily from the web.

In the context of LLM applications, Deep Lake provides a seamless way to store embeddings and their corresponding metadata. It enables hybrid searches on these embeddings and their attributes for efficient data retrieval. Moreover, as LangChain integrates with it, it facilitates the development and deployment of LLM-based applications.
As a result, Deep Lake serves as a convenient serverless memory solution for LLM chains and agents, whether for storing relevant documents for question-answering tasks or storing images for guided image-generation tasks.

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/c984e40c-bee1-46dc-8933-65bb49e3b8b0/Untitled/w=3840,quality=80)
