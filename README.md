# EXTAA ML Engineer Intern Assessment

## Task 1: Basic Manual RAG

### Overview
This project implements a basic Retrieval-Augmented Generation (RAG) pipeline using the provided IACS steel shipbuilding quality standard PDF. It extracts document text, divides it into chunks, retrieves relevant evidence using keyword-based and embedding-based search, and produces answers grounded in the retrieved content.

### Implementation
- **Document processing:** Extracts text from the supplied PDF, using pages 4–8.
- **Chunking:** Splits the extracted text into 44 chunks.
- **Keyword retrieval:** Ranks chunks using keyword matching.
- **Embedding retrieval:** Uses `sentence-transformers/all-MiniLM-L6-v2` to create 384-dimensional embeddings and cosine-similarity-based ranking.
- **Answer generation:** Produces answers from predefined evidence-based rules and includes the source PDF page number. It is not a generative LLM.
- **Abstention:** Returns `The supplied pages do not specify an answer.` when the supplied evidence does not support an answer.

### Evaluation
The notebook evaluates both keyword and embedding retrieval against five test questions: three answerable questions and two questions whose answers are not specified in the supplied pages.

| Evaluation | Keyword retrieval | Embedding retrieval |
|---|---:|---:|
| Answerable retrieval hits | 3/3 | 3/3 |
| Expected unanswerable cases identified | 2/2 | 2/2 |
| Answer checks passed | 5/5 | 5/5 |

These results are from the included test questions and are not a measure of performance on a larger dataset.

### Requirements
- Python
- `sentence-transformers`
- The PDF-reading library used in the notebook
- The supplied IACS PDF document

Install the embedding dependency in your Python environment:

```bash
pip install sentence-transformers
```

Install any additional PDF-reading dependencies the notebook requires.

### How to Run
1. Open the notebook in Google Colab or a compatible Jupyter environment.
2. Ensure the supplied PDF is available at the file path expected by the notebook.
3. Install the required dependencies.
4. Run the notebook cells in order.
5. Run the keyword and embedding retrieval evaluations and inspect the answer checks.

### Limitations
- The evaluation uses a small set of five test questions.
- Only pages 4–8 of the supplied PDF are used.
- Text extraction may not capture all information contained in diagrams or tables.
- Answer generation uses predefined rules rather than a generative language model.
- Retrieval and answer quality have not been evaluated on a large independent dataset.

### AI and Other Assistance
AI assistance was used during development and debugging. The implementation and evaluation results should be reviewed alongside the notebook.
