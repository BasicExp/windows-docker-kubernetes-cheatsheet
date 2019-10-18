# Windows Docker/Kubernetes Cheatsheet
Quick referrence list of useful commands aggregated across docker, kubectl, minikube specifically for Windows systems.

## Docker

  #### Commands

  <pending>

## KubtCtl

  #### Commands

  Get information about current Kubernetes resources, providing a name is optional

    `kubectl get pods | nodes | deployments <name>`

## MiniKube

  #### Additional Configuration Steps:

  MiniKube has some default values that are not all that useful for Windows users. You can follow these short steps to override the defaults with more
  common settings.

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

  #### Commands

  Configure docker to build images inside the minikube VM instead of the Docker VM (so they can be used by minikube).

    `minikube docker-env | Invoke-Expression`

  Start a Kubernetes cluster with a single node running on a virtual machine

    `minikube start`

  Delete the current minikube cluster, destroying the VM and everything stored on its virtual disk.

    `minikube delete`
