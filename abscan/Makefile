#!/usr/bin/make -f

.PHONY: help
.DEFAULT_GOAL := help
SHELL ?= /bin/sh

MAKEFILE_PATH := $(abspath $(lastword $(MAKEFILE_LIST)))
FILE_PATH := $(realpath $(dir $(MAKEFILE_PATH)))

AGENT ?= 1
ENVIRONMENT ?= default
HOSTPORT ?= 80
SERVICEPORT ?= 80
DNSNAME ?= abscan.io
NAMESPACE ?= abscan

help:
	@for makefile in $(MAKEFILE_LIST) ; do \
		grep -E '^[0-9a-zA-Z_-]+:.*?## .*$$' $$makefile | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' ; \
	done

k3d-install: ## Deploys a local K3d cluster for development.
	@k3d cluster create abscan-dev --agents ${AGENT} --port "${HOSTPORT}:${SERVICEPORT}@loadbalancer" --k3s-arg "--disable=traefik@server:0" --k3s-arg "--tls-san=${DNSNAME}@server:*"

k3d-uninstall: ## Deletes the local K3d cluster. DESTRUCTIVE.
	@k3d cluster delete abscan-dev

helm-install: ## Deploys the Helm charts.
	@helmfile -e $(ENVIRONMENT) sync

helm-uninstall: ## Removes the Helm deployment. DESTRUCTIVE.
	@helmfile destroy
	@kubectl delete namespace $(NAMESPACE)

setup: k3d-install helm-install ## Full development stack deployment.

setup-uninstall: k3d-uninstall k8s-delete-namespace ## Removes the development stack deployment.