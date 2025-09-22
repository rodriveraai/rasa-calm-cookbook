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
UV := $(shell which uv)

# Default config
CONFIG_FILE := config.yml
DEFAULT_CONFIG := config-openai.yml
ENV_FILE := .env
ENV_EXAMPLE := .env.example

# Include environment variables if .env exists
-include $(ENV_FILE)

help: ## Show this help message
	@echo ''
	@echo '${YELLOW}$(RECIPE_NAME) - Recipe Commands${RESET}'
	@echo ''
	@echo '${YELLOW}Setup:${RESET}'
	@echo '  Setup:        ${GREEN}make setup-recipe${RESET}    - Install recipe dependencies'
	@echo '  Environment:  ${GREEN}make setup-env${RESET}       - Create environment file'
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
	@echo '  Status:       ${GREEN}make status${RESET}          - Show environment status'
	@echo ''
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "  ${YELLOW}%-15s${GREEN}%s${RESET}\n", $$1, $$2}' $(MAKEFILE_LIST)
	@echo ''

.PHONY: setup-env
setup-env: ## Create .env file from template
	@if [ ! -f "$(ENV_FILE)" ]; then \
		if [ -f "$(ENV_EXAMPLE)" ]; then \
			echo "${BLUE}Creating $(ENV_FILE) from $(ENV_EXAMPLE)...${RESET}"; \
			cp $(ENV_EXAMPLE) $(ENV_FILE); \
			echo "${YELLOW}Please edit $(ENV_FILE) with your actual API keys and credentials${RESET}"; \
			echo "${YELLOW}Required variables:${RESET}"; \
			grep -E "^[A-Z_]+=.*-here" $(ENV_FILE) | cut -d'=' -f1 | sed 's/^/  - /' || true; \
		else \
			echo "${BLUE}Creating basic $(ENV_FILE)...${RESET}"; \
			echo "# Environment Variables for $(RECIPE_NAME)" > $(ENV_FILE); \
			echo "RASA_LICENSE=your-rasa-pro-license-key-here" >> $(ENV_FILE); \
			echo "OPENAI_API_KEY=your-openai-api-key-here" >> $(ENV_FILE); \
			echo "LOG_LEVEL=INFO" >> $(ENV_FILE); \
			echo "DEBUG=false" >> $(ENV_FILE); \
			echo "${YELLOW}Please edit $(ENV_FILE) with your actual credentials${RESET}"; \
		fi; \
	else \
		echo "${GREEN}$(ENV_FILE) already exists${RESET}"; \
	fi

.PHONY: check-env
check-env: ## Check environment and prerequisites
	@echo "${BLUE}Checking environment...${RESET}"
	@# Load environment variables if .env exists
	@if [ -f "$(ENV_FILE)" ]; then \
		echo "Loading environment from $(ENV_FILE)..."; \
		export $$(grep -v '^#' $(ENV_FILE) | xargs); \
	fi
	@# Check for required environment variables
	@if [ -z "$$RASA_LICENSE" ]; then \
		echo "${RED}Error: RASA_LICENSE environment variable not set${RESET}"; \
		echo "Options:"; \
		echo "  1. Run: ${GREEN}make setup-env${RESET} and edit .env file"; \
		echo "  2. Export directly: ${GREEN}export RASA_LICENSE='your-license-key'${RESET}"; \
		exit 1; \
	fi
	@# Check for LLM provider credentials
	@provider_found=false; \
	if [ -n "$$OPENAI_API_KEY" ]; then \
		echo "${GREEN}✓ OpenAI API key found${RESET}"; \
		provider_found=true; \
	fi; \
	if [ -n "$$AZURE_OPENAI_ENDPOINT" ] && [ -n "$$AZURE_OPENAI_API_KEY" ]; then \
		echo "${GREEN}✓ Azure OpenAI credentials found${RESET}"; \
		provider_found=true; \
	fi; \
	if [ -n "$$LOCAL_LLM_ENDPOINT" ]; then \
		echo "${GREEN}✓ Local LLM endpoint configured${RESET}"; \
		provider_found=true; \
	fi; \
	if [ "$$provider_found" = "false" ]; then \
		echo "${YELLOW}⚠ No LLM provider credentials found${RESET}"; \
		echo "Set one of: OPENAI_API_KEY, AZURE_OPENAI_*, or LOCAL_LLM_ENDPOINT"; \
	fi
	@# Check config file
	@if [ ! -f "$(CONFIG_FILE)" ]; then \
		echo "${YELLOW}No config.yml found. Using default: $(DEFAULT_CONFIG)${RESET}"; \
		cp $(DEFAULT_CONFIG) $(CONFIG_FILE); \
	fi
	@echo "${GREEN}✓ Environment check completed${RESET}"

.PHONY: status
status: ## Show environment status
	@echo "${BLUE}Environment Status:${RESET}"
	@echo ""
	@echo "${YELLOW}Files:${RESET}"
	@if [ -f "$(ENV_FILE)" ]; then \
		echo "  ✓ .env file exists"; \
	else \
		echo "  ✗ .env file missing (run 'make setup-env')"; \
	fi
	@if [ -f "$(CONFIG_FILE)" ]; then \
		echo "  ✓ config.yml exists"; \
	else \
		echo "  ✗ config.yml missing"; \
	fi
	@echo ""
	@echo "${YELLOW}Environment Variables:${RESET}"
	@if [ -n "$$RASA_LICENSE" ]; then \
		echo "  ✓ RASA_LICENSE is set"; \
	else \
		echo "  ✗ RASA_LICENSE not set"; \
	fi
	@if [ -n "$$OPENAI_API_KEY" ]; then \
		echo "  ✓ OPENAI_API_KEY is set"; \
	else \
		echo "  ✗ OPENAI_API_KEY not set"; \
	fi
	@if [ -n "$$AZURE_OPENAI_ENDPOINT" ]; then \
		echo "  ✓ AZURE_OPENAI_ENDPOINT is set"; \
	else \
		echo "  - AZURE_OPENAI_ENDPOINT not set"; \
	fi
	@if [ -n "$$LOCAL_LLM_ENDPOINT" ]; then \
		echo "  ✓ LOCAL_LLM_ENDPOINT is set"; \
	else \
		echo "  - LOCAL_LLM_ENDPOINT not set"; \
	fi
	@echo ""
	@echo "${YELLOW}Dependencies:${RESET}"
	@if command -v uv >/dev/null 2>&1; then \
		echo "  ✓ uv is installed"; \
	else \
		echo "  ✗ uv not found"; \
	fi
	@if $(PYTHON) -c "import rasa" 2>/dev/null; then \
		echo "  ✓ Rasa is installed"; \
	else \
		echo "  ✗ Rasa not installed"; \
	fi

.PHONY: setup-recipe
setup-recipe: setup-env ## Install recipe-specific dependencies
	@echo "${BLUE}Installing recipe dependencies...${RESET}"
	@if [ -f "pyproject.toml" ]; then \
		echo "Installing dependencies from pyproject.toml..."; \
		$(UV) pip install -e .; \
	else \
		echo "${YELLOW}No pyproject.toml found, installing base dependencies${RESET}"; \
		$(UV) pip install rasa-pro rasa-sdk openai; \
	fi
	@echo "${GREEN}✓ Recipe dependencies installed${RESET}"

.PHONY: config-openai
config-openai: ## Use OpenAI configuration
	@echo "${BLUE}Setting up OpenAI configuration...${RESET}"
	@cp config-openai.yml config.yml
	@echo "${GREEN}✓ OpenAI configuration active${RESET}"
	@if [ -z "$$OPENAI_API_KEY" ]; then \
		echo "${YELLOW}⚠ Remember to set OPENAI_API_KEY environment variable${RESET}"; \
		echo "Add it to your .env file or export it directly"; \
	fi

.PHONY: config-azure
config-azure: ## Use Azure OpenAI configuration
	@echo "${BLUE}Setting up Azure OpenAI configuration...${RESET}"
	@cp config-azure.yml config.yml
	@echo "${GREEN}✓ Azure OpenAI configuration active${RESET}"
	@if [ -z "$$AZURE_OPENAI_ENDPOINT" ] || [ -z "$$AZURE_OPENAI_API_KEY" ]; then \
		echo "${YELLOW}⚠ Remember to set Azure OpenAI environment variables:${RESET}"; \
		echo "  AZURE_OPENAI_ENDPOINT"; \
		echo "  AZURE_OPENAI_API_KEY"; \
	fi

.PHONY: config-local
config-local: ## Use local LLM configuration
	@echo "${BLUE}Setting up local LLM configuration...${RESET}"
	@cp config-local.yml config.yml
	@echo "${GREEN}✓ Local LLM configuration active${RESET}"
	@echo "${YELLOW}Make sure your local LLM server is running${RESET}"
	@if [ -n "$$LOCAL_LLM_ENDPOINT" ]; then \
		echo "Configured endpoint: $$LOCAL_LLM_ENDPOINT"; \
	else \
		echo "Default endpoint: http://localhost:8000/v1"; \
	fi

.PHONY: load-env
load-env: ## Load environment variables from .env file
	@if [ -f "$(ENV_FILE)" ]; then \
		echo "${BLUE}Loading environment variables from $(ENV_FILE)...${RESET}"; \
		export $$(grep -v '^#' $(ENV_FILE) | xargs); \
		echo "${GREEN}✓ Environment variables loaded${RESET}"; \
	else \
		echo "${YELLOW}No $(ENV_FILE) file found. Run 'make setup-env' first.${RESET}"; \
	fi

.PHONY: train
train: check-env load-env ## Train the Rasa model
	@echo "${BLUE}Training Rasa model...${RESET}"
	@if [ -f "$(ENV_FILE)" ]; then \
		export $$(grep -v '^#' $(ENV_FILE) | xargs) && $(RASA) train; \
	else \
		$(RASA) train; \
	fi
	@echo "${GREEN}✓ Model training completed${RESET}"

.PHONY: shell
shell: check-env load-env ## Start Rasa shell
	@echo "${BLUE}Starting Rasa shell...${RESET}"
	@if [ -f "$(ENV_FILE)" ]; then \
		export $$(grep -v '^#' $(ENV_FILE) | xargs) && $(RASA) shell; \
	else \
		$(RASA) shell; \
	fi

.PHONY: inspect
inspect: check-env load-env ## Start Rasa inspector (recommended for testing)
	@echo "${BLUE}Starting Rasa inspector...${RESET}"
	@echo "${YELLOW}Inspector will open in your browser${RESET}"
	@if [ -f "$(ENV_FILE)" ]; then \
		export $$(grep -v '^#' $(ENV_FILE) | xargs) && $(RASA) inspect; \
	else \
		$(RASA) inspect; \
	fi

.PHONY: run
run: check-env load-env ## Start Rasa server
	@echo "${BLUE}Starting Rasa server...${RESET}"
	@if [ -f "$(ENV_FILE)" ]; then \
		export $$(grep -v '^#' $(ENV_FILE) | xargs) && $(RASA) run --enable-api --cors "*"; \
	else \
		$(RASA) run --enable-api --cors "*"; \
	fi

.PHONY: run-actions
run-actions: load-env ## Start action server
	@echo "${BLUE}Starting action server...${RESET}"
	@if [ -f "$(ENV_FILE)" ]; then \
		export $$(grep -v '^#' $(ENV_FILE) | xargs) && $(RASA) run actions; \
	else \
		$(RASA) run actions; \
	fi

.PHONY: test-e2e
test-e2e: check-env load-env ## Run end-to-end tests
	@echo "${BLUE}Running end-to-end tests...${RESET}"
	@if [ -f "tests/e2e_test_cases.yml" ]; then \
		if [ -f "$(ENV_FILE)" ]; then \
			export $$(grep -v '^#' $(ENV_FILE) | xargs) && $(RASA) test e2e tests/e2e_test_cases.yml; \
		else \
			$(RASA) test e2e tests/e2e_test_cases.yml; \
		fi; \
		echo "${GREEN}✓ E2E tests completed${RESET}"; \
	else \
		echo "${YELLOW}No E2E tests found${RESET}"; \
	fi

.PHONY: validate
validate: check-env ## Validate configuration files
	@echo "${BLUE}Validating configuration...${RESET}"
	@if [ -f "config.yml" ]; then \
		if [ -f "$(ENV_FILE)" ]; then \
			export $$(grep -v '^#' $(ENV_FILE) | xargs) && $(RASA) data validate; \
		else \
			$(RASA) data validate; \
		fi; \
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
	find . -name "__pycache__" -type d -exec rm -rf {} + 2>/dev/null || true
	find . -name "*.pyc" -delete
	@echo "${GREEN}✓ Cleanup completed${RESET}"

.PHONY: reset
reset: clean ## Reset to fresh state
	@echo "${BLUE}Resetting recipe to fresh state...${RESET}"
	@if [ -f "config.yml" ] && [ -f "$(DEFAULT_CONFIG)" ]; then \
		rm config.yml; \
		echo "Removed config.yml (will use default on next run)"; \
	fi
	@echo "${YELLOW}Note: .env file preserved. Remove manually if needed.${RESET}"
	@echo "${GREEN}✓ Recipe reset completed${RESET}"

.PHONY: quick-start
quick-start: ## Quick start: setup, config, and train
	@echo "${BLUE}Quick start for $(RECIPE_NAME)...${RESET}"
	@make setup-recipe
	@make config-openai
	@if [ -f "$(ENV_FILE)" ]; then \
		echo "${BLUE}Environment file exists, proceeding with training...${RESET}"; \
		make train; \
	else \
		echo "${YELLOW}No .env file found. Please:${RESET}"; \
		echo "  1. Run: ${GREEN}make setup-env${RESET}"; \
		echo "  2. Edit .env with your credentials"; \
		echo "  3. Run: ${GREEN}make train${RESET}"; \
		exit 1; \
	fi
	@echo ""
	@echo "${GREEN}✓ Quick start completed!${RESET}"
	@echo "${YELLOW}Next steps:${RESET}"
	@echo "  1. ${GREEN}make inspect${RESET} - Test your assistant"
	@echo "  2. ${GREEN}make test-e2e${RESET} - Run tests"

.DEFAULT_GOAL := help