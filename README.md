# Challenge 1B: Persona-Centric Section Extractor from PDFs

## 🧠 Objective
Build a fully offline, Dockerized solution that extracts the most relevant sections from a set of PDFs, personalized to a given persona and task. The solution uses both traditional ML and Sentence Transformers for semantic understanding.

---

##  Directory Structure

```
Challenge_1b/
├── Collection_1/                  # Contains challenge1b_input.json, PDFs, and stores output
├── Collection_2/
├── Collection_3/
├── MyModel.joblib                 # Trained classifier model
├── MyEncoder.joblib              # Label encoder for headings
├── all-MiniLM-L6-v2/             # SentenceTransformer model folder (saved locally)
├── process_collection.py         # Main processing script
├── requirements.txt
├── Dockerfile
└── README.md
```

---

##  How the Pipeline Works

1. Load joblib ML model and SentenceTransformer (offline)
2. For each document in a collection:
   - Extract visual features from lines
   - Predict headings (`H1`, `H2`, etc.)
   - Merge headings & paragraphs per page
3. Compute semantic similarity to the given task
4. Rank relevant sections and extract supporting text

---

##  Docker Instructions

###  Build Image

```bash
docker build --platform=linux/amd64 -t challenge1b:latest .
```

### ▶️ Run for Collection_1

```bash
docker run --rm -v ${PWD}/Collection_1:/app/Collection_1 challenge1b:latest python process_collection.py Collection_1
```

### ▶️ Run for Collection_2

```bash
docker run --rm -v ${PWD}/Collection_2:/app/Collection_2 challenge1b:latest python process_collection.py Collection_2
```

### ▶️ Run for Collection_3

```bash
docker run --rm -v ${PWD}/Collection_3:/app/Collection_3 challenge1b:latest python process_collection.py Collection_3
```

> 🔒 Each command runs fully offline and processes only the specified collection.

---

## ⚙️ Requirements

- Model: `MyModel.joblib` (scikit-learn classifier)
- Encoder: `MyEncoder.joblib`
- SBERT model: `all-MiniLM-L6-v2/` (pre-downloaded using `.save()`)

Install locally if needed:
```bash
pip install -r requirements.txt
```

---

##  Collection Format

Each collection folder (e.g., `Collection_1`, `Collection_2`, etc.) should follow this structure:

Collection_X/
├── pdfs/ # Folder containing input PDFs
├── challenge1b_input.json # Task + persona + document list
└── challenge1b_output.json # Output will be saved here


> 📝 **Note**: For each collection, the following are expected: `pdfs`, `challenge1b_input.json`, and `challenge1b_output.json`.  
> You can create your own collection folders by adding new PDFs and crafting your own `challenge1b_input.json` for custom testing.




##  Output Format

Each collection will generate a file named `challenge1b_output.json` with:
```json
{
  "metadata": {
    "input_documents": [...],
    "persona": "...",
    "job_to_be_done": "...",
    "processing_timestamp": "..."
  },
  "extracted_sections": [
    {
      "document": "file1.pdf",
      "section_title": "Section Title",
      "importance_rank": 1,
      "page_number": 3
    }
  ],
  "subsection_analysis": [
    {
      "document": "file1.pdf",
      "refined_text": "relevant paragraphs",
      "page_number": 3
    }
  ]
}
```

---

##  Adobe Submission Compliance

| Constraint                            | Met? |
|--------------------------------------|------|
| Offline-only execution               | ✅   |
| CPU-only usage                       | ✅   |
| No internet during inference         | ✅   |
| Fully containerized with Docker      | ✅   |
| Collection-wise processing allowed   | ✅   |
| Inference time < 60s for 3–5 PDFs    | ✅   |

---

## 👨‍💻 Author
**Omkar Bhongale**  
Submission for **Adobe India Hackathon 2025 — Challenge 1B**
