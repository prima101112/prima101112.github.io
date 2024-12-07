---
title: "The Important of Technical Discussion"
date: 2024-12-06T09:44:36+07:00
draft: false
categories: ['technology', 'blog','learning']
tags: ['technology', 'social interaction', 'learn']
languageCode: 'en-us'
---

Recently i am creating a blog that code and create RAG pipeline.
Since Langchain (the library that I use) wraps all the functions behind the curtain, some users, like myself, may misunderstand its workings. I want to discuss this part of the code:

```python
def build_rag(vectorstore):
    retriever = vectorstore.as_retriever()
    llm = OllamaLLM(model="mistral")  # Specify Mistral model
    qa_chain = RetrievalQA.from_chain_type(llm, retriever=retriever)
    return qa_chain
```

In understanding this code I *assume* that when `qa_chain = RetrievalQA.from_chain_type(llm, retriever=retriever)` retreiver is vectorstore right?.
i *assume again* that llm is actually read the vector.

--- my day continue ---

And recently i have a discussion with one of my friend about this RAG stuff and he is actually taking a Master Degree in Artificial Intelegence area. this blog post is focus on

## how important a discussion is.

i have certain understanding of how this is works and he is correcting me. this is will not happend with AI that particular code will just explain to you how its work but not know what actually happend inside it.

{{% mermaid %}}
graph TD
    A[User Input: PDF Document] -->|Preprocess PDF| B[Extracted Text]
    B -->|Embed Text| C[Document Embeddings]
    C -->|Store Embeddings| D[FAISS Vector Database]
    E[User Query] -->|Input Query| F[Vector Database Search]
    D -->|Retrieve Matching Vectors| F
    subgraph ide1 [rag]
    F -->|Find Matching Vectors| G[Relevant Document Embeddings]
    G -->|Convert Vectors to Text| H((("`**Retrieved Text Documents**`")))
    H -->|Provide Context| I[Language Model LLM]
    end
    I -->|Generate Response| J[Final Answer]
{{% /mermaid %}}

And this time i got my lecture. fix my brain that llm is just accept human language thats why its called llm so vectorstore is basically search and when its match and get in to the llm, its form to text again then -> llms.

even in this code

`qa_chain = RetrievalQA.from_chain_type(llm, retriever=retriever)`

seems like llm is directly get from retriver but actually **after it found its translate to text first than input to the llm's**

thanks to my colleague