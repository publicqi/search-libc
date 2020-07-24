# search-libc

Web wrapper of [libc-database](https://github.com/niklasb/libc-database)

### [demo](https://publicki.top/libc/)

![screenshot](https://raw.githubusercontent.com/blukat29/search-libc/master/screenshot.png)

## Use existing Docker image

    docker pull blukat29/libc
    docker run -p 8080:80 -d blukat29/libc

## Run as Docker container

    git submodule update --init
    cd libc-database
    ./get
    cd ..
    docker build -t libc:latest .
    docker run -p 31337:80 -it libc:latest

## Run in debug mode

    git submodule update --init
    cd libc-database
    ./get
    cd ..
    cd app
    pip install Flask
    python manage.py

## Tips for deploy

I am not good at nginx configuration, so here's how I deployed this application. Before this application, I ran a hexo blog on the server using nginx, and vhost is used.

All I did to adapt this application is to forward `/libc/` path to port 31337. Also, you need to add a rule for `/d` for rendering symbols as texts. [Example](https://publicki.top/d/libc6_2.19-10ubuntu2_i386.symbols)

```
 14         location /libc{
 15                 proxy_pass http://localhost:31337;
 16         }
 
 7          location /d {
 8                  alias /home/www/libc_database/search-libc/libc-database/db;
 9                  location ~ \.symbols$ {
 10                         default_type text/plain;
 11                 }
 12         }
```

And you just have to follow the command under **Run as Docker container** title. 

If you want to change `/libc` to some other paths, remember you also have to change line 35 in [app/search/views.py](https://github.com/publicqi/search-libc/blob/master/app/search/views.py)

