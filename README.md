# packer-gce-goss

## Overview

This repository will create a GCE image and will run tests against it.

## Build Image

Execute the following to create the Packer Image:

```
packer build -var project_id="$project_id" packer.json
```

## Cloud Build Builder

Before using this builder , it must be built and pushed to the registry in your 
project. Run the following command:

```
cd cloudbuild/
gcloud builds submit .
```

## Create a KMS Key

Create a Keyring:

```
gcloud kms keyrings create packer \
  --location=global
```

Create the Key:

```
gcloud kms keys create key \
  --location=global \
  --keyring=packer \
  --purpose=encryption
```

Encrypt file:

```
gcloud kms encrypt \
  --plaintext-file=secrets.json \
  --ciphertext-file=secrets.json.enc \
  --location=global \
  --keyring=packer \
  --key=key
```
## Questions?

Open an issue.
