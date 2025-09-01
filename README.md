# Ollama Model Extractor for RAG & GGUF Projects

![llama](https://user-images.githubusercontent.com/1991296/230134379-7181e485-c521-4d23-a0d6-f7b3b61ba524.png)

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Release](https://img.shields.io/github/v/release/ggml-org/llama.cpp)](https://github.com/ggml-org/llama.cpp/releases)
[![Server](https://github.com/ggml-org/llama.cpp/actions/workflows/server.yml/badge.svg)](https://github.com/ggml-org/llama.cpp/actions/workflows/server.yml)

[Manifesto](https://github.com/ggml-org/llama.cpp/discussions/205) / [ggml](https://github.com/ggml-org/ggml) / [ops](https://github.com/ggml-org/llama.cpp/blob/master/docs/ops.md)

LLM inference in C/C++

## Recent API changes

- [Changelog for `libllama` API](https://github.com/ggml-org/llama.cpp/issues/9289)
- [Changelog for `llama-server` REST API](https://github.com/ggml-org/llama.cpp/issues/9291)


This script extracts and organizes Ollama model blobs into separate files for use with RAG (Retrieval-Augmented Generation) applications and GGUF model processing. It separates an Ollama model (e.g., Gemma3) into individual components:

- `model_quant.gguf` - The quantized model weights in GGUF format
- `params.txt` - Model parameters and configuration
- `license.txt` - License information
- `template.txt` - Prompt template
- `config.json` - Model configuration in JSON format

## Use Cases
- **RAG Applications**: Extract GGUF models for local inference in RAG pipelines
- **Model Analysis**: Separate model components for detailed examination
- **Custom Deployments**: Use extracted components in custom inference setups
- **llama.cpp Integration**: Direct compatibility with llama.cpp ecosystem

⚠️ **Important:**  
This repository **DOES NOT include the model weights**. You must download them legally from Ollama or another official source.  
Use of the models and derivatives is subject to the [Gemma Terms of Use](https://ai.google.dev/gemma/terms).

## Requirements
- Bash shell
- `jq` for JSON processing
- `cmake` and build tools (for llama.cpp integration)
- `libcurl4-openssl-dev` (dependency for network operations)

## Installation & Setup

### 1. Install Dependencies
```bash
# Update package list
sudo apt-get update

# Install all build dependencies
sudo apt-get install cmake build-essential clang libcurl4-openssl-dev jq

# Verify Clang installation
clang --version
```

### 2. Clone llama.cpp Repository
```bash
# Clone llama.cpp repository (official GGML organization repo)
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
```

### 3. Compile llama.cpp
```bash
# Create build directory
mkdir build
cd build

# Configure with Clang as compiler
cmake -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ ..

# Build the project
cmake --build . --config Release
```

### 4. Extract Ollama Model
```bash
# Navigate back to your project directory (if you were in llama.cpp/build)
cd /path/to/your/project

# Make the script executable
chmod +x extract_blobs.sh

# Run the extraction
./extract_blobs.sh
```

## Using Extracted Models

Once you have extracted the model files, you can use them with llama.cpp tools:

```bash
# Run inference with the extracted GGUF model
./llama.cpp/build/bin/llama-cli -m ~/model_quant_folder/model_quant.gguf -p "Hello, how are you?"

# Start an OpenAI-compatible API server
./llama.cpp/build/bin/llama-server -m ~/model_quant_folder/model_quant.gguf --port 8080

# Run in conversation mode
./llama.cpp/build/bin/llama-cli -m ~/model_quant_folder/model_quant.gguf -cnv
```

## Integration with RAG Applications

The extracted GGUF model can be integrated into RAG pipelines:

- **Local Inference**: Use the GGUF model for local text generation in RAG systems
- **Embedding Generation**: Extract embeddings for document indexing and retrieval
- **Custom Templates**: Utilize the extracted `template.txt` for consistent prompt formatting
- **API Integration**: Connect RAG applications to the llama-server endpoint

## Troubleshooting

### Common Build Issues
- **Missing libcurl**: If cmake fails, ensure `libcurl4-openssl-dev` is installed
- **Build tools missing**: Install `cmake` and `build-essential` packages
- **Permissions**: Ensure you have write permissions to the destination directory

### Model Extraction Issues
- **No manifest found**: Ensure Ollama is installed and has downloaded models
- **jq not found**: Install jq with `sudo apt-get install jq`
- **Blob files missing**: Verify Ollama model is fully downloaded
