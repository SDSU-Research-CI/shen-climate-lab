This folder contains two different pods for JupyterHub. One targeting NVIDIA A100 GPUs and the other NVIDIA L40 GPUs.

## Deploy

### Step 1: Set Context

If you wish to not pass the namespace name for each command, run the following to set the namespace context:

```
kubectl config set-context nautilus --namespace=sdsu-shen-climate-lab
```

Note: All the example below still include the `-n NAMESPACE` for reference. 

### Step 2: Create Personal Volume/PVC

IMPORTANT: Create a copy of the jupyter-volume.yaml file and modify the "jupyter-volume-{change name}" name in the file (line 5) replacing "username" with your username. If you don't do this, you will not have your own volume/PVC where your files are stored.

```
kubectl create -f jupyter-volume.yaml -n sdsu-shen-climate-lab
```

Run the following to see your new volume/PVC:

```
kubectl get pvc -n sdsu-shen-climate-lab
```

### Step 3: Create the Pod

IMPORTANT: Create a copy of the jupyter-pod-L40.yaml file and modify the "jupyterpod-{change-username}" name in the file (line 5) and "jupyter-volume-{change-name}" (line 62) replacing "username" with your username. If you don't do this, you may conflict with someone else using the default name.

Create pod:

```
kubectl create -f jupyter-pod-L40.yaml -n sdsu-shen-climate-lab
```

## Access

Get pod and ensure it's running:

```
kubectl get pods -n sdsu-shen-climate-lab
```

`Note: Replace "username" with your username in the example below.`

Look for the pod with your username in it. Once running, collect your pod's logs:

```
kubectl logs jupyterpod-username -n sdsu-shen-climate-lab
```

From the output, copy the URL to the clipboard that starts with http://127.0.0.1./. 

In a second terminal window, run the following command to set up port forwarding between your computer and the container running Jupyter.

```
kubectl port-forward jupyterpod-username -n sdsu-shen-climate-lab 8888:8888
```

*Note*: If you get an error message indicating port 8888 is already taken, simply change the port number on the left side of the colon in the above command i.e. `8889:8888`.
Then when you paste the URL from the output, make sure to update the port from 8888 to 8889, or whatever number you chose.
This issue can arise if you have a local instance of Jupyter Lab already running on port 8888.

You should now be able to access the URL you copied to the clipboard in your web browser.

## Shutdown / Cleanup

When done, delete your pod:

```
kubectl delete -f jupyter-pod-L40.yaml -n sdsu-shen-climate-lab
```

`Note: Your volume will persist so you can start the pod again and have acecss to your data.`
