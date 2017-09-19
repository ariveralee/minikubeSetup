# Minikube installation guide

## Steps

Most of this is taken from the guide itself listed on Minikubes github page but I have curated the content to make it a bit easier for installing on OSX.

1.) If you do not have homebrew, install it by entering the following on the command line:

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2.) Use homebrew to install Minikube:

```
$ brew cask install minikube
```

3.) Next up, you want to install a Virtual box. There are several options listed on the Minikube page but for ease I went with [Virtual Box](https://www.virtualbox.org/wiki/Downloads). Install it and move to the next step.

4.) This one is **very** important. You need to install Kubectl. Here is what I did to make my life easier:

```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/darwin/amd64/kubectl
```

Note that it will download to whatever directory that you're in. Now do the following to turn kubectl into an executeable.

```
$ chmod +x ./kubectl
```

Next, you want to move this to your bin.

```
$ mv ./kubectl /usr/local/bin/kubectl
```

If the above does not work, you might have to use `sudo`.

5.) Now we want to see if everything works decently. From your command line:

```
$ minikube start
```

You should see something similar to `Kubectl is now configured to use the cluster`.

6.) Now we are going to use an example pod to see if everything works.

```
$ kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
```

If all goes well you should see `deployment "hello-minikube" created`

7.) Next you want to expose the deployment so enter:

```
$ kubectl expose delployment hello-minikube --type=NodePort
```

If it works, you'll see `service "hello-minikube" exposed`

8.) We should not be able to get the pod. Depending on how long it takes to set up, you may see either `ContainerCreating` or `Running` under `STATUS`

```
$ kubectl get pod
```

You should see the pod listed.

8.) If everything has gone right up to this point then you should be able to curl the pod:

```
$ curl $(minikube service hello-minikube --url)
```

You should see some output but be able to determine that the command was successful.

## Usage

### Stopping the service

To stop the service you can do:

```
$ minikube stop
```

### Remove a service

To delete a service you can do:

```
$ minikube delete [service]
```

