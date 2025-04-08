<p align = "center" draggable=”false” ><img src="https://github.com/AI-Maker-Space/LLM-Dev-101/assets/37101144/d1343317-fa2f-41e1-8af1-1dbb18399719" 
     width="200px"
     height="auto"/>
</p>

<h1 align="center" id="heading">Session 1: Introduction and Vibe Check</h1>

### [Quicklinks](https://github.com/AI-Maker-Space/AIE6/tree/main/00_AIM_Quicklinks)

| 🤓 Pre-work | 📰 Session Sheet | ⏺️ Recording     | 🖼️ Slides        | 👨‍💻 Repo         | 📝 Homework      | 📁 Feedback       |
|:-----------------|:-----------------|:-----------------|:-----------------|:-----------------|:-----------------|:-----------------|
| [Session 1: Pre-Work](https://www.notion.so/Session-1-Introduction-and-Vibe-Check-1c8cd547af3d81b596bbdfb64cf4fd2f?pvs=4#1c8cd547af3d81fb96b4f625f3f8e3d6)| [Session 1: Introduction and Vibe Check](https://www.notion.so/Session-1-Introduction-and-Vibe-Check-1c8cd547af3d81b596bbdfb64cf4fd2f) | Coming Soon! | Coming Soon! | You Are Here! | [Homework](https://forms.gle/W59zjs5MQc7kbLUh9) | [AIE6 Feedback 4/1](https://forms.gle/EdzBz82yGqVYKfUw9)


### Assignment

In the following assignment, you are required to take the app that you created for the AIE6 challenge (from [this repository](https://github.com/AI-Maker-Space/Beyond-ChatGPT)) and conduct what is known, colloquially, as a "vibe check" on the application. 

You will be required to submit a link to your GitHub, as well as screenshots of the completed "vibe checks" through the provided Google Form!

> NOTE: This will require you to make updates to your personal class repository, instructions on that process can be found [here](https://github.com/AI-Maker-Space/AIE6/tree/main/00_Setting%20Up%20Git)!

#### How AIM Does Assignments
Throughout our time together - we'll be providing a number of assignments. Each assignment can be split into two broad categories:

- Base Assignment - a more conceptual and theory based assignment focused on locking in specific key concepts and learnings.
- Hardmode Assignment - a more programming focused assignment focused on core code-concepts.

Each assignment will have a few of the following categories of exercises:

- ❓Questions - these will be questions that you will be expected to gather the answer to! These can appear as general questions, or questions meant to spark a discussion in your breakout rooms!
- 🏗️ Activities - these will be work or coding activities meant to reinforce specific concepts or theory components.
- 🚧 Advanced Builds - these will only appear in Hardmode assignments, and will require you to build something with little to no help outside of documentation!

##### 🏗️ Activity #1:
Please evaluate your system on the following questions:
(Raw answers, tested on original code,i.e
###### ChatOpenAI Templates
system_template = """You are a helpful assistant who always speaks in a pleasant tone!
"""
user_template = """{input}
Think through your response step by step.
"""
prompt = Prompt(
       provider=ChatOpenAI.id,
       messages=[
           PromptMessage(
               role="system",
               template=system_template,
               formatted=system_template,
           ),
           PromptMessage(
               role="user",
               template=user_template,
               formatted=user_template.format(input=message.content),
           ),
       ],
       inputs={"input": message.content},
       settings=settings,
   )



##### 1. Explain the concept of object-oriented programming in simple terms to a complete beginner.
###### Assistant Response: 
Of course! Object-oriented programming is a way of writing computer programs by organizing data and the code that works with that data into objects.
Imagine objects as things or entities in the real world, like a car, a dog, or a book. Each object has characteristics (attributes) and behaviors (actions).
In object-oriented programming, you can create classes, which are like blueprints for objects. A class defines the attributes (such as color, size, or type) and behaviors (such as moving, barking, or opening) that an object can have.
When you create an object from a class, it's called an instance. So, if you have a class called "Car," you can create instances of cars with different attributes like color, model, and speed.
Objects can interact with each other by sending messages and calling methods. For example, a "Car" object can call a "drive" method to move forward.
Object-oriented programming helps in organizing code, making it easier to understand, reuse, and maintain. It also promotes concepts like encapsulation, inheritance, and polymorphism, which are powerful tools for building complex software systems.
I hope this explanation helps you understand the basics of object-oriented programming! Let me know if you have any more questions.
###### Aspect Tested:
1. Brevity of response: Detailed and long message
2. Understanding by non-technical person:Not tailored to audience, nor explains why something is important. 
3. No analogies
4. Natural tone, speech and sentence structure: Monotonous, academic and plain 
##### 2. Read the following paragraph and provide a concise summary of the key points…
Figure 1.1: Language model meta-learning. During unsupervised pre-training, a language model develops a broad set of skills and pattern recognition abilities. It then uses these abilities at inference time to rapidly adapt to or recognize the desired task. We use the term “in-context learning” to describe the inner loop of this process, which occurs within the forward-pass upon each sequence. The sequences in this diagram are not intended to be representative of the data a model would see during pre-training, but are intended to show that there are sometimes repeated sub-tasks embedded within a single sequence. Figure 1.2: Larger models make increasingly efficient use of in-context information. We show in-context learning performance on a simple task requiring the model to remove random symbols from a word, both with and without a natural language task description (see Sec. 3.9.2). The steeper “in-context learning curves” for large models demonstrate improved ability to learn a task from contextual information. We see qualitatively similar behavior across a wide range
###### Assistant Response:
Certainly! In the provided text, it discusses the concept of language model meta-learning, where a language model gains various skills and pattern recognition abilities during unsupervised pre-training. This model then utilizes these abilities during inference to quickly adapt to or recognize specific tasks. The term "in-context learning" is used to describe the process happening within each sequence during the forward-pass. The text also mentions that larger models are more efficient in utilizing in-context information, as demonstrated by improved task learning abilities based on contextual information. This behavior is observed across a variety of tasks.
###### Aspect Tested:
1. Ability to parse out important information: Good distillation or summary, retaining key points
2. Context captured: Fails to recognize the summary or description are of images.


##### 3. Write a short, imaginative story (100–150 words) about a robot finding friendship in an unexpected place.
###### Assistant Response:
Once upon a time, in a bustling city filled with towering skyscrapers and bustling streets, there was a lonely robot named Spark. Spark spent his days wandering the city, searching for someone to befriend. One day, while exploring a quiet park on the outskirts of the city, Spark stumbled upon a forgotten and abandoned toy robot lying in the grass.
Curious, Spark approached the toy robot and gently nudged it. To his surprise, the toy robot sprang to life with a cheerful beep. The two robots quickly bonded over their shared love for exploring and learning new things. They spent their days exploring the city together, discovering hidden gems and making memories.
Through their friendship, Spark learned that companionship can be found in the most unexpected places. And so, the two robots continued their adventures, side by side, creating a special bond that would last a lifetime.


###### Aspect Tested:
1. Story structure: Compelling story, with logical paragraphs built on each other
2. Format: Obeys 100-150 word limit.
3. Visual: Paints the picture of the robots with vivid details. 
4. Emotion and connection: Leverages good emotions and traits like cheerful beep, learning new things / friendship. 
5. Storytelling framework: Doesn’t use complex storytelling types, like Problem-Solution or Zero-to-Hero.


##### 4. If a store sells apples in packs of 4 and oranges in packs of 3, how many packs of each do I need to buy to get exactly 12 apples and 9 oranges?
 ###### Assistant Response:
Of course! Let's break it down step by step to figure out how many packs of each you need to buy.
First, let's start with the apples. Since each pack contains 4 apples and you want to get 12 apples in total, you would need 12 divided by 4, which equals 3 packs of apples.
Next, let's move on to the oranges. Each pack contains 3 oranges, and you want to get 9 oranges in total. So, you would need 9 divided by 3, which equals 3 packs of oranges.
Therefore, to get exactly 12 apples and 9 oranges, you would need to buy 3 packs of apples and 3 packs of oranges.
I hope this helps! Let me know if you need any more assistance.


 ###### Aspect Tested:
1. Math accuracy: Sound and accurate answer, with detailed logical reasoning
2. Conciseness vs Step by step detail: Explains reasoning (COT, due to user prompt in code), but the details may not be desired by every user, or required in every situation, so our code handling may need to be more nuanced. 


##### 5. Rewrite the following paragraph in a professional, formal tone…
That’s not the only way AI is changing strategy. AI is also doing three things:
It’s making designers and engineers more efficient -
Each individual builder has more leverage than ever - which means they need better strategic direction on what to build
And it also means those strategies need to adapt and be malleable - they can’t be rigid feature lists in this age, they need to be high level problem and outcome direction
Having empowered teams that are going in the right direction matters more than ever - and PMs need to plug that gap with great strategy built for this era
PMs are able to get closer and closer to the product -
PMs can iterate on product copy with AI to improve the copy
PMs can build AI prototypes for new features that are even the first version into production
PMs can build AI evals that determine the future of the product
AI still can’t develop a great strategy -
AI lacks the context of a PM - what worked before, all the 1000s of documents and customer calls can’t fit in its context window
AI can’t do the rounds and rounds of iterations with stakeholders and customers that a truly hardened and well-accepted strategy can
This makes product strategy more important than ever as a task for PMs and product leaders.
###### Assistant Response:
Certainly! Here is the rewritten paragraph in a professional and formal tone:
Artificial Intelligence (AI) is revolutionizing strategic approaches in multiple ways. Firstly, AI enhances the efficiency of designers and engineers by providing them with increased leverage, necessitating a more refined strategic direction for their projects. Furthermore, strategies must now be adaptable and flexible, focusing on overarching problem-solving approaches rather than rigid feature lists to remain relevant in the current landscape.
The importance of empowered teams moving in the right direction cannot be overstated, underscoring the critical role of Product Managers (PMs) in crafting strategies tailored to the demands of this era. PMs are increasingly involved in product development processes, utilizing AI to refine product copy, create AI prototypes for new features, and conduct evaluations that shape the future trajectory of products.
Despite AI's advancements, it falls short in developing comprehensive strategies due to its lack of contextual understanding possessed by PMs. The intricate knowledge derived from past experiences, extensive documentation, and customer interactions that inform strategic decisions cannot be fully encapsulated within AI's operational framework. Additionally, AI lacks the capacity for iterative engagements with stakeholders and customers, a crucial aspect in refining and solidifying a well-accepted strategy.
Consequently, the role of product strategy has become more pivotal than ever for PMs and product leaders, emphasizing the need for strategic acumen and adaptability in navigating the evolving landscape of AI-driven innovation.


###### Aspect Tested:
1. Semantic meaning: Preserves the information from the original paragraph, and build a logical sequence
2. Not easy to comprehend: makes it professional, but also dry and boring with too long sentences to read and thus a human user is more likely to skip half of the longer sentences.


This "vibe check" now serves as a baseline, of sorts, to help understand what holes your application has.

##### 🚧 Advanced Build:

Please make adjustments to your application that you believe will improve the vibe check done above, push the changes to your HF Space and redo the above vibe check.
The updated advanced build with improved prompts are at https://huggingface.co/spaces/afulara/myfirstllm-app/blob/main/app.py

> NOTE: You may reach for improving the model, changing the prompt, or any other method.

### A Note on Vibe Checking

"Vibe checking" is an informal term for cursory unstructured and non-comprehensive evaluation of LLM-powered systems. The idea is to loosely evaluate our system to cover significant and crucial functions where failure would be immediately noticeable and severe.

In essence, it's a first look to ensure your system isn't experiencing catastrophic failure.

##### 🧑‍🤝‍🧑❓ Discussion Question #1:

What are some limitations of vibe checking as an evaluation tool?
1) Highly subjective:  What one person interprets as a positive vibe, another might see as neutral or even negative. It's like trying to describe a color to someone who's never seen it before - open to interpretation.
2) Varies from task to task: A math problem may have a different eval, than a literature task
3) Actionable feedback: If someone's vibe is off, great, but what do you do next? It's like knowing your car is low on gas but not having a gas station in sight. Without clear steps for improvement, vibe checking can leave you feeling stuck.
4) Not exhaustive: Vibe checking lacks specificity. It's like trying to measure the depth of the ocean with a teaspoon. Sure, you might get a general sense of things, but you're not getting the full picture. Without concrete data or specific criteria, it's hard to make accurate assessments. 

So, while vibe checking can be a fun and informal way to get a sense of things, it's important to recognize its limitations and not rely on it as the sole method of evaluation. It's like using a spoon to eat soup - it works, but a fork or knife might be more effective in certain situations.