# SurfGen Setup

Protein-ligand docking 


## Google Vertex AI Deployment

### Prerequisite

1. Access to Google Cloud Artifact Registry. Location where docker images are deposited in order to GCP to create transient containers during pipeline run.

Install Google Cloud SDK.

```bash
curl https://sdk.cloud.google.com | bash
```
 
After installing, restart shell and initialize GCP credentials.
```bash
gcloud init
```

Lastly, configure docker to have access to GCP project registry. Change location, `us-central1-docker` if your project location is different.
```bash
gcloud auth configure-docker us-central1-docker.pkg.dev
```

### Deployment

To deploy image to GPC artifact registry run the following script.

```bash
bash artifact_registry.sh \
  esmdockfold:prod \
  us-central1-docker.pkg.dev/noble-office-299208/mercy-of-toren/esmdockfold:prod
```

Keep in mind that the official pipeline references the image with the `prod` tag. If you would like to have a test tag, feel free to change the tag to best fit your needs.

## Local Development

### Setup

1. Clone repository

```bash
git clone git@github.com:gabenavarro/SurfGen.git
# Jump into repository
cd ./SurfGen
```

2. Build docker image from docker file

```bash
docker build -f Dockerfile -t surfgen:dev .
```

3. Create docker container from docker image

With GPU allocation
```bash
docker run -d \
  --gpus all \
  --name surfgen-gpu-dev \
  -v $(pwd)/:/app/ \
  surfgen:dev
```

Without GPU allocation
```bash
docker run -d \
  --name surfgen-cpu-dev \
  -v $(pwd)/:/app/ \
  surfgen:dev
```