By default, our projects cant connect to each other, because they have different network bridges. Here is example of how to fix it for mokejimai_app and eshop_app projects:
#### 1. Check if the containers are on the same network
Run the following command to check which network both containers are using:
```sh
docker network ls
```
![[Pasted image 20250220101712.png]]

Then, inspect the network details:
```sh
docker network inspect <NETWORK_NAME>
```
It shows which containers the network manages:
![[Pasted image 20250220102307.png]]
If they are not in the same network, you may need to connect them
#### 2. Connect containers to networks
Use following commands:
```sh
docker network connect <NETWORK_NAME> eshop_app
docker network connect <NETWORK_NAME> mokejimai_app
```
![[Pasted image 20250220102452.png]]
You can inspect it, to confirm changes:
![[Pasted image 20250220102552.png]]
![[Pasted image 20250220102600.png]]
#### 3. Test connection between containers using cURL
Use cURL command to test connection:
```sh
docker exec -it eshop_app sh -c "curl -I http://mokejimai_app"
```
If response is 200, connection is established, great success!
![[Pasted image 20250220102743.png]]

![[XJyemeI.jpeg]]'