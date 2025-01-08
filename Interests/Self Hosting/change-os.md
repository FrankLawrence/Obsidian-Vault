Changing the underlying operating system while keeping your Docker containers intact can be a bit tricky, but it's doable. Here's a step-by-step approach to help you achieve this:

**Backup your data**

Before making any changes, ensure you have a backup of your important data, including the 50GB of photos and videos stored in the immich container. You can use Docker's built-in `docker container save` and `docker container load` commands to export and import the container's data.

**Export Docker containers and images**

1. Stop all running containers: `docker stop $(docker ps -aq)`
2. Export the Docker containers and images: `docker save -o docker_images.tar $(docker images -q)`
3. Export the Docker volumes (if any): `docker run --rm -v /var/lib/docker/volumes:/volumes -v $(pwd):/backup ubuntu tar -czf /backup/volumes.tar.gz /volumes`

**Prepare for the OS change**

1. Create a bootable USB drive with the new OS (Debian or NixOS) and boot from it.
2. Install the new OS, making sure to preserve the existing disk layout and partitions. You can choose to install the new OS alongside Ubuntu Server or replace it entirely.

**Restore Docker containers and images**

1. Install Docker on the new OS.
2. Restore the Docker containers and images: `docker load -i docker_images.tar`
3. Restore the Docker volumes (if any): `docker run --rm -v /var/lib/docker/volumes:/volumes -v $(pwd):/backup ubuntu tar -xzf /backup/volumes.tar.gz -C /volumes`

**Verify and update**

1. Start the Docker containers: `docker start $(docker ps -aq)`
2. Verify that the containers are running correctly and the data is intact.
3. Update the Docker images to ensure you have the latest versions: `docker pull $(docker images -q)`

**Additional considerations**

* If you're switching to NixOS, be aware that it uses a different package manager (Nix) and configuration system. You may need to adapt your Docker configuration and dependencies accordingly.
* If you're using any OS-specific dependencies or configurations, you may need to update them to work with the new OS.

By following these steps, you should be able to safely change your underlying OS while preserving your Docker containers and data.
