# FreeOrion docker-compose

Docker Compose and Dockerfile to get [FreeOrion](https://freeorion.org/) running in a container.

I have not used this in a long time. I don't know if it still works.

## Install

Download and install docker-compose and [docker](https://www.docker.com/get-started).

## Clone Repository

```
git clone github.com/magnusviri/dockerfiles.git
cd dockerfiles/freeorion
```

Edit the docker-compose.yml file and change TZ to your timezone.

## Start

```
docker-compose up -d
```

Save files will be located in "saves"

## License

MIT License
