---
{"dg-publish":true,"permalink":"/interesting/docker-3/"}
---


If you are faced with sudo issue on docker.
try running;
```sh
docker context ls
```
If you see something like linux-default, just run the next command , you should be able to run docker without sudo now.!
```sh
docker context use default
```

