# RAG-Anything Project Structure

This document provides a comprehensive overview of the RAG-Anything repository structure, explaining the purpose and organization of each directory and key file.

## Repository Overview

RAG-Anything is an All-in-One Multimodal Document Processing RAG (Retrieval-Augmented Generation) system built on [LightRAG](https://github.com/HKUDS/LightRAG). It processes diverse content including text, images, tables, equations, and other multimodal elements within a unified framework.

## Directory Structure

```
RAG-Anything/
├── .github/                    # GitHub-specific configurations
│   ├── ISSUE_TEMPLATE/        # Issue templates for bug reports and features
│   ├── workflows/             # GitHub Actions CI/CD workflows
│   ├── dependabot.yml         # Dependency update automation
│   └── pull_request_template.md
├── assets/                     # Static assets (images, logos, diagrams)
├── docs/                       # Documentation files
├── examples/                   # Example scripts and use cases
├── raganything/               # Main Python package (core library)
├── scripts/                   # Utility scripts
├── .gitignore                 # Git ignore rules
├── .pre-commit-config.yaml    # Pre-commit hooks configuration
├── env.example                # Environment variables template
├── LICENSE                    # MIT License
├── MANIFEST.in               # Package data files specification
├── PROJECT_STRUCTURE.md      # This file - project structure documentation
├── pyproject.toml            # Modern Python project configuration
├── README.md                 # Main documentation (English)
├── README_zh.md             # Chinese documentation
├── requirements.txt          # Python dependencies
└── setup.py                  # Package installation configuration
```

## Core Directories

### 📦 `/raganything` - Main Package

The core library containing all functionality for the RAG-Anything system.

**Key Modules:**

- **`__init__.py`** - Package initialization, exports main classes (`RAGAnything`, `RAGAnythingConfig`)
- **`raganything.py`** - Main RAGAnything class combining document parsing, content insertion, and query functionality
- **`config.py`** - Configuration dataclass (`RAGAnythingConfig`) with environment variable support
- **`base.py`** - Base classes and common functionality
- **`parser.py`** - Document parsers (`MineruParser`, `DoclingParser`) for converting documents to structured formats
- **`processor.py`** - ProcessorMixin for handling document processing workflows
- **`modalprocessors.py`** - Specialized processors for different content modalities:
  - `ImageModalProcessor` - Image content processing
  - `TableModalProcessor` - Table content processing
  - `EquationModalProcessor` - Mathematical equation processing
  - `GenericModalProcessor` - Generic content processing
  - `ContextExtractor` - Extract contextual information
- **`query.py`** - QueryMixin for handling different query modes (naive, local, global, hybrid, multimodal)
- **`batch.py`** - BatchMixin for parallel batch processing of multiple documents
- **`batch_parser.py`** - Batch parsing functionality
- **`prompt.py`** - Prompt templates and management for LLM interactions
- **`utils.py`** - Utility functions (processor support checks, file operations, etc.)
- **`enhanced_markdown.py`** - Enhanced markdown conversion with syntax highlighting and styling

### 📚 `/docs` - Documentation

Comprehensive documentation for various features and use cases.

**Files:**

- **`batch_processing.md`** - Batch processing feature documentation (parallel document processing)
- **`context_aware_processing.md`** - Context-aware processing and configuration
- **`enhanced_markdown.md`** - Enhanced markdown conversion feature guide
- **`offline_setup.md`** - Setup guide for offline/air-gapped environments

### 🎯 `/examples` - Example Scripts

Practical examples demonstrating how to use RAG-Anything features.

**Files:**

- **`raganything_example.py`** - Basic usage example
- **`batch_processing_example.py`** - Batch processing demonstration
- **`batch_dry_run_example.py`** - Batch dry-run mode example
- **`modalprocessors_example.py`** - Multimodal processor usage
- **`enhanced_markdown_example.py`** - Enhanced markdown conversion example
- **`insert_content_list_example.py`** - Content list insertion example
- **`lmstudio_integration_example.py`** - LM Studio integration
- **`image_format_test.py`** - Image format handling tests
- **`text_format_test.py`** - Text format handling tests
- **`office_document_test.py`** - Office document processing tests

### 🔧 `/scripts` - Utility Scripts

Helper scripts for setup and maintenance.

**Files:**

- **`create_tiktoken_cache.py`** - Creates tiktoken cache for offline usage

### 🖼️ `/assets` - Static Assets

Contains images, logos, and diagrams used in documentation and README files.

### ⚙️ `.github/` - GitHub Configuration

GitHub-specific configurations for project management and automation.

**Contents:**

- **`workflows/`** - GitHub Actions workflows for CI/CD
- **`ISSUE_TEMPLATE/`** - Templates for bug reports and feature requests
- **`dependabot.yml`** - Automated dependency updates configuration
- **`pull_request_template.md`** - PR template for contributors

## Key Configuration Files

### `pyproject.toml`

Modern Python project configuration file (PEP 518/621) defining:
- Project metadata (name, version, authors)
- Dependencies (core and optional)
- Build system requirements
- Tool configurations (ruff, setuptools)
- Optional dependency groups:
  - `[image]` - Image format support (Pillow)
  - `[text]` - Text file conversion (reportlab)
  - `[markdown]` - Enhanced markdown (markdown, weasyprint, pygments)
  - `[office]` - Office documents (requires LibreOffice)
  - `[all]` - All optional features

### `setup.py`

Legacy setup script for package installation, reading configuration from:
- `raganything/__init__.py` (version, author, URL)
- `requirements.txt` (dependencies)
- `README.md` (long description)

### `requirements.txt`

Core Python dependencies:
- `huggingface_hub` - Hugging Face model hub integration
- `lightrag-hku` - LightRAG framework (base system)
- `mineru[core]` - MinerU 2.0 for document parsing
- `tqdm` - Progress bars for batch processing

### `env.example`

Template for environment variables including:
- Working directory configuration
- Parser settings (method, output directory)
- Multimodal processing toggles
- API keys and endpoints (OpenAI, LM Studio, etc.)
- Model configurations
- Debug and display settings

### `.pre-commit-config.yaml`

Pre-commit hooks configuration for code quality:
- Code formatting checks
- Linting rules
- File validation

### `MANIFEST.in`

Specifies additional files to include in the package distribution (non-Python files).

## Architecture Overview

### Core Components

1. **RAGAnything Class** (`raganything.py`)
   - Inherits from QueryMixin, ProcessorMixin, and BatchMixin
   - Coordinates document parsing, content processing, and querying
   - Main entry point for using the system

2. **Configuration System** (`config.py`)
   - Environment-variable-driven configuration
   - Supports all parser, processor, and model settings
   - Flexible and extensible design

3. **Document Parsers** (`parser.py`)
   - MineruParser - Uses MinerU for document parsing
   - DoclingParser - Uses Docling as alternative parser
   - Automatic format detection and conversion

4. **Modal Processors** (`modalprocessors.py`)
   - Specialized processors for different content types
   - Image, table, equation, and generic processors
   - Context-aware processing with ContextExtractor

5. **Query System** (`query.py`)
   - Multiple query modes (naive, local, global, hybrid)
   - Multimodal query support with VLM integration
   - Flexible and extensible query pipeline

6. **Batch Processing** (`batch.py`)
   - Parallel processing with thread pools
   - Progress tracking with tqdm
   - Error handling and recovery

## Installation

### Basic Installation
```bash
pip install raganything
```

### With Optional Features
```bash
# All features
pip install raganything[all]

# Specific features
pip install raganything[image,text,markdown]
```

### Development Installation
```bash
git clone https://github.com/HKUDS/RAG-Anything.git
cd RAG-Anything
pip install -e ".[all]"
```

## Usage Flow

1. **Configuration**: Create `RAGAnythingConfig` or use environment variables
2. **Initialization**: Instantiate `RAGAnything` with configuration
3. **Document Processing**: 
   - Single document: `process_document()`
   - Batch processing: `process_batch()`
4. **Querying**: Use `query()` method with desired mode
5. **Results**: Retrieve and process query results

## Development

### Code Organization

- **Core Logic**: `raganything/` package
- **Examples**: `examples/` directory  
- **Tests**: Distributed as example scripts
- **Documentation**: `docs/` directory + README files

### Contributing

1. Follow the existing code structure
2. Add examples for new features in `examples/`
3. Document features in `docs/` directory
4. Update README files as needed
5. Use pre-commit hooks for code quality

## License

MIT License - See `LICENSE` file for details.

## Related Projects

- [LightRAG](https://github.com/HKUDS/LightRAG) - Base RAG framework
- [MinerU](https://github.com/opendatalab/MinerU) - Document parsing engine

## Support

- **GitHub Issues**: [Report bugs or request features](https://github.com/HKUDS/RAG-Anything/issues)
- **Discord**: [Join the community](https://discord.gg/yF2MmDJyGJ)
- **WeChat**: See README for group information

---

*Last Updated: February 2025*
