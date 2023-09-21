docker rm -f $(docker ps -aq)

docker rmi $(docker images -aq)

docker system prune -a

docker volume prune

https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes
