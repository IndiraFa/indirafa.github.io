---
layout: page-fullwidth
header:
    image_fullwidth: math_header_unsplash.jpg
    title: " "
subheadline: "Retrieval Augmented Generation"
teaser: This project was conducted as part of the Specialized Master in Artificial Intelligence, Data Expert and MlOps at Telecom Paris. We describe how to set up a RAG system in a containerized environment to ensure portability and ease of deployment. Then, we evaluate the effect of providing the LLM access to a question-and-answer math database to improve its reasoning capabilities. 
tags:
    - RAG
    - Docker
    - Streamlit
    - FastAPI
categories:
    - projects
---
<!--more-->

The full code and implementation details are available here : [github.com](https://github.com/IndiraFa/RAG_math_problem_solver)

## 1. Objective

The purpose of Retrieval-Augmented Generation (RAG) is to enrich the answers of a Large Language Model (LLM) by giving it access to a personalized knowledge base, in addition to its native training data. This project aims to build a RAG system capable of solving math problems. Indeed, LLMs are known to struggle with math and often lack robust mathematical skills and deep reasoning abilities.[^1],[^2]
We describe how to set up a RAG system in a containerized environment to ensure portability and ease of deployment. Then, we evaluate the effect of providing the LLM access to a question-and-answer math database to improve its reasoning capabilities. 

[^1]: Who's the Best Detective? Large Language Models vs. Traditional Machine Learning in Detecting Incoherent Fourth Grade Math Answers, <https://journals.sagepub.com/doi/10.1177/07356331231191174>  
[^2]: Stuck in the Quicksand of Numeracy, Far from AGI Summit: Evaluating LLMs’ Mathematical Competency through Ontology-guided Perturbations, <https://arxiv.org/html/2401.09395v1>

## 2.	How a RAG works

The macro-functioning of a Retrieval-Augmented Generation (RAG) system is depicted in Figure 1. It highlights how a LLM can integrate external knowledge retrieval to produce accurate and contextually enriched responses and enhance the relevance of the generated content.
The system combines retrieval (Step 2–3) and generation (Step 4–5). During retrieval, depending on the information given by the user, relevant chunks from the database are retrieved. During generation, a LLM produces the answer using a prompt composed of both the initial question and the retrieved data. 

![Macro functioning of the RAG system](/images/rag_macrofunctioning.png)

*Figure 1: Macro functioning of the RAG system*

The detailed process can be broken down into five steps: 
-	Step 1 - User input: The user initiates interaction by entering a prompt and query via a User Interface (UI) (option 1’) or directly via the API endpoints (option 1). This input represents the information need or question.
-	Step 2 – Query sent to the vector database: The system forwards the query via the API to a Vectorial Database. The query is typically transformed into an embedding (vector representation) and matched with relevant documents based on semantic similarity.
-	Step 3 - Retrieval of enhanced context: The vectorial database returns relevant documents or data fragments that match the user’s query. These documents provide enhanced context to improve the answer quality.
-	Step 4 - Compilation of the input to the LLM: The system sends a composite request to the LLM endpoint, consisting of the original prompt, the query, and the enhanced context retrieved from the database. This combined input allows the LLM to reason using both user input and factual external data.
-	Step 5 - Generation of response: The LLM endpoint processes the input and generates a response. This response incorporates retrieved knowledge, enabling more accurate, and domain-specific outputs. If there is a user interface, the generated response is sent back through the API to the user interface (option 5’)

Beforehand, for the system to be able to retrieve the relevant chunks, an indexation of raw documents in the vectorial database must be performed. Indeed, raw documents (text, PDFs, HTML, etc.) are typically unstructured and not searchable in a way that understands meaning. The indexation converts documents into dense vector representations (embeddings). This allows the system to retrieve relevant content based on meaning, not just keyword matching. Once documents are indexed as vectors, the system can quickly compare a user’s query vector against the pre-computed document vectors. Vector databases use techniques like Approximate Nearest Neighbor (ANN) search to efficiently find the most semantically similar documents to a query vector. This also allows for regular updates, ensuring that newly added documents can be integrated into the knowledge base.

The indexation process is described in Figure 2. [^3]

[^3]:  <https://python.langchain.com/docs/tutorials/rag/>

![Indexation process to populate te vector database](/images/indexation_process.png)

*Figure 2: Indexation process to populate te vector database*

This detailed process can be broken down into four steps: 
-	Step 1 – Loading the data: this is done using document loaders
-	Step 2 – Splitting the documents: The documents are broken into manageable chunks (for example 200-500 words). This is useful for indexing data later and passing it into a model during query, as large chunks are harder to search over and won't fit in a model's finite context window.
-	Step 3 - Embedding of the chunks: Each chunk is converted into a vector using a text embedding model.
-	Step 4 - Storage and indexing: The vectors are stored into a vector database, with associated metadata. An index is built on top to support fast similarities searches across vectors. 


## 3. Project architecture

In this project, we follow the general structure depicted in the previous section. 

-	Docker was used to deploy and run the RAG system. It is a platform that allows applications to run in isolated, lightweight containers. It simplifies the setup and ensures consistency across different environments. Packaging the RAG system (the vector database, API server, and LLM interface) into containers, makes it easier to manage dependencies, avoid compatibility issues, and streamline deployment. Using Dockerfiles and docker-compose, the entire system can be launched with a single command. This approach makes testing, iteration, and portability efficient throughout the development process.
-	FastAPI was chosen to deploy the backend of our RAG system, due to its high performance and ease of use. FastAPI allows to efficiently handle API requests between the user interface, the vector database, and the LLM components. 
-	For both embedding generation and text generation, we use Ollama. Ollama provides access to a large variety of models, including models for creating vector embeddings and LLMs for generating natural language responses. This flexibility makes it an ideal tool to support both the indexing phase and the LLM endpoint of our RAG pipeline.
-	The vector database is implemented using PostgreSQL with the PGVector extension, which allows to store and efficiently query high-dimensional vector embeddings. This setup supports similarity search, which is crucial for retrieving the most relevant context during generation.
-	For the user interface, we use Streamlit, a Python-based web framework that enables the rapid development of interactive front-end applications. It provides an intuitive way for users to input queries and view generated responses through a chat. 
-	We index a dataset from Hugging Face to add math knowledge in the RAG (see details in the Dataset choice section). 
An updated architecture scheme with the choices made for the project is depicted in Figure 3.


![Architecture of the implemented RAG system](/images/architecture.png)

*Figure 3: Architecture of the implemented RAG system*

## 4. Model choices

In our RAG system, two types of models are used, each playing a distinct role in the pipeline: a large language model (LLM) for generating answers, and an embedding model for converting text into vector representations. Both models are served and accessed locally through Ollama, a lightweight model server that simplifies loading, managing, and querying open-source models via a local API.

### 4.1.	Large Language Model (LLM)

The LLM is responsible for generating answers in natural language based on user queries and retrieved context from the knowledge base. It takes as input a prompt (which includes the user’s question and any relevant elements) and returns a coherent, context-aware answer.
We experimented with two LLMs using Ollama:

-	LLaMa 3 [^4]:
Developed by Meta, LLaMA 3 is a state-of-the-art open-weight model known for its strong reasoning and language understanding capabilities. We initially used the llama3 version with 8B parameters for its performance in generating detailed, relevant answers, and improved reasoning abilities. 
- Gemma-2b [^5]:
Gemma is a lighter, efficient open model developed by Google DeepMind. It is built from the same research and technology used to create the Gemini models. We used the gemma-2b version with 2B parameters. 
We switched to gemma in later tests for its faster inference time and lower resource usage, making it better suited to run on local hardware while still producing high-quality responses. 

These models are launched and accessed through the Ollama server, which listens on port 11434 and exposes a simple API for sending prompts and receiving responses. This allows for local, offline usage of powerful LLMs without reliance on external services.

The choice of model can be changed as a parameter in the app.py file. 
We compare the performances of the two models in the Results analysis section. 

[^4]: <https://ai.meta.com/blog/meta-llama-3/>
[^5]: <https://ai.google.dev/gemma/docs/core/model_card>

### 4.2.	Embedding model

An embedding model is used during the indexing phase of the RAG pipeline. It converts text from the reference documents into dense vector representations, which are then stored in the PGVector-enhanced PostgreSQL database. These embeddings allow the system to perform semantic search, identifying the most relevant content based on similarity to the user’s question.
We use the model *nomic-embed-text*[^6]  which is a high-performing, open-access embedding model developed by Nomic AI. It supports a large context window and produces embeddings that are well-suited for general-purpose semantic similarity tasks. Its outputs are compatible with vector databases and work effectively in RAG systems for document retrieval.
Like the LLM, the embedding model is served by Ollama, making it easy to switch or test models by changing configuration parameters (typically: model="nomic-embed-text").

[^6]: <https://huggingface.co/nomic-ai/nomic-embed-text-v1.5>

## 5.	Dataset choice

As knowledge base, we use the Orca-Math dataset from Hugging Face. [^7],[^8]  This dataset contains around 200K grade school math world problems. It was specifically designed to enhance the mathematical abilities of language models. 
A typical data entry in the dataset consists of a question and its corresponding answer. 

During the indexing of the data in the vector database, each question and its corresponding answer are combined into a single string and embedded as one vector in the vector database. The obtained embedding represents the combined meaning of the question-answer pair, which is useful for our RAG pipeline, where we want to retrieve semantically relevant questions and answers based on the user’s query. 


[^7]: <https://huggingface.co/datasets/microsoft/orca-math-word-problems-200k>
[^8]: Mitra, A., Khanpour, H., Rosset, C., & Awadallah, A. (2024). Orca-math: Unlocking the potential of slms in grade school math. arXiv preprint arXiv:2402.14830.

## 6. Results

Environment set up steps and rag usage step-by-step are detailed on  [the github page of the project](https://github.com/IndiraFa/RAG_math_problem_solver).

The FastAPI backend interface and Streamlit cat interface are visible in figures 4 and 5. 

![FastAPI backend interface to easily run requests](/images/backend.png)

*Figure 4: FastAPI backend interface to easily run requests*

![Streamlit interface when querying in the chat](/images/streamlit.png)

*Figure 5: Streamlit interface when querying in the chat*

Due to the long indexing time, we indexed 500 question-and-answer items from the dataset, which was sufficient to evaluate the performance of the RAG system. We used the separate endpoints created (/llm-only and /ask) to compare the results with and without RAG-based information retrieval. We also compared the two selected models: llama3 (8B parameters) and gemma:2b (2B parameters), which differ significantly in size and are expected to differ in performance as well.

Table 1 and Table 2 below show a detailed example of performance with and without information retrieval for both models. 

| Source     | Corresponding text     |
|----------------|----------------|
| Input = question indexed in the database | Namjoon has 3 sets of 7 marbles. He is going to make these marbles into 6 groups of 6 each. **How many more marbles does he need?** |
| Answer indexed in the database | Namjoon has 3 sets of 7 marbles, so he has a total of 3 * 7 = 21 marbles. He wants to make 6 groups of 6 marbles each, which means he needs a total of 6 * 6 = 36 marbles. To find out how many more marbles he needs, we subtract the number of marbles he has from the number he needs: 36 (needed) - 21 (has) = 15 marbles. Namjoon needs **15 more marbles** to make 6 groups of 6.|
|Answer provided by the RAG with Gemma | Namjoon needs **<span style="color:green;">15 more marbles</span>**  to make 6 groups of 6. |
| Answer provided by Gemma only| Namjoon needs 3 sets of 7 marbles each, which is a total of 3 * 7 = 21 marbles. Therefore, he needs 21 + 3 = **<span style="color:red;">24 more marbles </span>** to make 6 groups of 6 each.|
| Answer provided by the RAG with Llama3|Namjoon has 21 marbles and needs a total of 36 marbles to make 6 groups of 6 marbles each. To find out how many more marbles he needs, we subtract the number of marbles he has from the number he needs: 36 (needed) - 21 (has) = 15 marbles. **<span style="color:green;">Namjoon needs 15 more marbles to make 6 groups of 6.</span>** |
| Answer provided by Llama3 only| If Namjoon starts with 3 sets of 7, that means he started with 3 x 7 = 21 marbles. Now, if he wants to put them in 6 groups of 6, then he needs a total of 6 * 6 = 36 marbles. Since he has 21, he will need another 36 - 21 = 15. **<span style="color:green;">The answer is 15.</span>** |

*Table 1: Example of answers of the different models based on an input provided in the dataset*


|Source	|Corresponding text|
|----------------|----------------|
|Input = question similar but different from the database	|**Paul** has 3 sets of 7 marbles. He is going to make these marbles into **6 groups of 5 each**. How many more marbles does he need?||
Expected answer|	Paul needs **9 more marbles**|
|Answer provided by the RAG with Gemma	|The passage does not specify how many more marbles Paul needs, so **<span style="color:red;">I cannot answer this question </span>** from the provided context.|
|Answer provided by Gemma only	|To make 6 groups of 5 marbles each, Paul needs to make 6 * 5 = 30 more marbles. Therefore, **<span style="color:red;">Paul needs 30 more marbles</span>** .|
|Answer provided by the RAG with Llama3	|Let's work it out! Paul has a total of 3 sets of 7 marbles, so he has: 3 * 7 = 21 marbles. He wants to make 6 groups of 5 marbles each, which means he needs a total of:\n\n6 * 5 = 30 marbles. To find out how many more marbles he needs, we subtract the number of marbles he has from the number he needs: 30 (needed) - 21 (has) = 9 marbles **<span style="color:green;">Paul needs 9 more marbles to make 6 groups of 5.</span>** |
|Answer provided by Llama3 only	|He started with 7 x 3 = 21 marbles. To get 6 groups of 5, that means he needs 30 marbles in total So he will be short by 9. **<span style="color:green;">The answer is 9.</span>** |

 
*Table 2: Example of answers of the different models based on an input slightly differing from the dataset*

Table 1 presents the results when asking a question that is directly answered in the database. Gemma alone attempts to reason but fails to provide a correct answer. With information retrieval, however, Gemma produces the correct answer, demonstrating the positive impact of the RAG system on response relevance. This table also illustrates LLaMA3’s superior reasoning ability, as it provides the correct answer both with and without retrieval.

In Table 2, we modified the input to pose a very similar problem with slight changes. Gemma fails to answer correctly, whereas LLaMA3 continues to respond accurately. Notably, when the Gemma model alone cannot answer, it tends to fabricate a response. In contrast, the retrieval-augmented system acknowledges the lack of sufficient information, which can be more helpful to the user.

We also observed cases where, even with retrieval, LLaMA3 cannot answer a question whose answer is actually in the database, while Gemma does provide the correct response. However, when the question is slightly altered, neither model succeeds in giving a correct answer.

Finally, an interesting case showed that when asked a question directly from the database, Gemma benefits from retrieval, while LLaMA3 shows stronger native reasoning abilities. However, when the question is modified and some context is removed, only LLaMA3 with retrieval is able to answer correctly. This highlights LLaMA3’s ability to combine its own reasoning skills with retrieved information to adapt its response appropriately.



## 7. Conclusion and next steps

In this report, we described how to build a Retrieval-Augmented Generation (RAG) system designed to solve math problems. The proposed setup is flexible and modular: it supports various LLMs and embedding models, and can be used with various datasets. Additionally, we developed a user-friendly Streamlit interface to facilitate interaction with the system.
Our experiments demonstrate that the RAG setup successfully helps the LLM to answer math problems that are present in the database, confirming that the retrieval component is functioning correctly. However, we observed that this setup does not significantly enhance the model's global reasoning capabilities.
A comparison between Gemma and LLaMA3 highlights important differences: while Gemma benefits from retrieval for known problems, it lacks the ability to extrapolate and reason beyond the retrieved content. In contrast, LLaMA3 consistently exhibits stronger reasoning skills, both with and without retrieval, making it more adaptable in complex or unfamiliar problem settings. 
We notably observed LLaMA3’s ability to combine its own reasoning skills with retrieved information to adapt its response appropriately. These findings highlight the importance of model choice when deploying RAG systems for tasks requiring deep reasoning.

Future work could explore ways to improve this setup, particularly by integrating techniques to enhance the reasoning capabilities of LLMs. Evaluating additional LLMs, especially those optimized for mathematical reasoning, may yield improved performance. Moreover, experimenting with larger and more diverse datasets could provide further insights into the limits and potential of RAG in mathematical problem-solving. 

To move toward real-world deployment, it will be essential to scale up the system, optimize indexing time, and improve retrieval speed. This could be achieved by using GPU and more efficient vector databases as well as implementing batch indexing pipelines and asynchronous retrieval mechanisms. Adapting the system for a production environment will also require container orchestration (e.g., with Kubernetes), proper API management, and monitoring tools to ensure reliability, scalability, and performance under real-world usage.




