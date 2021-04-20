## Commands and Arguments in Docker
* `docker run ubuntu` - terminates immdiately because no process is running
* `docker run ubuntu [executable] [parameter]` - runs the executable and passes in the specified parameter
* This dockerfile loads container and sleeps for 10 seconds, to override you can say docker run [image name] sleep 5
```
FROM ubuntu
CMD ["sleep","10"]
```
* This dockerfile loads container and sleeps for 10 seconds too. 
```
FROM ubuntu
ENTRYPOINT ["sleep", "10"] 
OR
ENTRYPOINT ["sleep"]
CMD ["10"] #default value
OR
ENTRYPOINT ["sleep"]
```
To run you can do
  * docker run [imagename]
  * docker run [imagename] 20
  * docker run [imagename] 10
  * To override the executable itself docker run [imagename] --entrypoint [executable] [command]
* Specifying commands in  
```yaml
apiVersion: v1
kind: Pod
metadata:
 name: ubuntu-sleeper
spec:
 containers:
  - name: ubuntu-sleeper
    image: ubuntu-sleeper
    command: executablename # override entrypoint instruction file
    args: ["10"] #overrides command instruction in docker file
```
## Pod
`kubectl run --image [image name] [name of pod]` - imperative creation of pod
## Security
* `docker run --user 1001 ubuntu` - run container as user with id 1001
* `docker run --cap-add MAC_Admin ubuntu` - add capability MAC_Admin to container run time
* `docker run --cap-drop MAC-Admin ubuntu` - remove capability
* `docker run --privillaged ubuntu` - add all privilleges

`securityContext` set in a container is always overridden by securityContext set in the pod. You can only add capabilities to a securityContext in a container not a pod.

## VI editor
* `/xxxxx` allows you to search for string xxxxx
* Press n to go to next occurance

## grep
* `kubectl explain pod --recursive | grep -A5 tolerations` - -A5 prints 5 lines after the word tolerations

## Service Account
* kubectl create sa [name]
* every pod will automatically use the default service account. This is mounted as a volume
* You will find the token as a secret for the service account
* You can create a new service account and use that by specifying `serviceAccount` in a deployment
  
## Taints and Tolerations
* You taint a node and add tolerations to pod
* Taint - `kubectl taint nodes [node name] key=value:NoSchedule`
  * NoSchedule - does not schedule any new pods
  * NoExecute - deletes any executing pods and will not create any new pods 
* Tolerations:
  ```
  tolerations:
  key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoExecute"
  tolerationSeconds: 3600
  ``` 