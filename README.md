# Personal Webpage

Basic personal webpage for self presentation and software projects.

Run in a [Docker][1] container on [nginx][2] server.

[1]: https://docs.docker.com/
[2]: http://nginx.org/en/docs/

## TODO List

[x] install Docker and docker-compose
[x] run nginx in a container
[ ] create docker-compose file
[ ] add server config files
[ ] add hello world page
[ ] get domain (vopat.dev)
[ ] add https
[ ] find suitable webpage template
[ ] add context

## Installation

```bash
docker container run --name webserver --rm -p 8000:80 nginx
```