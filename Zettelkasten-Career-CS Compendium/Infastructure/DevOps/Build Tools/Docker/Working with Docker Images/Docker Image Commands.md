---
tags:
  - FORMAT
  - Docker
  - DevOps
---
Certainly\! Here are examples for the **`docker commit`** and **`docker rmi`** commands.

---

# Creating Images Using `docker commit`

The `docker commit` command is used to create a new image from the changes made to a container. It essentially takes a running container and saves the current state (including changes made to files, installed software, etc.) as a new image.

## Steps to Create an Image Using `docker commit`:

1. **Run a container** from an existing image.

2. **Make changes** inside the container (e.g., installing software, modifying files).

3. **Commit the changes** to create a new image.

## Example:

1. **Step 1: Run a Container from an Image** Let's start by running a container from the `ubuntu` image and start an interactive shell session:

```shell
docker run -it ubuntu:latest /bin/bash
```

2.   
   **Step 2: Make Changes Inside the Container** Once inside the container, let's say we install a package like `curl`:

```shell
apt-get update && apt-get install -y curl
```

3.   
   **Step 3: Commit the Changes to Create a New Image** After installing `curl`, exit the container:

```shell
exit
```

4.   
   Now, use `docker commit` to create a new image from the container. First, list your containers to find the container ID of the running container you just exited:

```shell
docker ps -a
```

5.   
   Output might look something like this:

```
CONTAINER ID   IMAGE         COMMAND       CREATED         STATUS                     NAMES
b69e7b7d8e64   ubuntu:latest "/bin/bash"   5 minutes ago   Exited (0) 2 minutes ago    interesting_kirch
```

6.   
   Now, create a new image from the container `b69e7b7d8e64` (replace this with your actual container ID):

```shell
docker commit b69e7b7d8e64 my-custom-ubuntu
```

7.   
   Here:

   * `b69e7b7d8e64` is the container ID.

   * `my-custom-ubuntu` is the name you want to give to the new image.

8. After the command completes, you'll have a new image called `my-custom-ubuntu`, which includes the changes (in this case, `curl` being installed).

9. **Step 4: Verify the New Image** To verify that your new image has been created, run:

```shell
docker images
```

10.   
    You should see your `my-custom-ubuntu` image listed:

```
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
my-custom-ubuntu  latest    a3b1c2d3e4f5   2 seconds ago    200MB
ubuntu            latest    b69e7b7d8e64   5 minutes ago    150MB
```

---

# Removing Unused Images with `docker rmi`

The `docker rmi` command is used to remove one or more Docker images from your local system. This is useful for cleaning up unused or untagged images that are taking up disk space.

## Example:

1. **List Your Docker Images** First, list all the images on your system:

```shell
docker images
```

2.   
   Example output:

```
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
my-custom-ubuntu  latest    a3b1c2d3e4f5   2 seconds ago    200MB
ubuntu            latest    b69e7b7d8e64   5 minutes ago    150MB
node              latest    9876abcd1234   2 days ago       300MB
```

3.   
   **Remove a Specific Image** To remove a specific image, you can use the `docker rmi` command followed by the image name or ID. For example, to remove the `my-custom-ubuntu` image, run:

```shell
docker rmi my-custom-ubuntu
```

4.   
   Alternatively, you can use the image ID:

```shell
docker rmi a3b1c2d3e4f5
```

5.   
   After running this, the image `my-custom-ubuntu` will be removed from your local system. If the image is being used by any containers, you may need to remove the containers first (or use the `-f` flag to force the removal).

6. **Remove Multiple Images at Once** To remove multiple images at once, simply list the image names or IDs:

```shell
docker rmi ubuntu node
```

7.   
   Or use the image IDs:

```shell
docker rmi b69e7b7d8e64 9876abcd1234
```

8.   
   **Force Removal (`-f` flag)** If an image is being used by a container, Docker will prevent you from removing it. To forcefully remove an image (and its associated containers), you can use the `-f` flag:

```shell
docker rmi -f my-custom-ubuntu
```

9.   
   **Prune Unused Images** If you want to remove all unused images that are not associated with any containers, you can use:

```shell
docker image prune
```

10.   
    This will prompt you to confirm that you want to delete all unused images. To automatically confirm, use the `-f` flag:

```shell
docker image prune -f
```

---

# Summary of Commands

| Command | Description | Example |
| ----- | ----- | ----- |
| `docker commit container-id image-name` | Create a new image from a container's changes. | `docker commit b69e7b7d8e64 my-custom-ubuntu` |
| `docker rmi image-name` | Remove an image from the local system. | `docker rmi my-custom-ubuntu` |
| `docker rmi -f image-name` | Force removal of an image, even if it is used by containers. | `docker rmi -f my-custom-ubuntu` |
| `docker image prune` | Remove all unused images (those not associated with any container). | `docker image prune` |

---

These commands help manage Docker images and keep your system clean by removing unused or unnecessary images. Let me know if you have any other questions\!

