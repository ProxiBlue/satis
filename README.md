# Docker Satis

A docker image and configuration to run Satis very easily in seconds:

* Automatically (cron every minute)
* Manually (http://127.0.0.1:3333/build)
* Admin (http://127.0.0.1/admin)

## Requirements
* docker
* docker-compose

## The default config file for satis looks like this:
```json
{
  "name": "Satis",
  "homepage": "http://satis.lc",
  "repositories": [
    {
      "type": "composer",
      "url": "https://packagist.org"
    }
  ],
  "require": {
      "league/fractal": "^0.15.0",
      ...........
  },
  "require-all":true,
  "require-dependencies":true,
  "require-dev-dependencies":true,
  "minimum-stability":"dev"
}
```

## Satis/Satisfy access
* Home page
[http://127.0.0.1:8181](http://127.0.0.1:8181)

* Manual build / Web hook 
[http://127.0.0.1:3333/build](http://127.0.0.1:3333/build)

* Admin 
[http://127.0.0.1:8181/admin](http://127.0.0.1:8181/admin)

Default credentials are: **admin / passw0rd** as you can see in `config.php`. 
You will also find instructions to change or add credentials in this section of the file.

## Configuration override (if needed)
* Add your own custom `config.json` (aka satis.json)
* Add your own custom `config.php` for Satisfy

```json
satis:
    image: ealebed/satis:1.0
    volumes:
        - ./config.php:/app/config.php
        - ./config.json:/app/config.json
```
But I advise you to create your own image and Dockerfile:
```json
FROM ealebed/satis:1.0
...
COPY config.php /app/config.php
COPY config.json /app/config.json
```

## Build frequency
By default, building script is executed every minute thanks to the docker-compose configuration
```json
satis:
    image: ealebed/satis:1.0
    environment:
        CRONTAB_FREQUENCY: "*/1 * * * *"
```
You can override this value changing the cron configuration: `*/5 * * * * OR */10 * * * *`
Or you can disable cron with: `CRONTAB_FREQUENCY=-1`

## Composer cache
Cache should be shared with the host to be reused when you restart the container, for better performance.
```json
satis:
    image: ealebed/satis:1.0
    volumes:
        - "/var/tmp/composer:/root/.composer"
```

## Ports
If you want to build on port 8888 and access the interface on port 5000:
```json
satis:
    image: ealebed/satis:1.0
    ports:
        - 8888:3000
        - 5000:80
```
