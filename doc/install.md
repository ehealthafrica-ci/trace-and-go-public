# Install with Docker

Install [Docker](https://docs.docker.com/installation/#installation)

### On Mac OS X

- Install [Bower](http://bower.io)
- Install [Homebrew](http://brew.sh)
- Install Docker Compose `$ brew install docker-compose`
- Install [boot2docker](http://boot2docker.io) and run `boot2docker init` from your CLI

### On Ubuntu

- Install Python's PIP and npm/nodejs `$ sudo apt-get install python-pip npm nodejs-legacy`
- Install Bower `$ sudo npm install -g bower`
- Install Docker Compose `$ sudo pip install -U docker-compose`

After you have all the tool dependencies installed for your respective OS, move onto install the app.

* If on Mac OS you need to run `boot2docker up`

## In tab 1

    $ git clone <thisrepo>
    $ cd <thisrepo>

Make your `/amsel/local_settings.py` file with at least `DEBUG=True` in it

Install Javascript dependencies:

    $ bower install

## In tab 2 start the db
NOTE: when this session is stopped, the data will be lose.

    $ sudo docker-compose up db

## In tab 3 start the celery worker
    $ sudo docker-compose up rabbitmq celery

## In tab 1 again start the webserver

    $ sudo docker-compose run web bash

*On some systems (MacOS for instance) you need to press enter after this command to actually be dropped into the docker shell*

Now within the docker container as root

    # createdb -h localhost -U postgres tag
    # /opt/tag/manage.py migrate
    # /opt/tag/manage.py createsuperuser
    # /opt/tag/manage.py loaddata /opt/tag/core/fixtures/groups.json
    # /opt/tag/manage.py runserver 0.0.0.0:8090
    
*If you're running this in a VirtualBox (MacOS), you will need to access this via the local IP of your Docker instance. This is specified when you first run `boot2docker start` and looks something like `192.168.59.103:2376` but be sure to change that port from `2376` to the one your python server is running at, in this case `8090`*


## Updating & New Dependencies

If new dependencies are added to the app, you need to do one of two things to install them:

- Stop your server, exit web bash, and then run `boot2docker poweroff`then restart with `up` argument
- From within web bash, run `pip install -r requirements.txt`
