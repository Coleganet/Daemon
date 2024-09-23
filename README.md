# Daemon
Create on Rancher Desktop witch a Docker-Desktop Daemon 

 INSTRUCTIONS 
 
1.- Deactivate traefik in rancher desktop and the spining .
2.- Create docker compose file OR Download from HERE
3.- copy and paste the next

```
  traefik:
    image: "traefik:v3.1"
    container_name: "traefik"
    command:
      #- "--log.level=DEBUG"
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entryPoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080"
    extra_hosts:
      - "host.docker.internal:host-gateway"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"

  whoami:
    image: "traefik/whoami"
    container_name: "simple-service"
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.whoami.rule=Host(`whoami.localhost`)"
      - "traefik.http.routers.whoami.entrypoints=web"
	  
```
done now you just start with 

docker compose up -d

The trick is you have this Lines

```
 extra_hosts:
      - "host.docker.internal:host-gateway"
```

Us root look in C:\Windows\System32\drivers\etc
and edit the hosts file , (Make a copy of the file in case before anything)
Add  them the next when the IP is the IP of your Rancher Desktop . Just check your Ip before anything 

 Added Docker Desktop TO Windows Host
 
172.20.0.3 host.docker.internal
172.20.0.3 gateway.docker.internal

save and exit from Rancher Desktop / Reboot Windows 
Now Traefik is indeed working again just go to http://localhost:8080/dashboard/#/ and the best the daemon of docker desktop is exposed 
Not need for any ports setup or any modifications that work perfect in any port or ip your external app or service will get Rancher Desktop
