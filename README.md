# ASSIST03
AI-powered document intelligence system that enables semantic search, knowledge retrieval, and question answering over uploaded PDFs and documents.

from pypdf import PdfReader
from sentence_transformers import SentenceTransformer
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

print("="*60)
print("PERSONAL KNOWLEDGE ASSISTANT")
print("="*60)

pdf_file = input("Enter PDF filename: ")

reader = PdfReader(pdf_file)

document_text = ""

for page in reader.pages:
    text = page.extract_text()

    if text:
        document_text += text + "\n"

print("\nPDF Loaded Successfully!")

# Split into chunks

chunks = []

chunk_size = 500

for i in range(0, len(document_text), chunk_size):
    chunks.append(document_text[i:i+chunk_size])

print(f"\nCreated {len(chunks)} knowledge chunks")

# Load embedding model

print("\nLoading AI model...")

model = SentenceTransformer('all-MiniLM-L6-v2')

chunk_embeddings = model.encode(chunks)

print("Knowledge base ready!")

while True:

    query = input("\nAsk a question (type exit to quit): ")

    if query.lower() == "exit":
        break

    query_embedding = model.encode([query])

    similarities = cosine_similarity(
        query_embedding,
        chunk_embeddings
    )[0]

    best_match_index = np.argmax(similarities)

    print("\nMost Relevant Information:\n")
    print(chunks[best_match_index])

    print("\nSimilarity Score:",
          round(similarities[best_match_index], 3))
