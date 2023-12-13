# laravel-sail-mongo-poc1

Probe of concept 1 of laravel and mongo with sail updated - 2024

Develop and training propose works with laravel, sail, mongodb and mongo-express in codespaces (configured) or localy 

## Steps Using this repository (codespaces or in your local environment)

1.- Copy the file .env.example to .env, edit it, making customization data, and backup it in a safe place.  

2.- Execute the next command to run/download containers and install laravel site

`
docker run --rm -u "$(id -u):$(id -g)" -v "$(pwd):/var/www/html"  -w /var/www/html laravelsail/php82-composer:latest composer install --ignore-platform-reqs
`

3.- Execute the next command to go up all the containers

`./vendor/bin/sail up -d`

> NOTE: with this you can go to the port tab in the terminal tab, in order to click the "open in browser" 80 port URL to verify that laravel is running.
* You can verfy all the server working in the containers.
* You could disabled debug mode in the .env file.
* You could edit and backup the .env file.
* Migration has to be run inside of the container
* The running container are the ones are in the docker-compose-yml (Some other are commented like: mysql, redis, meilisearch, mailpit, selenium)
* You could stop the contaner with ./vendor/bin/sail down

4.- Ready for work with laravel

> You could access with the .env credentials (adminer.org file included)

## You could see:

GitHub Codespaces Laravel. Install and first steps

https://www.youtube.com/watch?v=hPDASavmNeo&ab_channel=JavierTer%C3%A1nGonz%C3%A1lez

https://laravel.com/docs/9.x/installation/#getting-started-on-linux

## TROUBLESHOOTINGS:

If accidentally you created a mysql volume by running sail without an .env file, which was persistent the whole time thus of course having no user and database configured.

I executed ./vendor/bin/sail down --rmi all -v to remove all images and volumes and then just ran ./vendor/bin/sail up and it created the images and volumes from scratch. 
Now everything worked out and I can migrate my data.
./vendor/bin/sail down --rmi all -v

Or, complete rebuild all the docker containers:
./vendor/bin/sail build --no-cache

Problems with docker:

#(master) $ docker ps

Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

my solution:

1.- I killed dockerd process:

`#(master) $ ps -aux | grep dockerd`

`#(master) $ kill -9 DOCKERD_PROCESS_ID`

`#(master) $ sudo dockerd &`

Run dockerd successfully!

## Laravel URL and mempry problems:

I suggest to use this in web.php 


```
ini_set('memory_limit', -1);
$url = config('app.url');
URL::forceRootUrl($url);
if (App::environment('production')) {  
    URL::forceScheme('https');  
}
 ```

##

I disabled debug mode in the .env

NOTE: Migrations has to be run inside of the container
        
php8.1-redis php8.1-swoole php8.1-xdebug php8.1-mongodb \