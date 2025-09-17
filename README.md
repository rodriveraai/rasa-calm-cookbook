# Rasa CALM Cookbook

A comprehensive collection of recipes for building conversational AI assistants with Rasa's Conversational AI with Language Models (CALM) approach.

## Repository Structure

```
rasa-calm-cookbook/
├── README.md
├── pyproject.toml
├── Makefile
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci.yml
├── recipes/
│   ├── level-1-basic/
│   │   ├── basic-tutorial/
│   │   │   ├── README.md
│   │   │   ├── pyproject.toml
│   │   │   ├── config-openai.yml
│   │   │   ├── config-azure.yml
│   │   │   ├── config-local.yml
│   │   │   ├── domain.yml
│   │   │   ├── endpoints.yml
│   │   │   ├── data/
│   │   │   │   └── flows.yml
│   │   │   ├── actions/
│   │   │   │   ├── __init__.py
│   │   │   │   └── actions.py
│   │   │   ├── tests/
│   │   │   │   └── e2e_test_cases.yml
│   │   │   └── conversations/
│   │   │       └── sample_conversations.md
│   │   └── custom-actions/
│   │       ├── README.md
│   │       ├── pyproject.toml
│   │       ├── config-openai.yml
│   │       ├── config-azure.yml
│   │       ├── config-local.yml
│   │       ├── domain.yml
│   │       ├── endpoints.yml
│   │       ├── data/
│   │       │   └── flows.yml
│   │       ├── actions/
│   │       │   ├── __init__.py
│   │       │   └── actions.py
│   │       ├── tests/
│   │       │   └── e2e_test_cases.yml
│   │       └── conversations/
│   │           └── sample_conversations.md
│   ├── level-2-intermediate/
│   │   ├── voice-assistant/
│   │   │   ├── README.md
│   │   │   ├── pyproject.toml
│   │   │   ├── config-openai.yml
│   │   │   ├── config-azure.yml
│   │   │   ├── config-local.yml
│   │   │   ├── domain.yml
│   │   │   ├── endpoints.yml
│   │   │   ├── credentials.yml
│   │   │   ├── data/
│   │   │   │   └── flows.yml
│   │   │   ├── actions/
│   │   │   │   ├── __init__.py
│   │   │   │   └── actions.py
│   │   │   ├── tests/
│   │   │   │   └── e2e_test_cases.yml
│   │   │   └── conversations/
│   │   │       └── sample_conversations.md
│   │   ├── enterprise-search/
│   │   │   ├── README.md
│   │   │   ├── pyproject.toml
│   │   │   ├── config-openai.yml
│   │   │   ├── config-azure.yml
│   │   │   ├── config-local.yml
│   │   │   ├── domain.yml
│   │   │   ├── endpoints.yml
│   │   │   ├── data/
│   │   │   │   ├── flows.yml
│   │   │   │   └── knowledge/
│   │   │   │       ├── faq.md
│   │   │   │       └── product_docs.md
│   │   │   ├── actions/
│   │   │   │   ├── __init__.py
│   │   │   │   └── actions.py
│   │   │   ├── tests/
│   │   │   │   └── e2e_test_cases.yml
│   │   │   └── conversations/
│   │   │       └── sample_conversations.md
│   │   └── multi-llm-routing/
│   │       ├── README.md
│   │       ├── pyproject.toml
│   │       ├── config-multi-provider.yml
│   │       ├── config-azure-regions.yml
│   │       ├── config-cost-optimization.yml
│   │       ├── domain.yml
│   │       ├── endpoints.yml
│   │       ├── data/
│   │       │   └── flows.yml
│   │       ├── actions/
│   │       │   ├── __init__.py
│   │       │   └── actions.py
│   │       ├── tests/
│   │       │   └── e2e_test_cases.yml
│   │       └── conversations/
│   │           └── sample_conversations.md
│   └── level-3-advanced/
│       ├── fine-tuning/
│       │   ├── README.md
│       │   ├── pyproject.toml
│       │   ├── config-base.yml
│       │   ├── config-finetuned.yml
│       │   ├── domain.yml
│       │   ├── endpoints.yml
│       │   ├── data/
│       │   │   └── flows.yml
│       │   ├── actions/
│       │   │   ├── __init__.py
│       │   │   └── actions.py
│       │   ├── tests/
│       │   │   └── e2e_test_cases.yml
│       │   ├── notebooks/
│       │   │   └── fine_tuning_example.ipynb
│       │   └── conversations/
│       │       └── sample_conversations.md
│       ├── deployment/
│       │   ├── README.md
│       │   ├── pyproject.toml
│       │   ├── docker/
│       │   │   ├── Dockerfile
│       │   │   ├── Dockerfile.actions
│       │   │   └── docker-compose.yml
│       │   ├── kubernetes/
│       │   │   ├── values.yml
│       │   │   ├── secrets.yml
│       │   │   └── deployment.yml
│       │   ├── config.yml
│       │   ├── domain.yml
│       │   ├── endpoints.yml
│       │   ├── data/
│       │   │   └── flows.yml
│       │   ├── actions/
│       │   │   ├── __init__.py
│       │   │   └── actions.py
│       │   ├── tests/
│       │   │   └── e2e_test_cases.yml
│       │   └── conversations/
│       │       └── sample_conversations.md
│       └── coexistence-migration/
│           ├── README.md
│           ├── pyproject.toml
│           ├── config-coexistence.yml
│           ├── config-pure-calm.yml
│           ├── domain.yml
│           ├── endpoints.yml
│           ├── data/
│           │   ├── flows.yml
│           │   ├── stories.yml
│           │   ├── rules.yml
│           │   └── nlu.yml
│           ├── actions/
│           │   ├── __init__.py
│           │   └── actions.py
│           ├── tests/
│           │   └── e2e_test_cases.yml
│           └── conversations/
│               └── sample_conversations.md
├── docs/
│   ├── getting-started.md
│   ├── recipe-guide.md
│   ├── configuration-guide.md
│   └── troubleshooting.md
└── scripts/
    ├── setup-recipe.sh
    ├── validate-recipes.py
    └── generate-docs.py
```

## Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/ducktyper-ai/rasa-calm-cookbook.git
   cd rasa-calm-cookbook
   ```

2. **Set up development environment**
   ```bash
   make setup
   source .venv/bin/activate
   ```

3. **Choose a recipe and get started**
   ```bash
   cd recipes/level-1-basic/basic-tutorial
   make quick-start
   ```

## Available Recipes

### Level 1 - Basic
- **basic-tutorial**: Money transfer assistant following the official Rasa tutorial
- **custom-actions**: Building assistants with external API integrations

### Level 2 - Intermediate  
- **voice-assistant**: Voice-enabled assistant with speech recognition and synthesis
- **enterprise-search**: RAG-powered assistant with document retrieval
- **multi-llm-routing**: Cost-optimized assistant with multiple LLM providers

### Level 3 - Advanced
- **fine-tuning**: Fine-tuning smaller LLMs for improved performance and cost reduction
- **deployment**: Production deployment with Docker and Kubernetes
- **coexistence-migration**: Migrating from NLU-based assistants to CALM

## Prerequisites

- Python 3.10 or 3.11
- uv package manager
- Valid Rasa Pro license
- OpenAI API key (or alternative LLM provider)

## Development

Each recipe is self-contained with its own dependencies and can be run independently. See individual recipe READMEs for specific setup instructions.

### Contributing

1. Fork the repository
2. Create a new recipe following the established structure
3. Test your recipe thoroughly
4. Submit a pull request

## License

This project is licensed under the AGPL-3.0 License - see the LICENSE file for details.

## Support

- [Rasa Documentation](https://docs.rasa.com)
- [Rasa Community Forum](https://forum.rasa.com)
- [GitHub Issues](https://github.com/ducktyper-ai/rasa-calm-cookbook/issues)