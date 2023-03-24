# prm-docker-arf-front-end-dojo

A Dojo docker image with tools to build and test node.js projects by ARF team.

Tested and released images are published to dockerhub as nhsdev/prm-docker-arf-front-end-dojo.

## Usage

1. Setup docker.
2. Install [Dojo](https://github.com/kudulab/dojo) binary.
3. Provide a Dojofile at the root of the project:

```
DOJO_DOCKER_IMAGE="nhsdev/node-dojo:<commit>"
```

4. Create and enter the container by running `dojo` at the root of project.

By default, current directory in docker container is `/dojo/work`.

5. Use the output of DOJO_DOCKER_IMAGE in the relevant repo's Dojofile