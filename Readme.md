# Retrieval-Augmented Generation (RAG) Notebook

## Overview

This notebook demonstrates a simple Retrieval-Augmented Generation (RAG) system. It combines a Large Language Model (LLM) with an external knowledge base to provide more informed and contextually relevant answers to user queries. The process involves:

1.  **Initial LLM Response**: Getting a baseline response from an LLM without external context.
2.  **External Knowledge Base**: Loading a text document as an external knowledge source.
3.  **Vector Store Creation**: Splitting the document into chunks, embedding them using a Sentence Transformer, and indexing them with FAISS.
4.  **RAG Query**: Embedding the user's query, retrieving the most relevant chunks from the FAISS index, and augmenting the LLM's prompt with this retrieved context.
5.  **Context-Augmented Response**: Generating a final response from the LLM using the augmented prompt.

## Setup

### Dependencies

The following Python libraries are required:

- `sentence-transformers`
- `faiss-cpu`
- `langchain_community`
- `replicate`

Install them using pip:

```bash
pip install sentence-transformers faiss-cpu langchain_community replicate
```

````

### API Key

This notebook uses the Replicate API for the Large Language Model. You will need a Replicate API token.

1.  Obtain a Replicate API token from [replicate.com](https://replicate.com/signin).
2.  Store your API token securely in Colab's secrets manager under the name `REPLICATE_API_TOKEN`.

### External Knowledge Base

An external text file (`External_KB_for_RAG.txt`) is used as the knowledge base. Ensure this file is available in your Colab environment or upload it when prompted.

## How to Run

1.  **Install Dependencies**: Run the first code cell to install all required libraries.
2.  **Import Modules and Configure API**: Run the subsequent cells to import necessary modules and set up your Replicate API token.
3.  **Initial Query**: The notebook will prompt you to "Ask a question:". Enter your query to see the LLM's response without RAG.
4.  **Upload Knowledge Base**: Upload your `External_KB_for_RAG.txt` file when prompted.
5.  **Process Knowledge Base**: The notebook will then process the uploaded document by splitting it into chunks and creating a FAISS index.
6.  **RAG Execution**: The RAG process will automatically execute, embedding your query, retrieving context, and generating a RAG-augmented response.
7.  **Compare Results**: The final cell displays both the initial LLM response and the RAG-augmented response for comparison.

## Example Usage

**User Query**: "Hi. I’m a citizen of Romania. Do I need a Schengen visa if I travel to France?"

### Without RAG

(Response based solely on the LLM's pre-trained knowledge)

_Example Output_:

```text
As a Romanian citizen, you are part of the Schengen Area, which means you have the right to travel freely within the Schengen states without a visa for stays of up to 90 days within any 180-day period. France, being a member of the Schengen Area, falls under this category.
...
```

### With RAG (Context-Augmented)

(Response augmented with information from `External_KB_for_RAG.txt`)

_Example Output_:

```text
If you are traveling to France before January 1, 2025, you will need to apply for a Schengen visa. However, starting from January 1, 2025, Romanian citizens will need to apply for an ETIAS permit before traveling, as Romania is set to join the Schengen Area on this date. The ETIAS permit is required for short-term stays (up to 90 days in a 180-day period) and is valid for up to three years or until the traveler’s passport expires. It costs €7 for applications aged 18 to 70.
```

## Model Used

- **LLM**: `ibm-granite/granite-3.2-8b-instruct` from Replicate.
- **Embedding Model**: `all-MiniLM-L6-v2` from Sentence Transformers.


````
