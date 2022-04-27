# AlphaBravo Helmfile Repository
A respository for deploying Helm charts using [Helmfile](https://github.com/roboll/helmfile).

## Requirements
* [Helm](https://helm.sh/docs/intro/install/) >= v3.8.1
* [Helmfile](https://github.com/helmfile/helmfile) >= v0.143.1
* [Helm Secrets](https://github.com/jkroepke/helm-secrets) >= 3.12.0
* [sops](https://github.com/mozilla/sops) >= 3.7.2
* [Age](https://github.com/FiloSottile/age) >= v1.0.0

## Instructions
Development deployment:
```bash
helmfile sync
```

Production deployment:
```bash
helmfile -e production sync
```

## Values
Helm values are stored unencrypted and can be found in the `[app]/variables/[environment]/values.yaml` file.

## Secrets
Secrets are stored encrypted and can be found in the `[app]/variables/[environment]/secrets.yaml` file.

### Decrypting Secrets
Secrets are encrypted using Helm Secrets and Age file encryption utility.

If you have access to a development, staging, or production copy of the key file, run the following command to enable decryption before running the `helmfile` command:
```bash
export SOPS_AGE_KEY_FILE=path/to/key.txt
```

