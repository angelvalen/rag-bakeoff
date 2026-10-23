# Intro RAG Bake-Off: Technical Q&A

This project explores different Retrieval-Augmented Generation (RAG) pipelines for
multiple-choice Q&A, based on Meta's research paper introducing REFRAG. Using a curated
dataset of 70 multiple-choice questions, we compared Okapi BM25, dense, and hybrid
retrieval methods over both paragraph-based and recursively built chunks, and we assessed
their improvements over the baseline LLM (Google's Gemini Flash 2.5 Lite).

## 📋 Report

The accompanying [**report**](report.pdf) provides a comprehensive analysis of our RAG
benchmarking study, covering:

- **Theoretical Foundations**: Detailed explanations of RAG concepts, retrieval methods
  (BM25, dense, hybrid), and chunking strategies.
- **Methodology**: Complete experimental setup, dataset construction, and evaluation
  metrics.
- **Implementation Details**: Technical specifications of our pipeline and methods
  implementation.
- **Results Analysis**: Comparative performance across all tested configurations with
  statistical validation (5 runs per configuration).
- **Discussion, Insights & Future Work**: Practical recommendations for RAG system
  design based on our findings and suggested research directions.

The report adopts a hybrid approach, combining theoretical background with empirical
analysis, to serve both newcomers to RAG and experienced practitioners looking for
actionable insights.

## 🚀 Quick Start

**1. Installation**:

```
git clone https://github.com/angelvalen/rag-bakeoff.git
cd rag-bakeoff
pip install -r requirements.txt
```

**2. Required API Key**:
This project uses Google's Gemini API. You must obtain an API key and set it as an
environment variable:

```
# On Linux/Mac:
export GOOGLE_API_KEY="your-api-key-here"

# On Windows (Command Prompt):
set GOOGLE_API_KEY=your-api-key-here

# On Windows (PowerShell):
$env:GOOGLE_API_KEY="your-api-key-here"
```

**3. Usage**:
```
python main.py <command> [options]

Commands:

all          Run the full pipeline (chunking, experiments and figures)
chunks       Generate chunks
experiments  Run experiments and generate metrics
figures      Generate plots
publish      Promote the current results into the final directory

Options:

--small     Use a smaller dataset (for testing)
```

Running the program will overwrite the `/current` directory with the obtained results.
Running `python main.py publish` will save the current results into `/final` to avoid
losing them. Be careful when publishing, since you will overwrite the final directory.
Separate commands like `python main.py experiments` or `python main.py figures` will load
the chunks / metrics saved in the `/current` directory.

## 🎯 Key Findings

- **All RAG variants beat the baseline LLM** (answer accuracy 66 % → 84–92 %), confirming
  the value of retrieving external context.
- **BM25 + paragraph-based chunking** was the best configuration, reaching **94 % answer
  accuracy** with low latency. BM25 exploits the technical keywords and acronyms
  (REFRAG, CEPE, TTFT, …) that recur in the paper, while paragraph chunks are smaller and
  non-overlapping, which suits sparse retrieval.
- **Recursive chunking improved source attribution** over paragraph chunking, likely
  because the 200-character overlap preserves information that would otherwise be split
  across boundaries.
- **Hybrid retrieval** (BM25 + dense with Reciprocal Rank Fusion) was the most robust
  across chunking strategies, though its score was slightly dragged down by the weaker
  dense retriever on this keyword-rich task.
- **Latency** increases in the expected order: baseline < BM25 < dense < hybrid.

## 🎓 Acknowledgments

This project was part of the *Concurso de Modelización de Problemas de Empresa*, an event
organised in the [*Facultad de Ciencias Matemáticas, UCM*](https://matematicas.ucm.es/).
The [problem proposal](./problem_proposal.pdf) was given by the enterprise
[*Management Solutions*](https://www.managementsolutions.com/).

**Author**: *Ángel Valencia Padilla*

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for
details.