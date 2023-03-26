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

- [x] add "about" page
- [x] add blog
- [x] add first blog post 

## Deployment

### Local Deployment

1. Install [Jekyll](https://jekyllrb.com/docs/installation/)

    ```bash
    sudo dnf install ruby rubygems ruby-devel gcc gcc-c++ rpm-build
    sudo gem install jekyll bundler
    ````

2. Got to data dir: `cd data`
3. Install dependencies: `bundle`
4. Build and run server: `bundle exec jekyll serve --livereload`

### Remote Deployment

```bash
(cd data ; bundle exec jekyll build)
docker-compose up
```

## Uninstallation

```bash
docker-compose down
```

[1]: https://jekyllrb.com/
[2]: https://www.nginx.com/
[3]: https://www.docker.com/
