# Windows 10 Pro - Docker/Kubernetes Cheatsheet
Quick referrence list of useful commands aggregated across docker, kubectl, minikube specifically for Windows 10 Pro systems.

## Docker

  #### Installation:

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

  #### Commands:

  Get a complete list of commands and options.di
  
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

  Run a given container in detached mode (-d) where the current console is not linked to standard output of the container. Map the physical machine port (-p) to the container port. Automatically remove the container on exit (--rm).

  ```PowerShell
  docker run --rm -d -p 443:443/tcp -p 80:80/tcp <name>
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

## KubtCtl

  #### Installation:

  See MiniKube Installation

  #### Interactive Kubernetes Tutorial:

  https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/ 

  #### Commands:

  Get information about current Kubernetes resources, providing a name is optional

  ```PowerShell
  kubectl get pods | nodes | deployments <name>
  ```

  Run a container from a certain image on a give port.

  ```PowerShell
  kubectl run <name> --image=name:version --port=8080
  ```

## MiniKube

  #### Installation:

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
