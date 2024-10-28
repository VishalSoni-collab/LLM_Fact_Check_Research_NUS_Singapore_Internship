# LLM Fact-Checking System

This project implements a fact-checking system using Large Language Models (LLMs) and external data sources to verify statements for factual accuracy. It processes text inputs, breaks down complex answers into verifiable factoids, and checks each fact against context retrieved from web sources or structured knowledge bases.

## Project Overview

The system leverages the LangChain library with OpenAI and external tools (e.g., WolframAlpha, SerpAPI) to:
1. **Decompose Input Statements:** Split complex answers into independent factoids.
2. **Retrieve Relevant Context:** Use vector search and web document indexing to find supporting evidence.
3. **Verify Facts:** Apply LLMs to compare each factoid with context, labeling it as "True," "False," or "Unverifiable."

## Key Steps and Workflow

### 1. Data Loading and Input Processing
   - **Objective:** Load data from various file formats and prepare it for fact-checking.
   - **Implementation:**
     - The `read_file` function supports `.txt`, `.docx`, `.pdf`, and `.xls/.xlsx` files, reading and converting them to strings.
     - The primary text source is chosen by the user, ensuring flexible input options for the fact-checking process.

### 2. Breaking Down Answers into Factoids
   - **Objective:** Simplify complex answers into discrete, verifiable statements.
   - **Implementation:** 
     - The `breakdown_answer` function splits paragraphs into sentences, then sends each to OpenAI’s API, generating simpler statements.
     - The result is a list of standalone factoids, preserving original meaning without corrections or interpretations.
   - **Concept Insight:** By breaking down answers into individual facts, the system enables granular verification, improving accuracy in the fact-checking process.

### 3. Retrieving Relevant Context from the Web and Knowledge Bases
   - **Objective:** Gather supporting information for each factoid from web resources or vector-based databases.
   - **Implementation:**
     - The system uses LangChain’s `Qdrant` and `SimpleWebPageReader` modules to retrieve and process web data.
     - For each factoid, it performs a maximum-marginal-relevance search in Qdrant to find relevant passages.
     - These passages are used as context to compare against each fact.
   - **Key Libraries:**
     - `LangChain`, `Qdrant`, and `llama_index` are used for passage retrieval and indexing, while `tqdm` adds a progress bar for fact verification.

### 4. Verifying Facts with LLMs
   - **Objective:** Compare each factoid against retrieved passages to assess truthfulness.
   - **Steps:**
     - For each factoid and its supporting passage, the system generates a prompt for OpenAI's API, asking if the statement is "True" or "False" based on the context.
     - Results are logged as "True," "False," or "Unverifiable," and the verification score is calculated.
   - **Verification Score Calculation:**
     - Uses a helper function, `calculate_score`, to determine the accuracy rate based on verified facts.

### 5. Interactive Main Program
   - **Objective:** Allow user input for custom questions, answers, and web URLs.
   - **Implementation:** 
     - The `main` function collects user inputs, breaks down the answer, verifies each factoid, and outputs a summary of fact statuses.
   - **User Flow:** The system runs interactively, prompting for input and displaying verification results for each fact in real-time.

## Code Summary

1. **Data Loading and Preprocessing:**
   - Loads files with supported formats, ensuring data is ready for processing.
   
2. **Factoid Breakdown:**
   - Decomposes complex text into factoids to enhance verification accuracy.

3. **Context Retrieval:**
   - Retrieves passages from web sources or Qdrant for context, helping verify each factoid.

4. **Fact Verification:**
   - Uses OpenAI to assess each factoid's truthfulness against context and logs results.

5. **Interactive User Input:**
   - Provides an interactive interface for users to enter custom questions, answers, and URLs for fact-checking.

## API Keys
Set up environment variables or directly assign values to the API keys for:
- `OPENAI_API_KEY`
- `WOLFRAM_ALPHA_APPID`
- `SERPAPI_API_KEY`


### Running the Notebook
Run each cell sequentially to load, process, and verify input statements, or execute `main()` to use the interactive mode for custom fact-checking.

### Example Output

The output includes a verification status for each factoid, along with supporting evidence from the retrieved passages. Here’s an example table:

| Factoid                       | Relevant Context                                | Verification Result |
|-------------------------------|-------------------------------------------------|---------------------|
| Fact 1: The earth is round.   | NASA confirms the earth is spherical.           | True               |
| Fact 2: The sun orbits earth  | The sun is the center of our solar system.      | False              |
| Fact 3: Uncertain claim       | Context unavailable                             | Unverifiable       |

In this table:
- **Factoids** are discrete statements extracted from complex answers.
- **Relevant Context** provides supporting information from the web or Qdrant.
- **Verification Result** indicates whether the factoid aligns with the retrieved context, helping users gauge factual accuracy.

### Sample Outline

#### User Passage
The system processes user-provided text, decomposing it into smaller verifiable chunks.

#### Breaking Down in Chunks
Each passage is split into standalone factoids to assess truthfulness.

- **Chunk 1:**
  - **Content:** Factoid derived from the user passage.
  - **Result:** True/False based on relevant context.

- **Chunk 2:**
  - **Content:** Next factoid from the user passage.
  - **Result:** True/False based on context.

This process continues until all chunks from the passage have been verified. The system provides a robust fact-checking process, with each result based on contextual evidence.

## Usage Instructions

### Installation
Ensure required libraries are installed:
```bash
pip install langchain openai pandas pdfplumber docx2txt PyPDF2 tqdm
