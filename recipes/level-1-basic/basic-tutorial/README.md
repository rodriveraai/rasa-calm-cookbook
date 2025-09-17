# Basic Tutorial - Money Transfer Assistant

A foundational recipe that implements the official Rasa tutorial for building a money transfer assistant using CALM (Conversational AI with Language Models).

## Overview

This recipe demonstrates:
- Basic flow definition for money transfers
- Slot collection and validation
- Custom actions for business logic
- Branching logic with confirmations
- End-to-end testing

## Use Case

A banking assistant that helps users transfer money to contacts with:
- Recipient selection
- Amount specification
- Balance validation
- Transfer confirmation
- Transaction completion

## Prerequisites

- Python 3.10 or 3.11
- Valid Rasa Pro license
- OpenAI API key (or alternative LLM provider)

## Quick Start

1. **Navigate to recipe directory**
   ```bash
   cd recipes/level-1-basic/basic-tutorial
   ```

2. **Set environment variables**
   ```bash
   export RASA_LICENSE="your-rasa-pro-license-key"
   export OPENAI_API_KEY="your-openai-api-key"
   ```

3. **Install dependencies**
   ```bash
   make setup-recipe
   ```

4. **Train the model**
   ```bash
   make train
   ```

5. **Test the assistant**
   ```bash
   make inspect
   ```

## Configuration Options

Choose your LLM provider by copying the appropriate config file:

- **OpenAI**: `cp config-openai.yml config.yml`
- **Azure OpenAI**: `cp config-azure.yml config.yml`  
- **Local/Self-hosted**: `cp config-local.yml config.yml`

## Sample Conversations

### Happy Path
```
User: I want to send money
Bot: Who would you like to send money to?
User: to Jen
Bot: How much would you like to send?
User: $50
Bot: Please confirm: you want to transfer $50.0 to Jen?
User: yes
Bot: All done. $50.0 has been sent to Jen.
```

### Insufficient Funds
```
User: I want to transfer money
Bot: Who would you like to send money to?
User: Alice
Bot: How much would you like to send?
User: $2000
Bot: You do not have enough funds to make this transaction.
```

### Change of Mind
```
User: Send money to Bob
Bot: How much would you like to send?
User: $100
Bot: Please confirm: you want to transfer $100.0 to Bob?
User: no
Bot: Your transfer has been cancelled.
```

## Key Learning Points

1. **Flow Definition**: Learn how to structure business logic as YAML flows
2. **Slot Collection**: Understand automatic slot filling with LLMs
3. **Custom Actions**: Implement business logic with external API calls
4. **Branching Logic**: Handle conditional paths in conversations
5. **Testing**: Write comprehensive E2E tests for conversation validation

## Testing

Run end-to-end tests:
```bash
make test-e2e
```

## Troubleshooting

- **License Issues**: Ensure `RASA_LICENSE` is properly set
- **API Errors**: Verify `OPENAI_API_KEY` is valid and has sufficient credits
- **Training Failures**: Check that all required files are present

## Next Steps

After mastering this recipe, try:
- [Custom Actions](../custom-actions/) - Learn advanced action server patterns
- [Voice Assistant](../../level-2-intermediate/voice-assistant/) - Add speech capabilities
- [Enterprise Search](../../level-2-intermediate/enterprise-search/) - Integrate RAG