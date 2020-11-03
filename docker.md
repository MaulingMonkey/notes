```cmd
# Setup
docker build -t image-name .                                # Creates      "image-name"
docker run -d -p 8000:80 --name container-name image-name   # Creates/runs "container-name"

# Query
docker images                                               # lists "image-name"
docker image list                                           # lists "image-name"
docker container list                                       # lists "container-name"

# Cleanup
docker stop container-name                                  # Stops "container-name"
docker container rm "container-name"                        # Removes "container-name"
docker           rm "container-name"                        # Removes "container-name"
docker image rm  "image-name"                               # Removes "image-name"
docker       rmi "image-name"                               # Removes "image-name"
```

https://github.com/docker/getting-started
