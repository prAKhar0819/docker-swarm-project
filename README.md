Explanation ➖

There are 4 containers - haproxy, nginx1, nginx2, varnish 

Client request to http://<ec2-ip>

Request on host port 80 sent to haproxy, which receives the req on port 80 of docker container (haproxy).

Haproxy works as a load balancer uses round robin method here and distribute traffic to backend servers that we have provided  in haproxy.cfg in backend section with server templates so that when we scale up it will add those backend.

In haproxy.cfg varnish backend is used just to display varnish backend , no traffic is routed to varnish backend.

Then in nginx conf on port 80 we just apply reverse proxy to varnish container on varnish docker container port 80, then in varnish ..

In varnish req comes from nginx port 80 it check if it has cache then it sent it to user otherwise it goes to backend and fetched data , then serve to user

We use X-backend header to make clearity that which backend has to be triggered.

Further we enable docker - swarm

For docker swarm we make a copied but different version of compose.yml and create docker stack

First we enable docker swarm with command ($ docker swarm init)

Then we create stack using command
($ docker stack deploy --compose-file compose.yml <stack_name>)

 To scale up we use
($ docker swarm scale <service-name>=<no. of replica>)

 Now we can check on http://<ec2 ip>/stats   , haproxy backend will be displayed here.
