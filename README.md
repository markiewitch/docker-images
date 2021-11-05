# 🗺️️ Base Sylius Images

It is repository with base images of node & php

## Requirements
- [docker](https://www.docker.com/)

```yaml
    build-and-publish-php-image:
        strategy:
            matrix:
                php: [ "7.4-fpm", "8.0-fpm" ]
        runs-on: ubuntu-20.04
        name: "Build & publish sylius base php docker image"
        steps:
            -   uses: actions/checkout@v2
            -   name: Set up Docker Buildx
                id: buildx
                uses: docker/setup-buildx-action@v1
            -   name: Log in to the Container registry
                uses: docker/login-action@v1
                with:
                    registry: ${{ env.REGISTRY }}
                    username: ${{ github.actor }}
                    password: ${{ secrets.GITHUB_TOKEN }}
            -   name: Extract metadata (tags, labels) for Docker
                id: meta
                uses: docker/metadata-action@v3
                with:
                    images: ${{ env.REGISTRY }}/${{ env.PHP_IMAGE_NAME }}
                    tags: |-
                        type=sha,enable=true,priority=100,prefix=${{ matrix.php }}-sha-,suffix=,format=short
                        type=ref,event=branch,prefix=${{ matrix.php }}
                        type=semver,prefix=${{ matrix.php }}

            -   name: Build and push Docker php image
                uses: docker/build-push-action@v2
                with:
                    context: php/
                    push: false
                    tags: ${{ steps.meta.outputs.tags }}
                    labels: ${{ steps.meta.outputs.labels }}
                    build-args: |
                        PHP_BASE_IMAGE_TAG=${{ matrix.php }}
                    cache-from: type=gha
                    cache-to: type=gha,mode=max

    build-and-publish-node-image:
        strategy:
            matrix:
                node: [ "12-slim" ]
        runs-on: ubuntu-20.04
        name: "Build & publish sylius base node docker image"
        steps:
            -   uses: actions/checkout@v2
            -   name: Set up Docker Buildx
                id: buildx
                uses: docker/setup-buildx-action@v1
            -   name: Log in to the Container registry
                uses: docker/login-action@v1
                with:
                    registry: ${{ env.REGISTRY }}
                    username: ${{ github.actor }}
                    password: ${{ secrets.GITHUB_TOKEN }}
            -   name: Extract metadata (tags, labels) for Docker
                id: meta
                uses: docker/metadata-action@v3
                with:
                    images: ${{ env.REGISTRY }}/${{ env.PHP_IMAGE_NAME }}
                    tags: |-
                        type=sha,enable=true,priority=100,prefix=${{ matrix.node }}-sha-,suffix=,format=short
                        type=ref,event=branch,prefix=${{ matrix.node }}
                        type=semver,prefix=${{ matrix.node }}

            -   name: Build and push Docker php image
                uses: docker/build-push-action@v2
                with:
                    context: node/
                    push: false
                    tags: ${{ steps.meta.outputs.tags }}
                    labels: ${{ steps.meta.outputs.labels }}
                    build-args: |
                        NODE_BASE_IMAGE_TAG=${{ matrix.node }}
                    cache-from: type=gha
                    cache-to: type=gha,mode=max
```

If you have any questions please do not hesitate to contact me [Arminek](https://github.com/Arminek)
