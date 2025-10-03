# Docker

Same for podman just replace `docker` with `podman`

## Stop all running containers
docker stop $(docker ps -aq)

## Remove all containers
docker rm $(docker ps -aq)

## Remove all Docker volumes
docker volume rm $(docker volume ls -q)

## (Optional) Remove all Docker images
docker rmi $(docker images -q)

## Alternatively, to remove all unused Docker data including volumes
docker system prune --volumes