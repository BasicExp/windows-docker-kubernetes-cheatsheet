# Windows 10 Pro - Docker/Kubernetes Cheat Sheet

Quick referrence list of useful commands and need to know information aggregated across docker, kubectl, minikube specifically for Windows 10 Pro systems.

## Docker

#### Installation Instructions:

https://docs.docker.com/docker-for-windows/install/

#### Additional Configuration:

Docker's default settings for the VM can be a little on the light side, depending what you are trying to do. You can adjust the defaults by following these few simple steps. I had to do this originally because of
the size require to compile a larger Angular application.

1. Navigate to and open

```PowerShell
C:\Users<profile name>\AppData\Roaming\Docker\settings.json
```

2. Adjust the `memoryMib` value to be something more practical, like `4096`

I also recommend you remove Docker Desktop from your start up programs list on Windows, or it will create the VM, hogging system resources whenever the machine starts.

1. Search `Startup Apps` in the Windows Task Bar.
2. Select `Startup Apps`.
3. Turn `Docker Desktop` to off.

If you are using Visual Studio Code, I highly recomend using the Visual Studio Code extension for Docker. It allows for circumventing writing a lot of these longer commands with quick `Ctrl + Shift + P` and GUI shortcuts.

1. Open Visual Studio Code.
2. Go to the Extensions area.
3. Search `Docker`.
4. Select and install the `Docker` extension.

#### Mid Level YouTube Walk Through:

https://www.youtube.com/watch?v=cLT7eUWKZpg&t=2494s

#### Full Cheat Sheet

https://www.docker.com/sites/default/files/d8/2019-09/docker-cheat-sheet.pdf

#### Commands:

Get a complete list of commands and options

```PowerShell
docker --help
```

List all running containers

```PowerShell
docker ps
```

Build a Docker image from the the specified docker file (-f) with the following tagged (-t) name and version (name:version) from the following location (.). This example is being called from the location the docker file exists, inside a project directory, which is why the path is '.'.

```PowerShell
docker build --rm -f "my.prod.dockerfile" -t app-name:0.0.1 .
```

Push an image to a remote repository. These are usually a cloud service, but could be setup on an offline network. Tagging is what allows you to easily point your image to upload to correct place. You will also likely need to authenticate in some fashion using `docker login`, but this varies somewhat for each repository. They should have their own instructions on how to do this. If you push with no configuration or login, the default is to try to push your image to Docker Hub, which is public.

```PowerShell
docker tag <image-name:version> <repository-path:version>
```

```PowerShell
docker push <image-name:version>
```

Run a given container in detached mode (-d) where the current console is not linked to standard output of the container. Map the physical machine port (-p) to the container port. Automatically remove the container on exit (--rm).

```PowerShell
docker run --rm -d -p 443:443/tcp -p 80:80/tcp <name>
```

Stop a currently running container.

```PowerShell
docker kill <container-name>
```

Remove wasted space from partially compilled or unused containers on the Virtual Machine docker is running on.

```PowerShell
docker system prune
```

See what images are available for Docker to run in your local repository.

```PowerShell
docker images
```

Open a shell by executing the bash command in interactive mode (-i) with a psuedo terminale (-t) for a given container.

```PowerShell
docker exec -it <name/id> bash
```

Remove a specific image version from your repository

```PowerShell
docker rmi <name:version>
```

## KubtCtl

#### Installation Insturctions:

https://kubernetes.io/docs/tasks/tools/install-kubectl/

**NOTE:** If you are using the 'curl' command method in PowerShell, do not provide the command parameters `-LO`. The curl command is an alias in PowerShell for the Windows version of this command and they will cause the command to fail.

#### Interactive Kubernetes Tutorial:

https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/

#### Kubernetes YAML File Creation

https://www.mirantis.com/blog/introduction-to-yaml-creating-a-kubernetes-deployment/

#### KubeCtl Documentation

https://kubectl.docs.kubernetes.io/

#### Full Cheat Sheet

https://kubernetes.io/docs/reference/kubectl/cheatsheet/

#### Commands:

Get information about current Kubernetes resources, providing a name is optional. You can also use 'describe' for more detailed information, instead of 'get'.

```PowerShell
kubectl get pods | nodes | deployments | services <name>
```

Run a container from a certain image on a give port.

```PowerShell
kubectl run <name> --image=name:version --port=80
```

Apply works much in the same way that 'create' works, however, when commands are made with apply, you can recall the command to update the resource after making changes to the YAML file. If you want to use apply to update pods, you should create them with apply as well. Kubernetes warns against making resources with 'create' and trying to run apply on them to update them.

```PowerShell
kubectl apply -f resource.yaml
```

Create a Kubernetes resource (pod, service, etc) from a file. You can string multiple -f tags together for multiple files. You can also use the create command with flags to create resources, but with number of options you can work with, using a YAML file is a better overall UX.

**WARNING:** Kubernetes strongly recommends using the 'apply' command in a production setting.

```PowerShell
kubectl create -f resource.yaml
```

Run a command on a currently running pod in the k8s cluster.

Most commonly used to run `bash`, but note, you have to have bash installed in order to run it, and most base images do not install bash. You might have to run a command to install bash first depending on the image running in your pod.

```PowerShell
kubectl exec -it --namespace <namespace> <pod-name> <command>
```

Delete a Kubernetes resource (pod, service, etc)

```PowerShell
kubectl delete  pod | node | deployment | service <name>
```

Update the image (Rolling update) for a given deployment, for given container(s)

```PowerShell
kubectl set image deployment\<deploymnet-name> <container-name>=<image>:<version>
```

## MiniKube

#### Installation Instructions:

https://kubernetes.io/docs/tasks/tools/install-minikube/

#### Additional Configuration Steps:

MiniKube has some default values that are not all that useful for Windows users. You can follow these short steps to override the defaults with more
common settings. We need the switch that minikube is on do be able to access the public interent. We also want minikube to use hyperv by default, as it comes with Windows 10 Pro, and is more managable than VirtualBox. We also want a more practical memory value for this VM, so we can adjust the default value for that as well.

1. Navigate to and open:

```PowerShell
  C:\Users\<your profile name>\.minikube\config.json
```

2. Add the following JSON:

```JSON
  {
    "hyperv-virtual-switch": "Public Access Switch",
    "memory": 8152,
    "vm-driver": "hyperv"
  }
```

3. Adjust the memory value as needed, preferrably limiting to the bare minimum required.

In addition, the Public Access Switch by default is a virtual switch that exists when Hyper-V is enabled. When using a wired connection, access to the internet will function correctly, allowing MiniKube to download the resources it needs when spinning up the virtual maching. However, if MiniKube tries to download resources while you are on a WiFi connection, your NAC (Network Access Card) will require the ability to 'share hosts'. You can check to see if your NAC has this ability by running the following PowerShell command.

```PowerShell
netsh wlan show drivers
```

Check the line 'hosted network support', and that it says 'yes' or you wont be able to bridge your internet connection between the NAC and the virtual switch. If this is the case, always use a wired connection.

#### Commands:

Configure docker to build images inside the minikube VM instead of the Docker VM (so they can be used by minikube). All docker commands will execute in reference to the Kubernetes machine. (ie. docker images will show the images available on the kubernetes vm, not the docker vm)

```PowerShell
minikube docker-env | Invoke-Expression
```

Start a Kubernetes cluster with a single node running on a virtual machine

```PowerShell
minikube start
```

Delete the current minikube cluster, destroying the VM and everything stored on its virtual disk.

```PowerShell
minikube delete
```

Enable Cluster IP address tunnneling, which you enable in order to use the 'LoadBalancer' type service with MiniKube.

```PowerShell
minikube tunnel
```
