# A Curious Moon Docker Setup
This repo contains the essentials for running PostgresSQL in a Docker container. This allows you to get started with PostgresSQL without installing it directly on your computer.

## Getting Started
In this directory, there is a file called docker-compose.yml. This is a configuration file that tells Docker how to create an environment for you. It boots up the official Postgres Docker image, which is essentially a simple operating system with PostgresSQL installed for you, and not much else.

The configuration file also tells Docker where to store your data, so you won't lose your work. All the data required will be located in this directory.

## Some Lingo
- Docker: Software that runs light weight virtual machines. It allows you to run Linux on Windows or macOS.
- container: A "container" is a running VM.
- image: Sort of like an install disk. A Postgres Image is sort of like an installer for Linux that already has PostgresSQL installed. Multiple images get created when you run Docker. You don't generally have to worry about this part for what we're doing.
- volume: A folder with some persistent data in it. Sorta like a USB drive. If it gets "unplugged" (i.e. your container can't find it because it moved), you won't have the same data you had last time you used your container.


## Commands

### Start Docker
`docker compose up`
This command will start the docker container. You should see some output in the terminal as the system comes online.

### Stop Docker
To stop the container from the terminal, just press `CTL + C` (for cancel).

### Log in to the VM
Once the container is running, you can open a new terminal window to run out next command:

`docker exec -it curious bash`

With this command, you are going to `exec`ute a command inside of the container.

The next part of the command, `-it` is an optional flag that basically tells Docker we want to interact with this container. It keeps the window open so we can keep executing multiple commands.

In the docker-compose.yml file, there is a line that says `container_name: curious`. We've named this container `curious` to make it easier to find. Without that line, Docker will name the container its self, and it might not always be the same name.

In this case, the command we are are running is `bash`, which is a command line shell, like Window's PowerShell; it allows you to run other commands. It is usually the default "shell", and you don't even know you are using it. In this case, we _do_ need to tell Docker to run `bash` so we can run more commands (like `psql`).

**TL;DR:** Run the command above, and you'll be logged into the VM. At this point, it is just like you're running Linux, and you'll be able to browse the file tree and run command in Linux.

### Log out of the VM
To exit the VM, you can just close the terminal window, or press
`CTL + D` for "detach". The container is still running, but you will have to log in again to running more commands.

### Detached Mode
Up until now, you had to keep two terminal windows open: one with the running container, and the other where you are logged into the container.

If you want to work from just one terminal window, you can run your container in "detached mode". This will detach the container's process form the terminal window, and it will run in the background.

`docker compose up -d`

(it's the same as our first command, with the `-d` flag, for "detached")

## Managing Docker Containers
Ok, so now that the container is running in the background...how do you stop it?

You can use the Docker Desktop GUI, and manually view and stop any running containers. OR, we can dtop it from the terminal

`docker container ls` will list out all running containers along with some info about them. Take note of the first column, which it displays the ID of the container.

`docker container kill <container-id>` will stop the container with the matching ID. You don't need to type out the whole ID; the first few characters are all it needs.

## Adding Source Data
Create a directory called `source_data`, and drop the unzipped Curious Moon data in there. This will now be available in your container

## Finding your data
When logged in to the VM, type
`su postgres` to log in as the `postgres` user (rather than `root`).

Then type:

`cd ~/`

This will navigate to your home directory (`~/` is a shortcut to the current user's home directory). The `ls` command should then list two directories: `data` and `source_data`.

### Directories
`data` is where the PostgresSQL server stores all of its internal data. Don't touch this stuff.

`source_data` this should show all the files you've dropped into the `source_data` directory on your computer.
