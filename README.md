# Build Jekyll & Publish to Kube

A minimalist Github action to build, dockerize and publish your Jekyll website to Kubernetes.

## How it works
This action does the following:
- Builds your Jekyll website
- Builds a Docker image for your website using the Dockerfile in your repo's root directory. Don't have a Dockerfile? Consider the minimalist sample Dockerfile below.
- Uploads your Docker image to Dockerhub, the standard for free public container image storage.
- Updates a Kubernetes Deployment with the tag of your uploaded image

## How to use

[Set the following environment variables and secrets](https://docs.github.com/en/actions/learn-github-actions/variables) in your repository settings:

**Secrets**
- `DOCKER_PASSWORD`: A [DockerHub Access Token](https://docs.docker.com/docker-hub/access-tokens/) with permissions to upload your container to DockerHub
- `KUBE_CONFIG`: A [base64 encoded](https://superuser.com/questions/120796/how-to-encode-base64-via-command-line) string of your Kubernetes cluster's config file

**Variables**
- `DOCKER_NAME`: The full name of your DockerHub repository, e.g. "my-docker-name/my-repo-name"
- `DOCKER_USERNAME`: Your DockerHub username (used with your Access Token to log in for image upload)
- `KUBE_DEPLOYMENT`: The name of your Kubernetes Deployment resource to update with your latest image tag

## Sample Dockerfile
```
FROM nginx:alpine
COPY ./_site/ /usr/share/nginx/html
```

