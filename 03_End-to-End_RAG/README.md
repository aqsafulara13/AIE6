<p align = "center" draggable=”false” ><img src="https://github.com/AI-Maker-Space/LLM-Dev-101/assets/37101144/d1343317-fa2f-41e1-8af1-1dbb18399719" 
     width="200px"
     height="auto"/>
</p>

## <h1 align="center" id="heading">Session 3: End-to-End RAG</h1>

### [Quicklinks](https://github.com/AI-Maker-Space/AIE6/tree/main/00_AIM_Quicklinks)

| 🤓 Pre-work | 📰 Session Sheet | ⏺️ Recording     | 🖼️ Slides        | 👨‍💻 Repo         | 📝 Homework      | 📁 Feedback       |
|:-----------------|:-----------------|:-----------------|:-----------------|:-----------------|:-----------------|:-----------------|
| [Session 3: Pre-Work](https://www.notion.so/Session-3-End-to-End-RAG-Deployment-and-2025-Industry-Use-Cases-1c8cd547af3d818fb863ce11710a31ec?pvs=4#1c8cd547af3d81468dbadb4d669435bd)| [Session 3: End-to-End RAG Deployment and 2025 Industry Use Cases ](https://www.notion.so/Session-3-End-to-End-RAG-Deployment-and-2025-Industry-Use-Cases-1c8cd547af3d818fb863ce11710a31ec) | [Recording](https://us02web.zoom.us/rec/share/sGuBqB5go4WPjALRA5y_QkzDHju0qiAtSanlWN8FZLV8mdviZWAcg8fHU8J-hbmp.HlMKdznBNkeB-CgB) (?0&gz5BF)| [Session 3: Industry Use Cases & End-to-End RAG](https://www.canva.com/design/DAGjaej4jRA/W4Vw66N4gbJWvMvojgR5Dg/edit?utm_content=DAGjaej4jRA&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton) |You Are Here! | [Session 3 Assignment: End-to-End RAG](https://forms.gle/KwkHPyCN6zb1ypno7)| [AIE6 Feedback 4/8](https://forms.gle/oVeEd95qajYVhryF6)

Building off last week, we're going to take our Pythonic RAG application to the next level!

## Getting Started

This week, we'll be working out of the [AIE6-DeployPythonicRAG repository](https://github.com/AI-Maker-Space/AIE6-DeployPythonicRAG)!

> NOTE: IT IS CRITICAL THAT YOU DO NOT CLONE THE NEW REPOSITORY INTO YOUR EXISTING AIE5 REPOSITORY. THIS WILL CAUSE MANY HEADACHES AND GIT-RELATED SADNESS.

Clone the repository with the command: 

```bash
cd ~
git clone git@github.com:AI-Maker-Space/AIE6-DeployPythonicRAG.git
```

Navigate to that repository - and follow the instructions!

Homework


---
title: DeployPythonicRAG
emoji: 📉
colorFrom: blue
colorTo: purple
sdk: docker
pinned: false
license: apache-2.0
---

# Deploying Pythonic Chat With Your Text File Application

In today's breakout rooms, we will be following the process that you saw during the challenge.

Today, we will repeat the same process - but powered by our Pythonic RAG implementation we created last week. 

You'll notice a few differences in the `app.py` logic - as well as a few changes to the `aimakerspace` package to get things working smoothly with Chainlit.

> NOTE: If you want to run this locally - be sure to use `uv run chainlit run app.py` to start the application outside of Docker.

## Reference Diagram (It's Busy, but it works)

![image](https://i.imgur.com/IaEVZG2.png)

### Anatomy of a Chainlit Application

[Chainlit](https://docs.chainlit.io/get-started/overview) is a Python package similar to Streamlit that lets users write a backend and a front end in a single (or multiple) Python file(s). It is mainly used for prototyping LLM-based Chat Style Applications - though it is used in production in some settings with 1,000,000s of MAUs (Monthly Active Users).

The primary method of customizing and interacting with the Chainlit UI is through a few critical [decorators](https://blog.hubspot.com/website/decorators-in-python).

> NOTE: Simply put, the decorators (in Chainlit) are just ways we can "plug-in" to the functionality in Chainlit. 

We'll be concerning ourselves with three main scopes:

1. On application start - when we start the Chainlit application with a command like `uv run chainlit run app.py`
2. On chat start - when a chat session starts (a user opens the web browser to the address hosting the application)
3. On message - when the users sends a message through the input text box in the Chainlit UI

Let's dig into each scope and see what we're doing!

### On Application Start:

The first thing you'll notice is that we have the traditional "wall of imports" this is to ensure we have everything we need to run our application. 

```python
import os
from typing import List
from chainlit.types import AskFileResponse
from aimakerspace.text_utils import CharacterTextSplitter, TextFileLoader
from aimakerspace.openai_utils.prompts import (
    UserRolePrompt,
    SystemRolePrompt,
    AssistantRolePrompt,
)
from aimakerspace.openai_utils.embedding import EmbeddingModel
from aimakerspace.vectordatabase import VectorDatabase
from aimakerspace.openai_utils.chatmodel import ChatOpenAI
import chainlit as cl
```

Next up, we have some prompt templates. As all sessions will use the same prompt templates without modification, and we don't need these templates to be specific per template - we can set them up here - at the application scope. 

```python
system_template = """\
Use the following context to answer a users question. If you cannot find the answer in the context, say you don't know the answer."""
system_role_prompt = SystemRolePrompt(system_template)

user_prompt_template = """\
Context:
{context}

Question:
{question}
"""
user_role_prompt = UserRolePrompt(user_prompt_template)
```

> NOTE: You'll notice that these are the exact same prompt templates we used from the Pythonic RAG Notebook in Week 1 Day 2!

Following that - we can create the Python Class definition for our RAG pipeline - or *chain*, as we'll refer to it in the rest of this walkthrough. 

Let's look at the definition first:

```python
class RetrievalAugmentedQAPipeline:
    def __init__(self, llm: ChatOpenAI(), vector_db_retriever: VectorDatabase) -> None:
        self.llm = llm
        self.vector_db_retriever = vector_db_retriever

    async def arun_pipeline(self, user_query: str):
        ### RETRIEVAL
        context_list = self.vector_db_retriever.search_by_text(user_query, k=4)

        context_prompt = ""
        for context in context_list:
            context_prompt += context[0] + "\n"

        ### AUGMENTED
        formatted_system_prompt = system_role_prompt.create_message()

        formatted_user_prompt = user_role_prompt.create_message(question=user_query, context=context_prompt)


        ### GENERATION
        async def generate_response():
            async for chunk in self.llm.astream([formatted_system_prompt, formatted_user_prompt]):
                yield chunk

        return {"response": generate_response(), "context": context_list}
```

Notice a few things:

1. We have modified this `RetrievalAugmentedQAPipeline` from the initial notebook to support streaming. 
2. In essence, our pipeline is *chaining* a few events together:
    1. We take our user query, and chain it into our Vector Database to collect related chunks
    2. We take those contexts and our user's questions and chain them into the prompt templates
    3. We take that prompt template and chain it into our LLM call
    4. We chain the response of the LLM call to the user
3. We are using a lot of `async` again!

Now, we're going to create a helper function for processing uploaded text files.

First, we'll instantiate a shared `CharacterTextSplitter`.

```python
text_splitter = CharacterTextSplitter()
```

Now we can define our helper.

```python
def process_file(file: AskFileResponse):
    import tempfile
    import shutil
    
    print(f"Processing file: {file.name}")
    
    # Create a temporary file with the correct extension
    suffix = f".{file.name.split('.')[-1]}"
    with tempfile.NamedTemporaryFile(delete=False, suffix=suffix) as temp_file:
        # Copy the uploaded file content to the temporary file
        shutil.copyfile(file.path, temp_file.name)
        print(f"Created temporary file at: {temp_file.name}")
        
        # Create appropriate loader
        if file.name.lower().endswith('.pdf'):
            loader = PDFLoader(temp_file.name)
        else:
            loader = TextFileLoader(temp_file.name)
            
        try:
            # Load and process the documents
            documents = loader.load_documents()
            texts = text_splitter.split_texts(documents)
            return texts
        finally:
            # Clean up the temporary file
            try:
                os.unlink(temp_file.name)
            except Exception as e:
                print(f"Error cleaning up temporary file: {e}")
```

Simply put, this downloads the file as a temp file, we load it in with `TextFileLoader` and then split it with our `TextSplitter`, and returns that list of strings!

#### ❓ QUESTION #1:

Why do we want to support streaming? What about streaming is important, or useful?

####  Answer #1:

Streaming allows users to see partial responses as the LLM continues to output rest of response. This behaviour enables users to get faster feedback, since they don't need to wait for full response. It also leads to better UX, since streaming feels interactive. Finally, it helps with performance, by reducing memory usage and prevent timeouts. ie. if the answers are especially long, by streaming chunks, we don't need to hold in memory. 
This streaming behaviour is enabled through the astream function: (async for chunk in self.llm.astream(...)) which yields partial LLM responses. 
####  End of Answer #1
### On Chat Start:

The next scope is where "the magic happens". On Chat Start is when a user begins a chat session. This will happen whenever a user opens a new chat window, or refreshes an existing chat window.

You'll see that our code is set-up to immediately show the user a chat box requesting them to upload a file. 

```python
while files == None:
        files = await cl.AskFileMessage(
            content="Please upload a Text or PDF file to begin!",
            accept=["text/plain", "application/pdf"],
            max_size_mb=2,
            timeout=180,
        ).send()
```

Once we've obtained the text file - we'll use our processing helper function to process our text!

After we have processed our text file - we'll need to create a `VectorDatabase` and populate it with our processed chunks and their related embeddings!

```python
vector_db = VectorDatabase()
vector_db = await vector_db.abuild_from_list(texts)
```

Once we have that piece completed - we can create the chain we'll be using to respond to user queries!

```python
retrieval_augmented_qa_pipeline = RetrievalAugmentedQAPipeline(
        vector_db_retriever=vector_db,
        llm=chat_openai
    )
```

Now, we'll save that into our user session!

> NOTE: Chainlit has some great documentation about [User Session](https://docs.chainlit.io/concepts/user-session). 

#### ❓ QUESTION #2: 

Why are we using User Session here? What about Python makes us need to use this? Why not just store everything in a global variable?

####  Answer #2:

1) User Session enables us to keep each user's data isolated. If we were to use globally single session, it would be leak user's data into each other, causing privacy issues. A session is unique to each user or each connection, creating boundaries.
2) User Session also enables better session lifecycle management, as sessions are automatically cleaned up (eg: logout, session timeout, disconnect). Global session can lead to memory bloat.
3) User Session also enables clean threads, preventing concurrency bugs or race conditions, as async threads running simultaneouly are still contained in their own 'box'.

####  End of Answer #2

### On Message

First, we load our chain from the user session:

```python
chain = cl.user_session.get("chain")
```

Then, we run the chain on the content of the message - and stream it to the front end - that's it!

```python
msg = cl.Message(content="")
result = await chain.arun_pipeline(message.content)

async for stream_resp in result["response"]:
    await msg.stream_token(stream_resp)
```

### 🎉

With that - you've created a Chainlit application that moves our Pythonic RAG notebook to a Chainlit application!

## Deploying the Application to Hugging Face Space

Due to the way the repository is created - it should be straightforward to deploy this to a Hugging Face Space!

> NOTE: If you wish to go through the local deployments using `chainlit run app.py` and Docker - please feel free to do so!

<details>
    <summary>Creating a Hugging Face Space</summary>

1.  Navigate to the `Spaces` tab.

![image](https://i.imgur.com/aSMlX2T.png)

2. Click on `Create new Space`

![image](https://i.imgur.com/YaSSy5p.png)

3. Create the Space by providing values in the form. Make sure you've selected "Docker" as your Space SDK.

![image](https://i.imgur.com/6h9CgH6.png)

</details>

<details>
    <summary>Adding this Repository to the Newly Created Space</summary>

1. Collect the SSH address from the newly created Space. 

![image](https://i.imgur.com/Oag0m8E.png)

> NOTE: The address is the component that starts with `git@hf.co:spaces/`.

2. Use the command:

```bash
git remote add hf HF_SPACE_SSH_ADDRESS_HERE
```

3. Use the command:

```bash
git pull hf main --no-rebase --allow-unrelated-histories -X ours
```

4. Use the command: 

```bash 
git add .
```

5. Use the command:

```bash
git commit -m "Deploying Pythonic RAG"
```

6. Use the command: 

```bash
git push hf main
```

7. The Space should automatically build as soon as the push is completed!

> NOTE: The build will fail before you complete the following steps!

</details>

<details>
    <summary>Adding OpenAI Secrets to the Space</summary>

1. Navigate to your Space settings.

![image](https://i.imgur.com/zh0a2By.png)

2. Navigate to `Variables and secrets` on the Settings page and click `New secret`: 

![image](https://i.imgur.com/g2KlZdz.png)

3. In the `Name` field - input `OPENAI_API_KEY` in the `Value (private)` field, put your OpenAI API Key.

![image](https://i.imgur.com/eFcZ8U3.png)

4. The Space will begin rebuilding!

</details>

## 🎉

You just deployed Pythonic RAG!

Try uploading a text file and asking some questions!

#### ❓ Discussion Question #1:

Upload a PDF file of the recent DeepSeek-R1 paper and ask the following questions:

1. What is RL and how does it help reasoning?
2. What is the difference between DeepSeek-R1 and DeepSeek-R1-Zero?
3. What is this paper about?

Does this application pass your vibe check? Are there any immediate pitfalls you're noticing?

####  Answers :

1) Reinforcement Learning (RL) is a training approach that allows models to learn from interactions with an environment by receiving rewards for desired behaviors. In the context of DeepSeek-R1-Zero, RL helps enhance reasoning capabilities by enabling the model to explore and evolve its reasoning patterns autonomously, without the need for supervised fine-tuning (SFT). This leads to the development of effective and innovative reasoning behaviors, resulting in significant performance improvements on reasoning tasks.

2) DeepSeek-R1-Zero is a more advanced model that showcases improved performance in reasoning tasks through reinforcement learning (RL), whereas DeepSeek-R1 currently falls short in capabilities such as function calling, multi-turn interactions, and complex role-playing. Additionally, DeepSeek-R1 is optimized for Chinese and English and exhibits language mixing issues, while DeepSeek-R1-Zero demonstrates a strong foundational capability without such limitations.

3) The provided context references various papers related to advancements in large language models, mathematical reasoning, evaluation benchmarks, and reinforcement learning, among other topics in artificial intelligence. However, it does not specify the content or focus of any single paper. Therefore, I don't know what specific paper you are inquiring about.
####  End of Answers

## 🚧 CHALLENGE MODE 🚧

For the challenge mode, please instead create a simple FastAPI backend with a simple React (or any other JS framework) frontend.

You can use the same prompt templates and RAG pipeline as we did here - but you'll need to modify the code to work with FastAPI and React.

Deploy this application to Hugging Face Spaces!

https://huggingface.co/spaces/afulara/PythonicRAG-FastAPI-React


