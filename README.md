# ASSIST03
AI-powered document intelligence system that enables semantic search, knowledge retrieval, and question answering over uploaded PDFs and documents.
# app.py

from pypdf import PdfReader

print("=" * 50)
print("📚 Personal Knowledge Assistant")
print("=" * 50)

pdf_path = input("Enter the PDF file name (e.g., notes.pdf): ")

try:
reader = PdfReader(pdf_path)

```
document_text = ""

for page in reader.pages:
    text = page.extract_text()
    if text:
        document_text += text + "\n"

print("\n✅ Document loaded successfully!")
print(f"📄 Total characters extracted: {len(document_text)}")

while True:
    question = input("\nAsk a question (or type 'exit'): ")

    if question.lower() == "exit":
        print("👋 Goodbye!")
        break

    matches = []

    paragraphs = document_text.split("\n")

    for paragraph in paragraphs:
        if question.lower() in paragraph.lower():
            matches.append(paragraph)

    if matches:
        print("\n🔍 Relevant Information Found:\n")

        for match in matches[:5]:
            print("-" * 40)
            print(match)
            print("-" * 40)
    else:
        print("\n❌ No relevant information found.")
```

except FileNotFoundError:
print("\n❌ PDF file not found.")
except Exception as e:
print("\n⚠️ Error:", e)
