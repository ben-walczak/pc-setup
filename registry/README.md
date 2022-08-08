https://docs.docker.com/registry/deploying/


```bash
docker run -d -p 5000:5000 --name dockerregistry registry:latest
docker ps # shows registry is running
curl -X GET http://<ip-address>:5000/v2/_catalog # shows that there's no repositories
docker run -d -p 5000:5000 --restart-always --name registry -v $(pwd)/docker-registry/var/lib/registry registry:latest
nano /etc/docker/daemon.json
```

```json
{
    "insecure-registries": ["<ip-address>:5000"],
    "experimental" : false
}
```

```bash
systemctl restart docker
docker push <ip-address>:5000/nginx # specifying ip address and port will push the image to that repository/registry location
```