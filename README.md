# Rasa CALM Cookbook

A comprehensive collection of recipes for building conversational AI assistants with Rasa's Conversational AI with Language Models (CALM) approach.

## Repository Structure

```
Rasa CALM Cookbook Structure:

.
├── .env.example
├── .gitignore
├── Makefile
├── pyproject.toml
├── README.md
└── recipes
    ├── level-1-basic
    │   └── basic-tutorial
    │       ├── .env
    │       ├── .env.example
    │       ├── actions
    │       │   └── actions.py
    │       ├── config-azure.yml
    │       ├── config-local.yml
    │       ├── config-openai.yml
    │       ├── conversations
    │       │   └── sample_conversations.md
    │       ├── data
    │       │   └── flows.yml
    │       ├── domain.yml
    │       ├── endpoints.yml
    │       ├── Makefile
    │       ├── pyproject.toml
    │       ├── README.md
    │       └── tests
    │           └── e2e_test_cases.yml
    ├── level-2-intermediate
    │   ├── enterprise-search
    │   │   └── README.md
    │   ├── multi-llm-routing
    │   │   └── README.md
    │   └── voice-assistant
    │       └── README.md
    └── level-3-advanced
        ├── coexistence-migration
        │   └── README.md
        ├── deployment
        │   └── README.md
        └── fine-tuning
            └── README.md

16 directories, 25 files

```

## Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/rodriveraai/rasa-calm-cookbook.git
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
   make setup-recipe
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
- [GitHub Issues](https://github.com/rodriveraai/rasa-calm-cookbook/issues)