# Windows 10 Pro - Docker/Kubernetes Cheatsheet
Quick referrence list of useful commands aggregated across docker, kubectl, minikube specifically for Windows 10 Pro systems.

## Docker

  #### Mid Level YouTube Walk Through:

  https://www.youtube.com/watch?v=cLT7eUWKZpg&t=2494s

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

  #### Commands:

  <pending>

## KubtCtl

  #### Interactive Kubernetes Tutorial:

  https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/ 

  #### Installation:

  See MiniKube Installation

  #### Commands:

  Get information about current Kubernetes resources, providing a name is optional

    `kubectl get pods | nodes | deployments <name>`

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

  Configure docker to build images inside the minikube VM instead of the Docker VM (so they can be used by minikube).

    `minikube docker-env | Invoke-Expression`

  Start a Kubernetes cluster with a single node running on a virtual machine

    `minikube start`

  Delete the current minikube cluster, destroying the VM and everything stored on its virtual disk.

    `minikube delete`
