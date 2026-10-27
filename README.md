# EXTAA ML Engineer Intern Assessment

This repository contains my implementation of **Task 1: Basic Manual RAG** and **Task 4: Laya integration** for the EXTAA ML Engineer Intern assessment. Both tasks are documented in the submitted Jupyter/Colab notebook.

## Task 1: Basic Manual RAG

### Overview

The pipeline uses the supplied IACS steel shipbuilding quality standard PDF as its knowledge source. It extracts text from PDF pages 4–8, splits the text into 44 chunks, retrieves relevant evidence, and generates answers grounded in the retrieved chunks.

### Implementation

- **Document processing:** Extracts text from the selected PDF pages and retains original PDF page numbers.
- **Keyword retrieval:** Ranks chunks using overlap between unique question and chunk terms after basic text preprocessing.
- **Embedding retrieval:** Uses `sentence-transformers/all-MiniLM-L6-v2` to create 384-dimensional embeddings and rank chunks by cosine similarity.
- **Answer generation:** Uses predefined evidence-based rules and includes PDF page citations. This is a rule-based answer generator, not a generative LLM.
- **Fallback:** Returns `The supplied pages do not specify an answer.` when the supplied evidence does not support an answer.

### Task 1 test results

The supplied test set contains three answerable questions and two questions whose answers are not specified in the selected pages.

| Check | Keyword retrieval | Embedding retrieval |
|---|---:|---:|
| Answerable retrieval hits | 3/3 | 3/3 |
| Expected unanswerable cases handled | 2/2 | 2/2 |
| Answer checks passed | 5/5 | 5/5 |

These results describe only the supplied test questions and should not be interpreted as performance on a larger dataset.

## Task 4: Laya Evidence-Sufficiency Gate

### Overview

Task 4 extends the embedding-based RAG pipeline with Laya, which decides whether the top three retrieved chunks contain enough evidence to answer a question.

### Workflow

1. Retrieve the top three chunks using the existing embedding retriever.
2. Send the question and retrieved evidence to Laya for an `answer` or `abstain` decision.
3. If Laya returns `answer`, pass the retrieved evidence to the existing rule-based answer generator.
4. If Laya returns `abstain`, use the standard fallback response.
5. Record the decision, final answer, cited PDF pages, and any errors for evaluation.

Laya acts as a decision gate; it does not replace retrieval or generate the final answer. The original retrieval and answer-generation methods remain unchanged.

### Task 4 evaluation

The ten supplied questions were evaluated across development and test splits. Laya's decisions were compared with the expected answerability labels.

| Split | Correct decisions | Decision accuracy | Errors |
|---|---:|---:|---:|
| Development (5 questions) | 5/5 | 100% | 0 |
| Test (5 questions) | 5/5 | 100% | 0 |

Across these questions, Laya answered the eight questions labelled answerable and abstained on q9 and q10, whose answers were not available in the supplied pages. The final answers matched the Task 1 baseline in this evaluation.

**Interpretation:** Laya made the expected decisions on this small, supplied set. This does not establish 100% accuracy on unseen questions or prove a general improvement in answer quality. In this run, Laya added an explicit evidence-sufficiency decision step while final answers remained unchanged.

## Repository files

- **Notebook (`.ipynb`):** Implementation, experiments, and documentation for Task 1 and Task 4.
- **`task1_baseline_outputs.json`:** Baseline embedding-retrieval outputs for development and test questions.
- **`task4_laya_outputs.json`:** Laya decisions, final answers, supporting PDF pages, and error fields for development and test questions.
- **`task4_evaluation_summary.json`:** Decision-accuracy and error summaries for both splits.

## Requirements and running the notebook

The notebook was developed in Google Colab and uses Python libraries including `sentence-transformers`, `pypdf`, and `laya`.

1. Open the notebook in Google Colab or a compatible Jupyter environment.
2. Run the cells in order and upload the assessment ZIP file when prompted.
3. Allow the notebook to extract the required PDF, questions, and Task 4 supporting files.
4. Install any missing dependencies if prompted, and ensure the Laya package and agent are accessible before running Task 4.

The JSON files are saved evaluation artifacts. Rerun the relevant notebook cells if you change the implementation and need to regenerate them.

## Limitations

- The source text is restricted to PDF pages 4–8; information elsewhere in the document is not considered.
- PDF text extraction may miss information in complex tables, diagrams, or formatting.
- Answer generation is rule-based and is not a general-purpose language model.
- Evaluation uses only ten supplied questions, so the results are not representative of broader use.
- Retrieval quality and Laya's decisions may differ on other documents, questions, or retrieved evidence.

## AI assistance and development record

ChatGPT was used for implementation guidance, debugging, and documentation. The notebook contains the implementation and evaluation steps for review.

The documented development session ran from **12:26 AM to 2:57 AM EAT** (2 hours and 31 minutes). These times were recorded retrospectively rather than by a continuously running timer.
