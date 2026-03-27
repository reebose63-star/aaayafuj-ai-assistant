# aaayafuj-ai-assistant

**Ethical AI assistants and local ALLM integrations for offline, controllable intelligence.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![Code Quality](https://img.shields.io/badge/Code%20Quality-Active-brightgreen.svg)](https://github.com/aaayafuj/ai-assistant)
[![Ethical AI](https://img.shields.io/badge/Ethical-AI-orange.svg)](#ethics--safety)

---

## Overview

**aaayafuj-ai-assistant** is a framework for building and deploying local language models with a focus on:

- **Privacy-first design** — All processing happens locally
- **Transparent behavior** — Understand how your AI makes decisions
- **Controllable outputs** — Full governance over model responses
- **Educational foundation** — Learn how LLMs work internally
- **Ethical constraints** — Built-in safeguards against misuse

This project is ideal for developers, researchers, and organizations that need intelligent automation without cloud dependency or privacy concerns.

---

## Features

### Core Capabilities

✅ **Local LLM Integration**
- Support for Ollama, LiteLLM, and custom model loaders
- Offline inference with no external API calls
- GPU acceleration support (CUDA, Metal, ROCm)

✅ **Prompt Engineering**
- Advanced templating system for consistent outputs
- Few-shot learning examples and in-context adaptation
- Safety validation before model execution

✅ **Response Validation**
- Output filtering and sanitization
- Confidence scoring and uncertainty detection
- Structured output parsing (JSON, YAML, markdown)

✅ **Knowledge Integration**
- RAG (Retrieval-Augmented Generation) support
- Document ingestion and semantic search
- Vector database integration (Chroma, Pinecone, FAISS)

✅ **Audit & Monitoring**
- Complete request/response logging
- Cost tracking (if using external models)
- Performance metrics and latency analysis

---

## Quick Start

### Prerequisites

- Python 3.9+
- 8GB RAM minimum (16GB+ recommended for larger models)
- Optional: NVIDIA GPU with CUDA 11.8+ or Apple Silicon

### Installation

```bash
git clone https://github.com/aaayafuj/ai-assistant.git
cd ai-assistant

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# For GPU support (CUDA)
pip install -r requirements-cuda.txt

# For M1/M2 Mac support
pip install -r requirements-metal.txt
```

### Basic Usage

```python
from aaayafuj_ai import LocalAssistant, PromptTemplate

# Initialize with a local model
assistant = LocalAssistant(
    model="mistral:7b",  # Via Ollama
    temperature=0.7,
    max_tokens=512
)

# Create a prompt template
template = PromptTemplate(
    name="code_review",
    template="""Review this Python code for security issues:

{code}

Provide specific recommendations."""
)

# Run inference
response = assistant.generate(
    template=template,
    code="user_input = input('Enter password: ')"
)

print(response.text)
print(f"Confidence: {response.confidence}")
```

### Running with Docker

```bash
docker build -t aaayafuj-ai .
docker run -it --gpus all aaayafuj-ai python main.py
```

---

## Architecture

```
aaayafuj-ai-assistant/
├── src/
│   ├── core/
│   │   ├── models.py         # Model loaders and interfaces
│   │   ├── prompts.py        # Templating and prompt engineering
│   │   └── validators.py     # Output validation layer
│   ├── integrations/
│   │   ├── ollama.py         # Ollama integration
│   │   ├── litellm.py        # LiteLLM connector
│   │   └── rag.py            # RAG pipeline
│   ├── utils/
│   │   ├── logging.py        # Audit logging
│   │   ├── metrics.py        # Performance tracking
│   │   └── config.py         # Configuration management
│   └── cli/
│       └── main.py           # Command-line interface
├── tests/
│   ├── test_models.py
│   ├── test_prompts.py
│   └── test_validators.py
├── examples/
│   ├── basic_inference.py
│   ├── rag_example.py
│   └── structured_output.py
├── requirements.txt
├── Dockerfile
└── README.md
```

---

## Configuration

Create a `.env` file in the project root:

```env
# Model Configuration
MODEL_NAME=mistral:7b
MODEL_TEMPERATURE=0.7
MODEL_MAX_TOKENS=512

# Logging
LOG_LEVEL=INFO
AUDIT_LOG_PATH=./logs/audit.log

# GPU Support
USE_GPU=true
GPU_MEMORY_FRACTION=0.8

# RAG Configuration
ENABLE_RAG=false
VECTOR_DB=chroma
EMBEDDINGS_MODEL=sentence-transformers/all-MiniLM-L6-v2
```

---

## Examples

### Example 1: Code Security Analysis

```python
from aaayafuj_ai import LocalAssistant

assistant = LocalAssistant(model="neural-chat:7b")

code_sample = '''
import sqlite3
query = f"SELECT * FROM users WHERE id = {user_input}"
conn.execute(query)
'''

result = assistant.analyze_security(code_sample)
print(result.vulnerabilities)
```

### Example 2: Document Q&A with RAG

```python
from aaayafuj_ai import RAGAssistant

rag = RAGAssistant(
    model="mistral:7b",
    vector_db="chroma"
)

# Ingest documents
rag.ingest_documents("./docs/")

# Ask questions
answer = rag.query("What is the API authentication method?")
print(answer)
```

### Example 3: Batch Processing

```python
from aaayafuj_ai import BatchProcessor

processor = BatchProcessor(model="neural-chat:7b")

results = processor.process_file(
    input_file="prompts.jsonl",
    output_file="results.jsonl",
    num_workers=4
)

print(f"Processed {results.count} items in {results.duration}s")
```

---

## API Reference

### LocalAssistant

```python
assistant = LocalAssistant(
    model: str,              # Model name or path
    temperature: float = 0.7,  # Sampling temperature
    max_tokens: int = 512,   # Max output length
    top_p: float = 0.9,      # Nucleus sampling
    enable_logging: bool = True,
    audit_path: str = None
)

# Methods
response = assistant.generate(prompt, **kwargs)
response = assistant.stream(prompt, **kwargs)
assistant.set_system_prompt(prompt)
metrics = assistant.get_metrics()
```

### PromptTemplate

```python
template = PromptTemplate(
    name: str,
    template: str,
    variables: List[str] = None,
    examples: List[Dict] = None,
    validators: List[Callable] = None
)

# Methods
rendered = template.render(var1=value1, var2=value2)
template.validate(inputs)
```

### Response

```python
response.text          # Generated text
response.tokens_used   # Token count
response.latency_ms    # Inference time
response.confidence    # Confidence score (0-1)
response.metadata      # Additional data
```

---

## Performance Benchmarks

| Model | Parameters | Latency (ms) | Memory (GB) | Quality |
|-------|-----------|--------------|------------|---------|
| Mistral | 7B | 150-250 | 6-8 | ⭐⭐⭐⭐ |
| Neural Chat | 7B | 200-300 | 6-8 | ⭐⭐⭐⭐ |
| Llama 2 | 7B | 180-280 | 6-8 | ⭐⭐⭐⭐⭐ |
| Orca | 3B | 80-120 | 3-4 | ⭐⭐⭐ |

*Benchmarks: A100 GPU, batch size 1, standard prompts*

---

## Security & Privacy

🔒 **Privacy First**
- All inference runs locally
- No data transmitted to external services
- No model training on user data
- Complete audit trail of requests

🛡️ **Safety Mechanisms**
- Input validation and sanitization
- Output filtering for harmful content
- Rate limiting and resource constraints
- Explicit authorization for sensitive operations

⚠️ **Important Notes**
- Models are not infallible — validate critical outputs
- Local models may hallucinate; use RAG for factual accuracy
- GPU memory leaks possible with extended sessions
- Regularly update models and dependencies

---

## Ethics & Safety

This project is built on the principle: **"Power is not dominance. Power is discipline."**

✅ **What This Project Does:**
- Enable transparent, controllable AI
- Provide educational tools for learning
- Respect user privacy and autonomy
- Encourage responsible experimentation

❌ **What This Project Does NOT Do:**
- Train on private data without consent
- Enable surveillance or unauthorized analysis
- Facilitate malicious automation
- Replace human judgment in critical decisions

**All use must be legal, ethical, and within authorized scope.**

---

## Contributing

We welcome contributions! Please follow these guidelines:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/your-feature`
3. **Commit** with clear messages: `git commit -m "Add [feature]"`
4. **Test** your changes: `pytest tests/`
5. **Submit** a pull request with description

### Code Standards

- Python 3.9+ compatible
- PEP 8 style guide
- Type hints required
- Docstrings for all functions
- Test coverage > 80%

---

## Troubleshooting

**Issue: Model fails to download**
```bash
# Manually pull with Ollama
ollama pull mistral:7b
```

**Issue: Out of memory errors**
- Reduce `max_tokens`
- Use smaller model variants
- Enable GPU offloading

**Issue: Slow inference**
- Verify GPU is being used: `nvidia-smi`
- Reduce batch size
- Check system load and competing processes

---

## Roadmap

- [ ] Multi-model ensemble support
- [ ] Advanced RAG with reranking
- [ ] Web API interface
- [ ] Prompt optimization toolkit
- [ ] Model quantization tools
- [ ] Integration with vector databases (Pinecone, Weaviate)
- [ ] Web UI dashboard

---

## License

MIT License — See [LICENSE](LICENSE) file for details

**Free to use, modify, and distribute. Attribution appreciated.**

---

## Citation

If you use this project in research, please cite:

```bibtex
@software{aaayafuj_ai_assistant,
  author = {Yafet Yohannes},
  title = {aaayafuj-ai-assistant: Ethical Local LLM Framework},
  year = {2024},
  url = {https://github.com/aaayafuj/ai-assistant}
}
```

---

## Contact & Support

- **Issues**: [GitHub Issues](https://github.com/aaayafuj/ai-assistant/issues)
- **Discussions**: [GitHub Discussions](https://github.com/aaayafuj/ai-assistant/discussions)
- **Email**: hello@aaayafuj.dev
- **Documentation**: [Full Docs](./docs/README.md)

---

**Built with discipline. Power is not dominance.**

*The ordinary soldier will reign.* — **1.1.1.25.1.6.21.10** 👑
