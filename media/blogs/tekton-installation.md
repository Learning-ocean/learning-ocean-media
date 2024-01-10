---
author: Gaurav Sharma
description: Installation Now that you have a better understanding of
  what cloud-native continuous integration/continuous deployment (CI/CD)
  pipe
keywords: how to install tekton,how to install tekton in kubernetes ,how
  to install tekton,tekton development environment,tekton development
  tutorials,how to install tkn command,how to install tekton for
  beginners,tekton tutorials for beginners,tekton tutorials step by step
  ,devops tutorials,devops tutorials for beginners
lang: en
language: English
next-head-count: 29
revisit-after: 20 days
robots: index, follow
title:
- Installation - Learning-Ocean
- Installation - Learning-Ocean
viewport: width=device-width
---

::: {#__next}
<div>

[![](/images/learning-ocean.png){width="30" height="30"}
Learning-Ocean](/){.navbar-brand}

[]{.navbar-toggler-icon}

::: {.collapse .navbar-collapse}
-   [Ansible](/tutorials/ansible){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [Docker](/tutorials/docker){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [Kubernetes](/tutorials/kubernetes){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [Docker-Compose](/tutorials/docker-compose){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [Docker-Swarm](/tutorials/docker-swarm){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [Terraform](/tutorials/terraform){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [Shell Script](/tutorials/shellscript){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [AWS](/tutorials/aws){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [Subscribe](/subscribe){.text-dark .nav-link
    style="text-transform:capitalize"}
-   [About Us](/about){.text-dark .nav-link
    style="text-transform:capitalize"}
:::

</div>

::: {role="main"}
::: row
::: {.lo-leftbar .col-12 .col-md-2}
<div>

-   
-   
-   
-   

</div>
:::

::: {.lo-rightbar .col-12 .col-md-10}
::: row
::: {.lo-blog .col-12 .col-md-9}
::: lo-editor
# **Installation**

Now that you have a better understanding of what cloud-native continuous
integration/continuous deployment (CI/CD) pipelines are all about, it\'s
time to get your hands dirty and start exploring Tekton. In order to use
Tekton locally, there are some tools that you will need on your
computer.

\

Next, you will need to install the appropriate container runtime.

Once you have everything locally installed, you will also need a
Kubernetes cluster in which Tekton will live. There are many options
available to you. In this section, you will see how to install minikube,
a tiny distribution of Kubernetes that runs locally.

\

Finally, you will see how to install the Tekton custom resource
definitions (CRDs) on your cluster and install the command-line
interface (CLI) tool needed to access those resources. Once this is
done, you will be ready to get started with your Tekton Pipelines.

\

## Git

The first tool that we will need is Git. Git is a code versioning system
that is free and open source.

\

To install Git, you can go to
[https://git-scm.com/downloads](%20https://git-scm.com/downloads%20){rel="noopener noreferrer"
target="_blank"} and pick your operating system.

If you are using a Windows operating system, it will automatically start
a file download. Open this file to begin the installation process. The
website will redirect you to a page with the installation instructions
for your operating system for macOS and Linux.

\

## VS Code

Code editors are a divisive topic among software developers. You can
pick the one you want for the examples in this blog. The choice of the
integrated development environment (IDE) won\'t matter, but I prefer VS
Code because of its extension ecosystem.

With VS Code, you can find extensions to make you more productive in
just about any coding language, including YAML files.

\

## Installing the extensions

To install extensions, you can open the Extensions panel in VS Code with
Ctrl + Shift + X. From here, you can search for and install the
following extensions:

-   Kubernetes by Microsoft
-   YAML by Red Hat
-   Tekton Pipelines by Red Hat

\

To install the extensions, click on the Install button.

Now that you have a running developer environment, it\'s time to start
adding the necessary tooling to containerize and deploy your
application.

\

## Installing a container runtime

There are many different options available to you. In this blog, we will
focus on the most popular choice, which is Docker.

\

Docker

Docker, from the eponymous company, is the most popular option out
there. This is especially true if you are using a Windows or macOS
operating system. To run containers, you technically have to use a Linux
operating system. Docker will take care of creating a virtual machine
(VM) and will run the containers in there.

\

You can install Docker by visiting their Getting Started with Docker
page at
[https://docker.com/get-started](https://docker.com/get-started){rel="noopener noreferrer"
target="_blank"}.

\

To verify the installation of Docker, you can run the following command:

\

``` {.ql-syntax spellcheck="false"}
$ docker version
```

\

## Docker Hub

While you are on the Docker Get Started with Docker page, you can also
sign up for a free Docker Hub account. This account will give you access
to a container registry. A registry is to containers what a repository
is to code. It is a central place to store and share your images.

\

## Kubernetes distribution (local, cloud, hosted)

Tekton runs inside a Kubernetes cluster, so to do any of the exercises
in this blog, you will need access to such a cluster.

If you already have a Kubernetes cluster available to you, feel free to
skip this section and use your own.

If you do need access to your own Kubernetes cluster to experiment with
the tutorials, I recommend using a Kubernetes distribution that runs
locally.

\

## Minikube

The easiest way to get started with Kubernetes is by running a micro
distribution on your computer. There are many micro distributions
available, such as **MicroK8s** by Canonical and
https://docker.com/get-started by Rancher. The one that will be used
here is by the **Cloud Native Computing Foundation (CNCF)** and is
called **minikube**.

\

You will find the download and installation instructions for minikube on
any operating system at https://minikube.sigs.k8s.io/docs/start/.

\

Once it\'s installed, you can start a cluster with the following
command:

\

``` {.ql-syntax spellcheck="false"}
$ minikube start
```

\

You\'ll then have a Kubernetes cluster running on your personal
computer.

\

## Connecting to your Kubernetes cluster

To interact with your Kubernetes cluster, you will need a CLI tool
called **kubectl**. If you\'ve already interacted with a Kubernetes
cluster, you are most likely familiar with this tool. If you don\'t have
it installed already, you can go to the Kubernetes official website and
follow your operating system\'s installation instructions. kubectl
installation at
[https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/){rel="noopener noreferrer"
target="_blank"}

Once the tool has been installed, you can test it out with the following
command:

``` {.ql-syntax spellcheck="false"}
$ kubectl version
```

This command should output the current version of kubectl.

You will also need to configure kubectl to use your existing cluster. If
you are using minikube, you can restart your cluster. When using
minikube start, it will automatically configure kubectl for you.

\

For minikube, type the following command:

``` {.ql-syntax spellcheck="false"}
$ minikube stop && minikube start
```

If you are using a cloud-based Kubernetes distribution, you might need
to look up your provider\'s documentation.

With kubectl, you will be able to manage everything that is happening
inside your cluster. Now that you have all the necessary tooling, you
can start installing Tekton and the related tooling.

\

## Preparing the Tekton tooling

Now that you have all the necessary tooling to run Tekton, it\'s time to
get started with Tekton itself. The first step will be to install one
final CLI tool to interact with Tekton. You can find this tool along
with the installation instructions for your operating system at
[https://tekton.dev/docs/getting-started/#set-up-the-cli](https://tekton.dev/docs/getting-started/#set-up-the-cli){rel="noopener noreferrer"
target="_blank"}.

\

## Install Tekton

The next step will be to install Tekton on your Kubernetes cluster. This
command will install all the CRDs to be able to run Tekton Tasks and
Pipelines. To install Tekton from your command line run below command.

``` {.ql-syntax spellcheck="false"}
$ kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

\

Now that you have everything installed, try to run the following
command:

``` {.ql-syntax spellcheck="false"}
learning-ocean:gaurav$ tkn version
Client version: 0.21.0
Pipeline version: v0.29.0
learning-ocean:adherelive-web gaurav$ 
```

\
:::

\

<div>

</div>

\
:::

::: {.m-0 .col-12 .col-md-3}
<div>

-   
-   
-   
-   

</div>
:::
:::
:::
:::
:::
:::
