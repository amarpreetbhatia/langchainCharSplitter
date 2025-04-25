# langchainCharSplitter
simple example to run the text splitter from Document using langchain

## Steps

1. **Split Text with LangChain**  
    Use LangChain's text splitter to divide the content of `movies.txt` into smaller, manageable chunks.

2. **Generate Embeddings with OpenAI**  
    For each text chunk, use OpenAI's Embedding model to generate a vector representation (embedding).

3. **Store in Supabase**  
    Insert each text chunk along with its embedding into a table in your Supabase database for efficient storage and retrieval.

## Requirements

- Python 3.8+
- [LangChain](https://python.langchain.com/)
- [OpenAI Python SDK](https://github.com/openai/openai-python)
- [Supabase Python Client](https://github.com/supabase-community/supabase-py)

## Example Usage

```python
from langchain.text_splitter import CharacterTextSplitter
from openai import OpenAI
from supabase import create_client

# 1. Read and split text
with open("movies.txt") as f:
     text = f.read()
splitter = CharacterTextSplitter(chunk_size=500, chunk_overlap=50)
chunks = splitter.split_text(text)

# 2. Generate embeddings
openai_client = OpenAI(api_key="YOUR_OPENAI_API_KEY")
embeddings = [openai_client.embeddings.create(input=chunk, model="text-embedding-ada-002").data[0].embedding for chunk in chunks]

# 3. Insert into Supabase
supabase = create_client("SUPABASE_URL", "SUPABASE_KEY")
for chunk, embedding in zip(chunks, embeddings):
     supabase.table("your_table").insert({"text": chunk, "embedding": embedding}).execute()
```

## Notes

- Ensure your Supabase table has columns for `text` and `embedding`.
- Set your API keys and database credentials securely.
- Adjust `chunk_size` and `chunk_overlap` as needed for your use case.