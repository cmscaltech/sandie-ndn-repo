## Dockerfile SANDIE-NDN Repo Packager Build Machine

This [Dockerfile](Dockerfile) creates Docker containers used for building and updating the NDN software stack RPMs delivered by the [SANDIE](https://github.com/cmscaltech/sandie-ndn) project.

### Build Docker image from Dockerfile

```console
user@cms:~$ docker build . --no-cache -t sandiendnrepo:1.0.0  --network=host
```

### Run Docker container from image

```console
user@cms:~$ docker run -d -it --name=sandiendnrepo -it --cap-add=NET_ADMIN --net=host sandiendnrepo:1.0.0
```
