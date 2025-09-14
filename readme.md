# AI Pizza Restaurant Q&A Agent

This project is an interactive Python application that uses LangChain and Ollama to answer questions about a pizza restaurant using real customer reviews. Below is a step-by-step explanation of the code so anyone can understand how it works.

---

## Prerequisites

- **Ollama** installed and running locally ([Ollama website](https://ollama.com/))
- The model `llama3.2:1b` pulled via `ollama pull llama3.2:1b`
- The model `mxbai-embed-large` pulled via `ollama pull mxbai-embed-large`
- Python packages installed: `langchain_ollama`, `langchain_chroma`, `pandas`
- A CSV file named `realistic_restaurant_reviews.csv` in the project directory

---

## File: `vector.py`

This file loads restaurant reviews, embeds them, and creates a retriever for relevant review snippets.

### Step-by-step:

1. **Imports**  
   - `OllamaEmbeddings`, `Chroma`, `Document`, `os`, `pandas` for embeddings, vector storage, and data handling.

2. **Load Reviews**  
   - Reads the CSV file containing reviews.

3. **Set Up Embeddings**  
   - Uses Ollama's `mxbai-embed-large` model to create embeddings for each review.

4. **Database Location**  
   - Checks if a local vector database exists; if not, prepares to add documents.

5. **Prepare Documents**  
   - For each review, creates a `Document` object with content and metadata.

6. **Create Vector Store**  
   - Initializes a Chroma vector store with the embeddings.

7. **Add Documents**  
   - If the database is new, adds all documents to the vector store.

8. **Create Retriever**  
   - Sets up a retriever to fetch the top 5 relevant reviews for a given query.

---

## File: `main.py`

This file is the interactive Q&A agent.

### Step-by-step:

1. **Imports**  
   - `OllamaLLM` for the language model, `ChatPromptTemplate` for prompt formatting, and `retriever` from `vector.py`.

2. **Initialize Model**  
   - Loads the `llama3.2:1b` model via Ollama.

3. **Prompt Template**  
   - Defines a prompt instructing the model to answer questions about a pizza restaurant using provided reviews.

4. **Create Prompt and Chain**  
   - Uses LangChain's template system to format the prompt and chains it with the model.

5. **Interactive Loop**  
   - Starts a loop for user interaction.
   - Prints a separator for clarity.
   - Asks the user for a question.
   - If the user enters "q", the loop breaks and the program exits.

6. **Retrieve Relevant Reviews**  
   - Uses the retriever to fetch reviews most relevant to the user's question.

7. **Invoke Model**  
   - Passes the reviews and question to the model via the prompt chain.
   - Prints the model's answer.

---

## How It Works

- When you run `main.py`, you can ask any question about the pizza restaurant.
- The program finds the most relevant reviews using semantic search.
- It then asks the LLM to answer your question, using those reviews as context.
- The answer is printed for you.

---

## Example Usage

```
Ask your question (q to quit): What do people think about the crust?
```

The agent will find reviews mentioning the crust and answer your question based on them.

---

## Notes

- Make sure Ollama is running and the required models are pulled.
- The CSV file should have columns: `Title`, `Review`, `Rating`, `Date`.
- All dependencies should be installed in your Python environment.

---

## Troubleshooting

- **Model not found:** Run `ollama pull llama3.2:1b` and `ollama pull mxbai-embed-large`.
- **Missing packages:** Install with `pip install