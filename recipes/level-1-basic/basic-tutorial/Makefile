# Terminal colors
GREEN  := $(shell tput -Txterm setaf 2)
YELLOW := $(shell tput -Txterm setaf 3)
WHITE  := $(shell tput -Txterm setaf 7)
RESET  := $(shell tput -Txterm sgr0)
BLUE   := $(shell tput -Txterm setaf 4)
RED    := $(shell tput -Txterm setaf 1)

# Recipe settings
RECIPE_NAME := basic-tutorial
PYTHON := $(shell which python)
RASA := $(PYTHON) -m rasa

# Default config
CONFIG_FILE := config.yml
DEFAULT_CONFIG := config-openai.yml

help: ## Show this help message
	@echo ''
	@echo '${YELLOW}$(RECIPE_NAME) - Recipe Commands${RESET}'
	@echo ''
	@echo '${YELLOW}Setup:${RESET}'
	@echo '  Setup:        ${GREEN}make setup-recipe${RESET}    - Install recipe dependencies'
	@echo '  Config:       ${GREEN}make config-openai${RESET}   - Use OpenAI configuration'
	@echo '  Config:       ${GREEN}make config-azure${RESET}    - Use Azure OpenAI configuration'
	@echo '  Config:       ${GREEN}make config-local${RESET}    - Use local LLM configuration'
	@echo ''
	@echo '${YELLOW}Development:${RESET}'
	@echo '  Train:        ${GREEN}make train${RESET}           - Train the model'
	@echo '  Shell:        ${GREEN}make shell${RESET}           - Start interactive shell'
	@echo '  Inspect:      ${GREEN}make inspect${RESET}         - Start inspector (recommended)'
	@echo '  Run:          ${GREEN}make run${RESET}             - Start server'
	@echo '  Actions:      ${GREEN}make run-actions${RESET}     - Start action server'
	@echo ''
	@echo '${YELLOW}Testing:${RESET}'
	@echo '  Test E2E:     ${GREEN}make test-e2e${RESET}        - Run end-to-end tests'
	@echo '  Validate:     ${GREEN}make validate${RESET}        - Validate configuration'
	@echo ''
	@echo '${YELLOW}Utilities:${RESET}'
	@echo '  Clean:        ${GREEN}make clean${RESET}           - Clean generated files'
	@echo '  Reset:        ${GREEN}make reset${RESET}           - Reset to fresh state'
	@echo ''
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "  ${YELLOW}%-15s${GREEN}%s${RESET}\n", $$1, $$2}' $(MAKEFILE_LIST)
	@echo ''

.PHONY: check-env
check-env: ## Check environment and prerequisites
	@echo "${BLUE}Checking environment...${RESET}"
	@if [ -z "$$RASA_LICENSE" ]; then \
		echo "${RED}Error: RASA_LICENSE environment variable not set${RESET}"; \
		echo "Export your license: export RASA_LICENSE='your-license-key'"; \
		exit 1; \
	fi
	@if [ ! -f "$(CONFIG_FILE)" ]; then \
		echo "${YELLOW}No config.yml found. Using default: $(DEFAULT_CONFIG)${RESET}"; \
		cp $(DEFAULT_CONFIG) $(CONFIG_FILE); \
	fi
	@echo "${GREEN}✓ Environment check passed${RESET}"

.PHONY: setup-recipe
setup-recipe: ## Install recipe-specific dependencies
	@echo "${BLUE}Installing recipe dependencies...${RESET}"
	@if [ -f "pyproject.toml" ]; then \
		uv pip install -e .; \
	else \
		echo "${YELLOW}No pyproject.toml found, using base dependencies${RESET}"; \
	fi
	@echo "${GREEN}✓ Recipe dependencies installed${RESET}"

.PHONY: config-openai
config-openai: ## Use OpenAI configuration
	@echo "${BLUE}Setting up OpenAI configuration...${RESET}"
	@cp config-openai.yml config.yml
	@echo "${GREEN}✓ OpenAI configuration active${RESET}"
	@echo "${YELLOW}Make sure to set OPENAI_API_KEY environment variable${RESET}"

.PHONY: config-azure
config-azure: ## Use Azure OpenAI configuration
	@echo "${BLUE}Setting up Azure OpenAI configuration...${RESET}"
	@cp config-azure.yml config.yml
	@echo "${GREEN}✓ Azure OpenAI configuration active${RESET}"
	@echo "${YELLOW}Make sure to set AZURE_OPENAI_ENDPOINT and AZURE_OPENAI_API_KEY${RESET}"

.PHONY: config-local
config-local: ## Use local LLM configuration
	@echo "${BLUE}Setting up local LLM configuration...${RESET}"
	@cp config-local.yml config.yml
	@echo "${GREEN}✓ Local LLM configuration active${RESET}"
	@echo "${YELLOW}Make sure your local LLM server is running on localhost:8000${RESET}"

.PHONY: train
train: check-env ## Train the Rasa model
	@echo "${BLUE}Training Rasa model...${RESET}"
	$(RASA) train
	@echo "${GREEN}✓ Model training completed${RESET}"

.PHONY: shell
shell: check-env ## Start Rasa shell
	@echo "${BLUE}Starting Rasa shell...${RESET}"
	$(RASA) shell

.PHONY: inspect
inspect: check-env ## Start Rasa inspector (recommended for testing)
	@echo "${BLUE}Starting Rasa inspector...${RESET}"
	@echo "${YELLOW}Inspector will open in your browser${RESET}"
	$(RASA) inspect

.PHONY: run
run: check-env ## Start Rasa server
	@echo "${BLUE}Starting Rasa server...${RESET}"
	$(RASA) run --enable-api --cors "*"

.PHONY: run-actions
run-actions: ## Start action server
	@echo "${BLUE}Starting action server...${RESET}"
	$(RASA) run actions

.PHONY: test-e2e
test-e2e: check-env ## Run end-to-end tests
	@echo "${BLUE}Running end-to-end tests...${RESET}"
	@if [ -f "tests/e2e_test_cases.yml" ]; then \
		$(RASA) test e2e tests/e2e_test_cases.yml; \
		echo "${GREEN}✓ E2E tests completed${RESET}"; \
	else \
		echo "${YELLOW}No E2E tests found${RESET}"; \
	fi

.PHONY: validate
validate: ## Validate configuration files
	@echo "${BLUE}Validating configuration...${RESET}"
	@if [ -f "config.yml" ]; then \
		$(RASA) data validate; \
		echo "${GREEN}✓ Configuration is valid${RESET}"; \
	else \
		echo "${RED}No config.yml found${RESET}"; \
		exit 1; \
	fi

.PHONY: clean
clean: ## Clean generated files
	@echo "${BLUE}Cleaning generated files...${RESET}"
	rm -rf models/ logs/ .rasa/
	find . -name "*.tar.gz" -delete
	find . -name "__pycache__" -type d -exec rm -rf {} +
	find . -name "*.pyc" -delete
	@echo "${GREEN}✓ Cleanup completed${RESET}"

.PHONY: reset
reset: clean ## Reset to fresh state
	@echo "${BLUE}Resetting recipe to fresh state...${RESET}"
	@if [ -f "config.yml" ] && [ -f "$(DEFAULT_CONFIG)" ]; then \
		rm config.yml; \
		echo "Removed config.yml (will use default on next run)"; \
	fi
	@echo "${GREEN}✓ Recipe reset completed${RESET}"

.PHONY: quick-start
quick-start: ## Quick start: setup, config, and train
	@echo "${BLUE}Quick start for $(RECIPE_NAME)...${RESET}"
	@make setup-recipe
	@make config-openai
	@make train
	@echo ""
	@echo "${GREEN}✓ Quick start completed!${RESET}"
	@echo "${YELLOW}Next steps:${RESET}"
	@echo "  1. Set environment variables (if not done already)"
	@echo "  2. ${GREEN}make inspect${RESET} - Test your assistant"
	@echo "  3. ${GREEN}make test-e2e${RESET} - Run tests"

.DEFAULT_GOAL := help