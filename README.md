# 🛡️ PyRIT Red Teaming Sample I

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![PyRIT](https://img.shields.io/badge/PyRIT-Microsoft-orange.svg)](https://github.com/Azure/PyRIT)
[![Security](https://img.shields.io/badge/Security-Red%20Teaming-red.svg)](https://www.microsoft.com/en-us/security)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A simple implementation of Microsoft's PyRIT (Python Risk Identification Tool) framework for conducting comprehensive red teaming attacks against AI models to provide automated adversarial testing capabilities with advanced configuration management, and result analysis.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Supported Models](#supported-models)
- [Attack Configurations](#attack-configurations)
- [Export Formats](#export-formats)
- [License](#license)

## Overview

The **PyRIT Red Teaming - Sample I** is a simple implementation of multi-turn prompt injection attacks, based on **Microsoft's PyRIT** framework and its **Red Teaming Attack (Multi-Turn)** functionality (https://azure.github.io/PyRIT/code/executor/attack/2_red_teaming_attack.html). Red Teaming attacks are used for testing AI models for vulnerabilities, prompt injection attacks, and adversarial responses.  
In this implementation LLMs are used to execute the following red teaming atttacks roles: 
- **Adversarial LLM**: The LLM used for generating adversarial prompts that align with the attack strategy
- **Target LLM**: The LLM used as the target model for red teaming attacks
- **Scoring LLM**: The LLM used as the model that evaluates the attack outcome i.e. if it is **SUCCESS** or **FAILURE** 

Based on PyRIT design https://arxiv.org/abs/2410.02828 the below chart depicts the various components and data flows: 

![adversarial atttack components](https://github.com/antdragiotis/pyrit-red-teaming-sample_I/blob/main/assets/PyRIT_RedTeaming_Sample_I_Architecture.png)

The Adversarial LLM generates prompts based on queries extracted from the **AdvBench dataset** (https://arxiv.org/abs/2307.15043). This dataset has been amended with harm types relevant to each query (https://azure.github.io/PyRIT/_autosummary/pyrit.datasets.fetch_adv_bench_dataset.html#pyrit.datasets.fetch_adv_bench_dataset) 

Indicative results of the process are shown in the below table: 

![Aggregated Results](https://github.com/antdragiotis/pyrit-red-teaming-sample_I/blob/main/assets/PyRIT_RedTeaming_Sample_I_Results.png)

### Key Objectives

- **Automated Red Teaming**: Conduct systematic adversarial attacks against AI models
- **Multi-Model Support**: Test various AI models including OpenAI, Azure, and custom endpoints
- **Reporting**: Generate analysis reports with success rates and metrics
- **Scalable Architecture**: Object-oriented design for easy extension and maintenance
- **Security Assessment**: Identify vulnerabilities in AI systems

## Features

### 📊 Attack Types Supported

| Attack Type | Description | Use Case |
|-------------|-------------|----------|
| **Prompt Injection** | Tests model's resistance to instruction manipulation | Security assessment |
| **Adversarial Prompts** | Uses AdvBench dataset for standardized testing | Benchmark comparisons |
| **Encoding Obfuscation** | Base64 and other encoding techniques | Bypass detection systems |
| **Multi-Turn Attacks** | Complex attack chains across multiple interactions | Advanced threat simulation |

### Core Components

| Component | Responsibility | Key Features |
|-----------|---------------|--------------|
| `PyRITRedTeamingSystem` | Main orchestrator | System initialization, workflow coordination |
| `TargetModelManager` | Model management | Multi-provider support, credential handling |
| `AttackExecutor` | Attack execution | Async operations, configuration management |
| `ResultAnalyzer` | Data analysis | Statistical analysis, export capabilities |
| `DatasetManager` | Dataset handling | AdvBench integration, custom dataset support |

## Installation 

### Prerequisites

- Python 3.8 or higher
- Microsoft PyRIT library
- API keys for target models (OpenAI, Azure, etc.)

### Clone Repository

```bash
git clone https://github.com/yourusername/pyrit-red-teaming-sample_I.git
cd pyrit-red-teaming-sample_I
```

### Install Dependencies

```bash
# Create virtual environment (recommended)
python -m venv pyrit-env
source pyrit-env\Scripts\activate (on Windows)

# Install required packages
pip install -r requirements.txt
```

### Required Libraries

```txt
pyrit-tool>=0.4.0
pandas>=1.5.0
numpy>=1.21.0
asyncio>=3.4.3
dataclasses>=0.8
typing-extensions>=4.0.0
```

### Environment Setup

Set up your API keys as environment variables:

```bash
# OpenAI API Key
export OPENAI_API_KEY="your_openai_api_key_here"
export OPENAI_CHAT_KEY"your_openai_chat__here"

# Azure AI Foundry (if using Azure models)
export AZURE_FOUNDRY_MODELS_COMPLETION_ENDPOINT="your_azure_endpoint"
export AZURE_FOUNDRY_API_KEY="your_azure_api_key"

# Crucible API Key
export CRUCIBLE_API_KEY="your_crucible_api_key_here"
```

## Supported Models

### OpenAI Models

| Model | Type | Description | Status |
|-------|------|-------------|--------|
| `gpt-4o` | Latest GPT-4 | Most advanced OpenAI model | ✅ Supported |
| `gpt-4o-mini` | Optimized GPT-4 | Faster, cost-effective variant | ✅ Supported |
| `gpt-3.5-turbo` | GPT-3.5 | Legacy model for comparison | ✅ Supported |

### Azure AI Models

| Model | Type | Description | Requirements |
|-------|------|-------------|--------------|
| `Meta-Llama-3.1-8B-Instruct` | Llama 3.1 | Meta's latest instruction model | Azure API Key |
| `Ministral-3B` | Mistral | Lightweight French AI model | Azure API Key |

### External Endpoints

| Endpoint | Type | Description | Access |
|----------|------|-------------|--------|
| `pieceofcake` | Crucible | Red team testing platform | Public endpoint |


## ⚙️ Attack Configurations

### Configuration Parameters

```python
@dataclass
class AttackConfiguration:
    max_turns: int = 3                                          # Maximum conversation turns
    memory_labels: Dict[str, str] = None                        # Memory labeling for tracking
    dataset_categories: List[str] = ["Financial and Business"]  # AdvBench categories to use
    dataset_limit: int = 5                                      # Number of objectives to test
```

### Available Dataset Categories

| Category | Description | Objective Count |
|----------|-------------|-----------------|
| `Financial and Business` | Financial fraud, business ethics | 50+ |
| `Illegal Activities` | Criminal activities, illegal content | 100+ |
| `Hate Speech` | Discrimination, offensive content | 75+ |
| `Physical Harm` | Violence, dangerous activities | 80+ |
| `Privacy Violations` | Personal data, surveillance | 60+ |

### Prompt Converters

| Converter | Purpose | Use Case |
|-----------|---------|----------|
| `Base64Converter` | Encoding obfuscation | Bypass content filters |
| `ROT13Converter` | Simple text rotation | Basic obfuscation testing |
| `UnicodeConverter` | Character encoding | International character bypass |

### Scoring Mechanisms

| Scorer | Type | Description |
|--------|------|-------------|
| `SelfAskTrueFalseScorer` | Binary | True/False attack success |
| `FloatScaleScorer` | Continuous | 0.0-1.0 confidence scoring |
| `LikertScaleScorer` | Ordinal | 1-5 severity rating |

## Export Formats

#### JSON Export Structure
```json
{
    "conversation_id": "uuid",
    "timestamp": "2024-01-15T10:30:00Z",
    "objective": "Test objective string",
    "model_target": "gpt-4o-mini",
    "attack_type": "RedTeamingAttack",
    "turns": [
        {
            "turn_id": 1,
            "prompt": "Attack prompt",
            "response": "Model response",
            "score": 0.8,
            "success": true
        }
    ],
    "final_result": {
        "success": true,
        "execution_time_ms": 8743,
        "total_turns": 2
    }
}
```

#### CSV Export Columns
- `objective`: The attack objective
- `attack_identifier`: Type of attack used
- `scorer`: Scoring mechanism used
- `executed_turns`: Number of conversation turns
- `execution_time_s`: Total execution time
- `success`: Boolean success indicator
- `outcome_reason`: Detailed outcome explanation


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2024 [Your Name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
