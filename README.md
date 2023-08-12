# Build Jekyll & Publish to Kube

A minimalist Github action to build, dockerize and publish your Jekyll website to Kubernetes.

## How it works
This action does the following:
- Builds your Jekyll website
- Builds a container image for your website using the Dockerfile in your repo's root directory. Don't have a Dockerfile? Consider the minimalist sample Dockerfile below.
- Uploads your Docker image to Dockerhub, the standard for free public container image storage.
- Updates a Kubernetes Deployment with the tag of your uploaded image

## How to use

Provide the required variables and secrets:

**Secrets**
- `docker_username`: Your DockerHub username (used with your Access Token to log in for image upload)
- `docker_password`: A [DockerHub Access Token](https://docs.docker.com/docker-hub/access-tokens/) with permissions to upload your container to DockerHub
- `kube_config`: A [base64 encoded](https://superuser.com/questions/120796/how-to-encode-base64-via-command-line) string of your Kubernetes cluster's config file

**Variables**
- `docker_name`: The full name of your DockerHub repository, e.g. "my-docker-name/my-repo-name"
- `kube_deployment`: The name of your Kubernetes Deployment resource to update with your latest image tag

### Sample workflow file
```
on: [push]
jobs:
  build-publish:
    name: Build & Publish
    uses: minicreative/build-publish-jekyll/.github/workflows/build-publish.yml@main
    with:
      docker_name: ${{ vars.docker_name }}
      kube_deployment: ${{ vars.kube_deployment }}
    secrets:
      docker_username: ${{ vars.docker_username }}
      docker_password: ${{ secrets.docker_password }}
      kube_config: ${{ secrets.kube_config }}
```

### Sample Dockerfile
```
FROM nginx:alpine
COPY ./_site/ /usr/share/nginx/html
```

