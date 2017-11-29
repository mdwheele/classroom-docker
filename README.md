# Dockerized GitHub Classroom

This repository is meant to be a Docker-based development environment for GitHub Classroom.

## Manual

```bash
# Clone and change directories to GitHub Classroom app
git clone git@github.com:github/classroom
cd classroom

# Clone Docker configs to a directory called `.docker`
git clone git@github.com:mdwheele/classroom-docker .docker

# Create a repo-specific ignore rule for `.docker`
# We do this to not have to commit "/.docker" to the upstream
# project's .gitignore file.
echo ".docker" >> .git/info/exclude

# You'll use the `classroom` shell to interact with the Docker 
# environment
./classroom
# > Usage: ./classroom {build|up|down|shell|clean}

# On the first run, build the `workspace` image we'll work from.
# The workspace is based on the version of Ruby supported by Classroom.
# It installs things like NodeJS, bundler, vim, etc.
# 
# This does NOT run script/setup or script/server. That is YOUR job after 
# all containers have started. You'll need a shell...
./classroom build

# To get a shell to the `workspace` container...
./classroom shell

# The `shell` command starts all the containers (runs `up`) and starts bash.
# You'll be dropped in `/usr/src/app` where the project is bind-mounted to
# by Docker. In this way, you can make changes to the project on your 
# host and they will be automatically available in the container. Be sure
# to pay attention to where you're running commands. 

# Go through the normal setup steps for Classroom...

# Configure the environment
cp .env.example .env
vi .env 

# Run setup
script/setup

# Run the Rails server
script/server

# When you're done, you can exit and stop all containers.

# Leave the bash session on the workspace container...
exit 

# Stop all containers...
./classroom down

# Alternatively, if you're done and want containers removed...
./classroom clean

# At this point I would also clean up unused images and volumes...
docker system prune -a
```