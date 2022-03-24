# AlphaBravo Helmfile Repository
A respository for deploying Helm charts using [Helmfile](https://github.com/roboll/helmfile).

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
Helm values are stored unencrypted and can be found in the `charts/[app]/[environment]/values.yaml` file.

## Secrets
Secrets are stored encrypted and can be found in the `charts/[app]/[environment]/secrets.yaml` file.

### Decrypting Secrets
Secrets are encrypted using Helm Secrets and Age file encryption utility.

If you have access to a development, staging, or production copy of the key file, run the following command to enable decryption before running the `helmfile` command:
```bash
export SOPS_AGE_KEY_FILE=path/to/key.txt
```

