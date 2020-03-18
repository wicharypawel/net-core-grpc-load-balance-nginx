# gRPC Nginx load balancer hosted in Docker

__NOTE: Run commands in root directory__

## Build images
```
docker build -t grpc-reverseproxy-client:latest -f .\NetCoreGrpc.ReverseProxyLoadBalancer.ConsoleClientApp\Dockerfile .
docker build -t grpc-reverseproxy-lb-nginx:latest -f .\NetCoreGrpc.ReverseProxyLoadBalancer.Nginx\Dockerfile .\NetCoreGrpc.ReverseProxyLoadBalancer.Nginx
docker build -t grpc-reverseproxy-server:latest -f .\NetCoreGrpc.ReverseProxyLoadBalancer.AspNetCoreServerApp\Dockerfile .
```

## Create resources in K8s
```
docker network create --subnet=172.18.0.0/16 grpc-custom-network
docker run -d --net grpc-custom-network --name grpc-reverseproxy-server-1 --ip 172.18.0.22 grpc-reverseproxy-server
docker run -d --net grpc-custom-network --name grpc-reverseproxy-server-2 --ip 172.18.0.23 grpc-reverseproxy-server
docker run -d --net grpc-custom-network --name grpc-reverseproxy-server-3 --ip 172.18.0.24 grpc-reverseproxy-server
docker run -d --net grpc-custom-network --name grpc-reverseproxy-lb-nginx grpc-reverseproxy-lb-nginx
docker run -d --net grpc-custom-network --name grpc-reverseproxy-client grpc-reverseproxy-client
```

## Verify connection
```
docker logs grpc-reverseproxy-client
```

## Verify network
```
docker network inspect grpc-custom-network
```

## Tear down resources
```
docker rm grpc-reverseproxy-client -f
docker rm grpc-reverseproxy-lb-nginx -f
docker rm grpc-reverseproxy-server-1 -f
docker rm grpc-reverseproxy-server-2 -f
docker rm grpc-reverseproxy-server-3 -f
docker network rm grpc-custom-network
```
