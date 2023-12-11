# Dockerfile from image

**This project is only at the proof-of-concept state yet**

I would happily add features to this project, feel free to create an issue
for any suggestion.

## How to build and run

The goal of this project is to easily generate a Dockerfile from an existing
Docker image.

To build the image from source (this step is not mandatory as the image also
is on the Docker Hub):

    git clone https://github.com/liudonghua123/dockerfile-from-image.git
    cd dockerfile-from-image
    docker build -t liudonghua123/dockerfile-from-image .

To get a Dockerfile from an existing image:

```
docker run --rm -v '/var/run/docker.sock:/var/run/docker.sock' liudonghua123/dockerfile-from-image <IMAGE_ID_OR_NAME>
```

## Example usage

```
[root@localhost dockerfile-from-image]# docker run --rm -v '/var/run/docker.sock:/var/run/docker.sock' liudonghua123/dockerfile-from-image liudonghua123/dockerfile-from-image
FROM liudonghua123/dockerfile-from-image:latest
ADD file:1f4eb46669b5b6275af19eb7471a6899a61c276aa7d925b8ae99310b14b75b92 in /
CMD ["/bin/sh"]
RUN apk add --update python3 wget      \
    && wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py      \
    && python3 get-pip.py --break-system-packages      \
    && apk del wget      \
    && pip3 install --break-system-packages -U docker      \
    && rm -f get-pip.py      \
    && yes | pip3 uninstall --break-system-packages pip
COPY entrypoint.py /root
ENTRYPOINT ["/root/entrypoint.py"]
```

## LICENSE

MIT License

Copyright (c) 2023 liudonghua