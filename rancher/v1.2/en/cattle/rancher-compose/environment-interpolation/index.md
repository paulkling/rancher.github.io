---
title: Environment Interpolation in Rancher Compose
layout: rancher-default-v1.2
version: v1.2
lang: en
redirect_from:
  - /rancher/rancher-compose/environment-interpolation/
  - /rancher/latest/en/cattle/rancher-compose/environment-interpolation/
---

## Environment Interpolation
---

Using Rancher Compose, environment variables from the machine running Rancher Compose can be used within the `docker-compose.yml` and `rancher-compose.yml` files. This is only supported in Rancher Compose commands and not in the Rancher UI.  

### How to use it

With the `docker-compose.yml` and `rancher-compose.yml` files, you can reference the environment variables on your machine. If there are no environment variables on the machine, it will replace the variable with a blank string. Rancher Compose will provide a warning on which environment variables are not set.  If using environment variables for image tags, please note that Rancher Compose will not strip the `:` from the image to fetch the latest image. Since the image name, i.e. `<imagename>:` is an invalid image name, no container will be deployed. It's up to the user to ensure that all environment variables are present and valid on the machine.

#### Example

On our machine running Rancher Compose, we have an environment variable, `IMAGE_TAG=14.04`.

```bash
# Image tag is set as environment variable
$ env | grep IMAGE
IMAGE_TAG=14.04
# Run Rancher Compose
$ rancher-compose up
```

**Example `docker-compose.yml`**

```yaml
ubuntu:
  tty: true
  image: ubuntu:$IMAGE_TAG
  stdin_open: true
```

<br>

In Rancher, an `ubuntu` service will be deployed with an `ubuntu:14.04` image.

### Environment Interpolation Formats

Rancher Compose supports the same formats as Docker Compose.

```yaml
web:
  # unbracketed name
  image: "$IMAGE"

  # bracketed name
  command: "${COMMAND}"

  # array element
  ports:
  - "${HOST_PORT}:8000"

  # dictionary item value
  labels:
    mylabel: "${LABEL_VALUE}"

  # unset value - this will expand to "host-"
  hostname: "host-${UNSET_VALUE}"

  # escaped interpolation - this will expand to "${ESCAPED}"
  command: "$${ESCAPED}"
```
