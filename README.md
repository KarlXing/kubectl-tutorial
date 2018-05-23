# kubectl-tutorial
A brief tutorial on using kubectl tool to run applications on kubernetes clusters.

## Set up to start using kubectl on CHASE-CI clusters:

1. Install kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/. To know more about it, the documentation is available [here](https://kubernetes.io/docs/reference/kubectl/overview/).
2. Gain access to CHASE-CI cluster. Go to http://nautilus.optiputer.net
   - Click on login (upper right corner)
   - Choose UCI (or your institution) from the list and use your credentials. Now you should have user access.
   - Click the "Get Config" link on top right and get your config file.
   - Put the file to your $home/.kube directory.
   - (option 1) Register at https://rocket.nautilus.optiputer.net/ chat app and request the owners there to give you admin access to create a namespace. Recommended as the owners are active on the chat and help you fast, even if you don't need admin access.
   - (option 2) If someone from your lab already has admin access (like me), ask to add as a user to his/her namespaces(s).
3. If you use a different kubernetes cluster, contact its admin on creating namespaces and gaining access.
  
## Using kubectl:

You can use kubectl from a terminal (or cmd on Windows).

- To and check the pods (container or group of containers) currently running:

   ```kubectl get pods -n <namespace>```

- To create a pod:

   ```kubectl create -f /path/to/*.yaml -n <namespace>```
   
   Or, you can also get the .yaml parameter file using a web address.
   
   ```kubectl create -f https://raw.githubusercontent.com/dimm0/prp_k8s_config/master/tensorflow-example.yaml -n <namespace>```
   
   If you run ```kubectl get pods -n <namespace>``` again, you should see the STATUS changing from 'ContainerCreating' to 'Running'.

- To delete a pod:
   ```kubectl delete -f https://raw.githubusercontent.com/dimm0/prp_k8s_config/master/tensorflow-example.yaml -n <namespace>```
  
  Or specify the appropriate path to the .yaml file. 
  ## Alway delete your pod after use.

- To open the pod terminal:

   ```kubectl exec -it <pod-name> -n <namespace> bash```
   
   The ```<pod-name> ``` is the 'name' parameter of .yaml file.
   
- If you used the .yaml file above, it comes with a docker with TensorFlow and all dependencies
  installed. It also has jupyterlab pre-installed, so you can tunnel jupyter notebooks 
  remotely. The pod (let's use this term hereafter, instead of docker container) also has 
  three ipythin notebooks.

  ### Let's try the mnist example:
  
  - Check if jupyterlab is installed. If not, install it.
    
    ```pip install jupyterlab```
  
  - Then enter the following command to start a remote tunnel.
  
      ``` jupyter lab --allow-root --port=<any port number under 64K and more than 2K> --ip=127.0.0.1```
      It will create a URL to access the jupyter instance. Copy it.
      
  - On a new terminal window, enter this:
  
      ```kubectl port-forward gpu-pod-example <same-port-number> -n <namespace>```
      
      It will start the port forwarding.
      
  - Open a browser and paste the URL copied from before and enter. Now you have access to
    all the jupyter notecbooks on the pod.
    
  - Try the stepwise MNIST example.
      




