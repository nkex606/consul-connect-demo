# consul-connect-demo

This project demonstrate Connect feature of Consul by Hashicrop.


## consul server

The consul cluster should has at least three servers for better performance and fault tolerance, but I use only one single server for this demo.

Connect should be enabled for your consul cluster. By default, Connect is enabled when running consul in dev mode. I use the same configuration from hashicorp github which you can find [here](https://github.com/hashicorp/consul-demo-traffic-splitting/blob/master/consul_config/consul.hcl).

## consul proxy

Service communacation should through sidecar proxies. I use the docker image from nicholasjackson's consul envoy which you can find in [docker hub](https://hub.docker.com/r/nicholasjackson/consul-envoy).

The image should pass three environment variables, `CONSUL_HTTP_ADDR` telling the proxy address and port number of the consul server, since consul communacates with proxies via gRPC, `CONSUL_GRPC_ADDR` is use for specify the gRPC address and port. Finally, `SERVICE_CONFIG` tells the proxy which json config should be use for registering service.

## services

All services are written in Golang with Echo framework.

All services except `food`, should listen on localhost for security, as mentioned before, communication between services should through consul proxies. But in this demonstration, address is listen on `0.0.0.0` for convenience.

For `food` service, `PRICE_URL` and `DESC_URL` are passed as environment variables, the address **http://localhsot:9090** and **http://localhost:9091** tells the food app calling address for `price` and `desc` services, respectively.

     
For `price` and `desc` services, just pass `LISTEN_HOST` as environment variables.

    You can see the configuration files in service_config/xxx.json
