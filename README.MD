# dockerfiles

Well, maybe not Dockerfiles. This is a collection of Docker related files I use on my servers to deploy various services. (you won't find a description of my whole infra here for obvious reasons)  
Note: everything here is my personal opinion - you might not agree with it. Nothing stops you from forking the repo and adjusting every single thing for your needs.

## Key concepts

1. Easy repo updates - all ``docker-compose.yml`` files actually used for deploying services are ignored by git so it doesn't scream when you try to pull or something. Only template files will be updated. You can put the secrets right into ``docker-compose.yml``.
2. One service is independent from another - of course there are some exceptions, for example everything exposing anything over HTTP depends on a reverse proxy in my case. The next concept is an exception too. The main aspect is to just not have everything in one docker stack. Can you imagine the nightmare of redeploying one service and having to take everything down? I can, because I used to have my environment structured like this (thankfully only on one server).
3. Easy backups - bind mounts allow the user to just package this repo in a zip or another format, then transfer this one file to their PC or somewhere else. This makes any re-deploys, host reinstalls, re-anything usually faster.
4. Templates are provided in a "barebones" state. Modify them as you want. Add additional networks, volumes, DB containers, whatever you need. There's always a link on top of the template where you can find more info about the thing you're about to deploy.

## What you might want to stick to

1. Using bind mounts instead of volumes.
2. Creating external networks, if they're going to be shared across multiple stacks. (created with ``docker network create network-name``)
3. Using ``expose`` instead of ``ports`` when possible, for example web UI's exposed with a reverse proxy. ``expose`` is usually not necessary; it's left in templates as a hint what a service exposes internally to the other ones. ``ports`` should only be used for things that other hosts on the network need to access.
5. Deploying all services with a ``TZ`` environment variable to have correct timezone inside containers.
6. Using official images, if available. If there's no official image for a service you want to run, consider using linuxserver.io images.

## Usage

(i don't like spoonfeeding people with commands, if you want to use this I expect you to have some knowledge about Docker and docker-compose)

1. Clone this repo.
2. ``cd`` to the directory of service you want to deploy.
3. Copy ``docker-compose.template.yml`` to ``docker-compose.yml``.
4. Adjust ``docker-compose.yml`` to your needs with your favourite text editor.
5. Deploy with ``docker-compose up -d``

