# GitLab

## CI Using DinD GitLab Shared Runner

Don't use this in case you have your own runners, bind mount docker socket instead and do docker commands directly.

```yaml
default:
  image: docker:24.0.5
  services:
    - docker:24.0.5-dind
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_JOB_TOKEN $CI_REGISTRY

build:
  stage: build
  variables:
    IMAGE_TAG: ${CI_REGISTRY_IMAGE}:$CI_COMMIT_REF_SLUG
  script:
    - docker build . -t $IMAGE_TAG
    - docker push $IMAGE_TAG
```

## GitLab Container Registry

Use the following script to connect to gitlab repository before pulling

```bash
#!/bin/bash

docker login registry.gitlab.com -u GITLAB_DEPLOY_TOKEN_USER --password-stdin <<<GITLAB_DEPLOY_TOKEN_SECRET
```
