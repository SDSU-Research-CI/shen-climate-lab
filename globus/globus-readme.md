# Deploy Globus Connect

## One Time Steps

### 1. Create the PVC to hold the data

Create the PVC:

```
kubectl create -f globus-connect-volume.yaml -n sdsu-shen-climate-lab
```

Check status, waiting for STATUS to be "Bound":

```
kubectl get pvc -n sdsu-shen-climate-lab
```

### 2. Deploy the Globus Connect K8S service

```
kubectl create -f globus-connect-service.yaml -n sdsu-shen-climate-lab
```

### 3. Deploy Globus Connect Pod

```
kubectl create -f globus-connect.yaml -n sdsu-shen-climate-lab
```
Check the pod status, waiting for Status of "Running":

```
kubectl get pods -n sdsu-shen-climate-lab
```

### 4. Connect to the pod's terminal

```
kubectl exec -it globus-connect-0 -n sdsu-shen-climate-lab -- bash
```

Note: the /data volume is mounted with 5.5 TB of space.

## Configure Globus

### 1. Become a gridftp user

```
su - gridftp
```

### 2. Login to your Globus account

```
globus login --no-local-server
```
Copy the URL and log in to Globus on the web then paste the authorization code back into the terminal.

### 3. Create a Globus Endpoint

Use a unique name for the endpoint, in the command below it is "my-fcn-data".

```
globus endpoint create --personal my-fcn-data | tee endpoint-inf
```

```
export ep=<id from output>
export epkey=<key from output>
```

### 4. Create Endpoint with a setup key

```
cd globusconnectpersonal-3.2.2/
./globusconnectpersonal -setup $epkey
```

### 5. Verify Endpoint is configured

```
globus endpoint search --filter-scope my-endpoints
```

### 6. Start Globus connect personal

```
./globusconnectpersonal -start &
```

## On-Going / Update Data 

If you wish to stop the pod running Globus, first check the statefulset status:

```
kubectl get statefulset -n your_namespace
```

If still deployed, decrease replicas to 0:

```
kubectl scale statefulset globus-connect --replicas=0 -n sdsu-shen-climate-lab
```

If you wish to start it up again:

```
kubectl scale statefulset globus-connect --replicas=1 -n sdsu-shen-climate-lab
```

If the statefulset is no longer deployed, create the statefulset:

```
kubectl create -f globus-connect.yaml -n sdsu-shen-climate-lab
```