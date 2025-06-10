# Azure AI Evaluation SDK - Foundry Project Type (Microsoft.CognitiveService) Evaluation

![image](https://github.com/user-attachments/assets/5e6b4e6b-150a-43a4-8c7d-b05dea57a4e6)

This repository contains examples demonstrating how to use the Azure AI Evaluation SDK for evaluating AI applications and LLMs both locally and in the cloud.

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Usage](#usage)
  - [Local Evaluation](#local-evaluation)
  - [Cloud Evaluation](#cloud-evaluation)
- [Available Evaluators](#available-evaluators)
- [Data Format](#data-format)
- [License](#license)

## 🎯 Overview

This project provides practical examples of using the `azure-ai-evaluation` package to evaluate AI model outputs across various metrics including:

- Response quality (coherence, fluency, relevance)
- Safety metrics (hate/unfairness, violence, self-harm, sexual content)
- Similarity scores (BLEU, F1, GLEU, ROUGE)
- Task-specific metrics (groundedness, intent resolution, retrieval)

## 📚 Prerequisites

- Python 3.8+
- Azure subscription with access to:
  - Azure OpenAI Service
  - Azure AI Project (for cloud evaluations)
- An Azure OpenAI deployment with a GPT model supporting chat completion (e.g., `gpt-4`)

## 🚀 Installation

1. Clone this repository:
```bash
git clone https://github.com/yourusername/foundry_project_evalsdk.git
cd foundry_project_evalsdk
```

2. Install required dependencies:
```bash
pip install -r requirements.txt
```

## 📁 Project Structure

```
foundry_project_evalsdk/
├── data/
│   └── evaluate_test_data.jsonl    # Sample evaluation data
├── scenarios/
│   ├── .env                        # Environment variables (create from sample.env)
│   ├── sample.env                  # Template for environment variables
│   ├── local/
│   │   └── Evaluate_local.ipynb    # Local evaluation examples
│   └── cloud_eval/
│       └── Evaluate_On_Foundry_Proj.ipynb  # Cloud evaluation examples
├── requirements.txt                # Python dependencies
├── LICENSE                         # MIT License
└── README.md                       # This file
```

## ⚙️ Configuration

1. Copy the sample environment file:
```bash
cp scenarios/sample.env scenarios/.env
```

2. Edit `scenarios/.env` and set your Azure credentials:
```env
# Azure OpenAI Configuration
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com/
AZURE_OPENAI_KEY=your-api-key
AZURE_OPENAI_DEPLOYMENT=your-deployment-name
AZURE_OPENAI_API_VERSION=2025-01-01-preview

# Azure AI Project (for cloud evaluations and content safety)
AZURE_AI_PROJECT_URL=https://your-resource.services.ai.azure.com/api/projects/project-id
```

## 💻 Usage

### Local Evaluation

The [`Evaluate_local.ipynb`](scenarios/local/Evaluate_local.ipynb) notebook demonstrates:

1. **Batch evaluation** using multiple evaluators:
```python
from azure.ai.evaluation import evaluate, RelevanceEvaluator, CoherenceEvaluator

eval_results = evaluate(
    data="../../data/evaluate_test_data.jsonl",
    evaluators={
        "coherence": CoherenceEvaluator(model_config=model_config),
        "relevance": RelevanceEvaluator(model_config=model_config),
    }
)
```

2. **Individual evaluator usage** for specific metrics
3. **Saving results** to JSONL format for further analysis

### Cloud Evaluation

The [`Evaluate_On_Foundry_Proj.ipynb`](scenarios/cloud_eval/Evaluate_On_Foundry_Proj.ipynb) notebook shows how to:

1. **Upload datasets** to Azure AI Project
2. **Configure evaluators** for cloud execution
3. **Trigger remote evaluations** for scalable processing
4. **Monitor evaluation status** and retrieve results

Before running cloud evaluations:
```bash
az login --tenant your-tenant-id
```

## 📊 Available Evaluators

### Quality Metrics
- **Coherence**: Evaluates logical flow and consistency
- **Relevance**: Measures response relevance to query
- **Fluency**: Assesses language quality
- **Groundedness**: Checks if responses are grounded in provided context

### Safety Metrics
- **Content Safety**: Overall safety assessment
- **Hate/Unfairness**: Detects biased or hateful content
- **Violence**: Identifies violent content
- **Self-Harm**: Detects self-harm related content
- **Sexual**: Identifies sexual content

### Similarity Metrics
- **BLEU Score**: Measures n-gram overlap
- **F1 Score**: Precision and recall balance
- **ROUGE Score**: Text summarization quality
- **Semantic Similarity**: Deep semantic comparison

## 📄 Data Format

Evaluation data should be in JSONL format with the following structure:

```json
{
  "query": "What is the capital of France?",
  "context": "France is in Europe",
  "response": "Paris is the capital of France.",
  "ground_truth": "Paris is the capital of France."
}
```

See [`evaluate_test_data.jsonl`](data/evaluate_test_data.jsonl) for examples.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📚 Resources

- [Azure AI Evaluation SDK Documentation](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/develop/evaluate-sdk)
- [Azure AI Safety Evaluation](https://aka.ms/azureaistudiosafetyeval)
- [Data Requirements for Built-in Evaluators](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/develop/evaluate-sdk#data-requirements-for-built-in-evaluators)
- [Simulator for Generating Test Data](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/develop/simulator-interaction-data)

## ⚠️ Important Notes

- Keep your `.env` file secure and never commit it to version control
- Some evaluators require additional Azure AI Content Safety credentials
- Cloud evaluations require proper Azure authentication via `az login`
- API keys shown in notebooks should be replaced with your own credentials
