## Tips

Docker images - https://hub.docker.com/
You can add docker as service in Phpstorm
## Commands

```sh
# Start docker container for project
docker compose up -d
# Stop docker container 
docker compose down
# See active docker container
docker ps
# Connect to docker container
docker exec -it <mycontainer> bash
# See all logs (error and access) of web cointainer
docker logs -f <container_name>

```
