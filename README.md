# Personal Webpage

Clean and minimalistic personal webpage with page about me and with a blog.

## Technical Details

* Content is generated with [Jekyll][1]
* [Nginx][2] is used as a web server
* All runs in a [Docker][3] container

## TODO List

- [x] install Docker and docker-compose
- [x] get domain

    * https://www.freenom.com/
    * https://www.cloudflare.com/

- [x] run Nginx server with docker-compose file
- [x] install Jekyll theme

    * https://github.com/piharpi/jekyll-klise

- [ ] add "About me" page
- [ ] add blog

## Installation

```bash
docker-compose up
```

## Uninstallation

```bash
docker-compose down
```

[1]: https://jekyllrb.com/
[2]: https://www.nginx.com/
[3]: https://www.docker.com/
