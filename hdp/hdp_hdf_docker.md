# Run HDP/HDF with docker

## HDP

Download script

Download docker image

```
cd /path/to/script
sh docker-deploy-{HDPversion}.sh
```

Start HDP docker

```
docker start sandbox-hdp
docker start sandbox-proxy
```

Remoe HDP docker

```
docker stop sandbox-hdp
docker stop sandbox-proxy
docker rm sandbox-hdp
docker rm sandbox-proxy
```

If want to remove docker Images

```
docker rmi hortonworks/sandbox-hdp:{release}
```
