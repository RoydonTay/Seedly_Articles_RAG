# Retrieval Augmented Generation with Seedly Articles

## Outline
In this project, I built an RAG with a quantized llama2 model: llama-2-7b-chat.Q4_K_M.gguf downloaded from [TheBloke/Llama-2-7B-Chat-GGUF](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGUF/tree/main). The documents used for RAG were retrieved via webscrapping of the [Seedly Blog](https://blog.seedly.sg), which contains articles about personal finance. The articles I retrieved were mostly about purchasing property in Singapore and insurance policies. The aim is to develop a language model that is more contextually aware and capable of answering questions related to personal finance within the context of Singapore.

## Method
For the webscrapping, I used the [Scrapy](https://scrapy.org) to retrieve the text from the articles. I used [FastEmbed](https://github.com/qdrant/fastembed) for conversion of text chunks into embeddings, and used [FAISS](https://github.com/facebookresearch/faiss) vector store for text storage and retrieval. Finally, I used [LangChain](https://www.langchain.com) to interface all the different components, from retrieval of text chunks to prompt structuring and chaining to achieve a desired output.

The final output is generated by chaining two prompts, the first one to summarize context provided (top 2 most similar text chunks to the question user asked), and the second one to generate final response. This is to ensure the prompt used to generate output does not become too long (containing full-length text chunks as context), reducing the context window available for generation of output.

- Webscrapping Script: [article_spider.py](https://github.com/RoydonTay/Seedly_Articles_RAG/blob/main/seedly_scrape/spiders/article_spider.py)
- Creating Vector Store: [FAISS_db.py](https://github.com/RoydonTay/Seedly_Articles_RAG/blob/main/FAISS_db.py)
- RAG implementation: [seedly_RAG.py](https://github.com/RoydonTay/Seedly_Articles_RAG/blob/main/seedly_RAG.py)

## Results
To assess the outputs of the RAG, I compared it to that of the base LLM. The implementation of RAG did not significantly change outputs from base LLM for some questions. For some questions asked, the model also hardly uses the context provided. This is probably because additional information provided by RAG in this project is retrieved from the internet, which is very similar to the data the base model was trained on. See [Experiment_logs.txt](https://github.com/RoydonTay/Seedly_Articles_RAG/blob/main/Experiment_logs.txt) for comparison of outputs.

The chaining of prompts works as expected, with summarization of text chunks successfully added to second prompt. The method may improve model's response if it provides information previously unavailable to model.

## Reflections
Doing this project, I learnt how to implement an RAG using LangChain, how to interface an LLM with prompt templates and prompt chaining using the LangChain Expression Language (LCEL), and how to use vector stores and embedding functions together with LangChain. I believe these foundational knowledge in LLM development frameworks will allow me to build more complex LLM applications in the future.
