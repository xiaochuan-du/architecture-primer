## Registry

``` 
curl localhost:5000/v2/_catalog

curl localhost:5000/v2/pathology-backend-docker/tags/list

# Fetch the manifest identified by name and reference where reference can be a tag or digest. A HEAD request can also be issued to this endpoint to obtain resource information without receiving all data.

curl localhost:5000/v2/pathology-backend-docker_dev/manifests/10

# query digest
curl --head localhost:5000/v2/pathology-backend-docker_dev/manifests/10
```



Dock registry

```ec2-52-80-6-131.cn-north-1.compute.amazonaws.com.cn:5000
ec2-52-80-6-131.cn-north-1.compute.amazonaws.com.cn:5000
```

 

[Docker logging by another container](https://aws.amazon.com/cn/blogs/devops/send-ecs-container-logs-to-cloudwatch-logs-for-centralized-monitoring/)

[Container logging by a driver](https://docs.docker.com/engine/admin/logging/awslogs/)



[docker registry api](https://docs.docker.com/registry/spec/api/#tags)