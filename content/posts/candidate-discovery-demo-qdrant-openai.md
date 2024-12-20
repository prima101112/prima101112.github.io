---
title: "Candidate Discovery Demo Qdrant Openai"
date: 2024-12-20T22:17:10+07:00
draft: true
---
## Candidate discovery demo

In this post will be litle bit technical. continuing what i already created that talk with pdf (RAG)
i will little bit re architec the app design. before we usi FAISS to store our fectore. i am afraid it cannot scale. so in search of vector databases. i encounter with bunc of option there is pgai (that really take my interest) still in beta, but also qdrant that already have their enterpise option.

### lets use this stack then :

- qdrant vectore database , why? is the most mature afaik and backed by a company
- using flask/fast api to serve
- maybe combine with postgress to store the actual data

### so the goal here is

- could upload a resume of candidate -> embed
- could search -> (using RAG) best candidate with chat.
- return with human language

### graph and architecture

{{% mermaid %}}
graph TD
    A[User Input via Chat] -->|Submit Query| B[Next.js Frontend]
    B -->|API Call| C(FastAPI Backend)
    C -->|Vectorize Query| D[OpenAI Embedding API]
    D -->|Vector Embedding| E[Qdrant]
    E -->|Find Similar Candidates| F[Candidate Matches]
    F -->|Return Results| C(FastAPI Backend)
    C -->|Send Response| B
    B -->|Display Matches| A
{{% /mermaid %}}

### define step

- installing qdrant
- code the fast api add some logic
  - POST pdf/text => embeding => qdrant => success/error
  - POST {query_content:} => reult the match candidate
  - POST for plain text candidate data (testing purpose)


this is part 1 will continou tomorrow
