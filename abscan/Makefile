#!/usr/bin/make -f

.PHONY: help
.DEFAULT_GOAL := help
SHELL ?= /bin/sh

MAKEFILE_PATH := $(abspath $(lastword $(MAKEFILE_LIST)))
FILE_PATH := $(realpath $(dir $(MAKEFILE_PATH)))

help:
	@for makefile in $(MAKEFILE_LIST) ; do \
		grep -E '^[0-9a-zA-Z_-]+:.*?## .*$$' $$makefile | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' ; \
	done

k3d-install: ## Deploys a local K3d cluster for development.
ifdef AGENT
	@k3d cluster create abscan-dev --agents $(AGENT) --port "80:80@loadbalancer"
else
	@k3d cluster create abscan-dev --agents 1 --port "80:80@loadbalancer"
endif

k3d-uninstall: ## Deletes the local K3d cluster. DESTRUCTIVE.
	@k3d cluster delete abscan-dev

helm-install: ## Deploys the Helm charts.
ifdef ENVIRONMENT
	@helmfiles -e $(ENVIRONMENT) sync
else
	@helmfiles sync
endif

helm-uninstall: ## Removes the Helm deployment. DESTRUCTIVE.
	@helmfiles destroy

setup: k3d-install helm-install ## Full development stack deployment.