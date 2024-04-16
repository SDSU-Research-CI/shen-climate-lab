This folder contains two different pods for JupyterHub. One targeting NVIDIA A100 GPUs and the other NVIDIA L40 GPUs.

## Deploy

### Step 1: Set Context

```
$ kubectl config set-context nautilus --namespace=sdsu-shen-climate-lab
```
### Step 2: Create the Pod

``
kubectl create -f jupyter-volume.yaml -n sdsu-shen-climate-lab
```

```
kubectl get pvc -n sdsu-shen-climate-lab
```


## Access

## Shutdown / Cleanup

