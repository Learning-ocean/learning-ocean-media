---
author: Gaurav Sharma
description: Tasks Tasks are the basic building block that you will use
  to build your larger pipelines. A Task is a collection of Steps that y
keywords: first task tekton,first task in tekton ,what is task in
  tekton,tekton step by step tutorials,tekton tutorials for
  beginners,best tekton tutorials,tekton by learning-ocean,tekton cicd
  tools tutorials,tekton tutorials step by step for beginners,hello
  world task in tekton ,how to watch logs for tekton task
lang: en
language: English
next-head-count: 29
revisit-after: 20 days
robots: index, follow
title:
- First Task - Learning-Ocean
- First Task - Learning-Ocean
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
# Tasks

Tasks are the basic building block that you will use to build your
larger pipelines. A Task is a collection of Steps that you define and
arrange in a specific order of execution as part of your continuous
integration flow. A Task executes as a Pod on your Kubernetes cluster.
A Task is available within a specific namespace, while a ClusterTask is
available across the entire cluster.

A Task declaration includes the following elements:

-   Parameters
-   Resources
-   Steps
-   Workspaces
-   Results

\

In a nutshell, tasks should perform a single operation in your CI/CD
pipeline. Examples of tasks could include cloning a repository,
compiling some code, or running a series of tests. When we build our own
tasks, then we should also try to make them as reusable as possible.

\

We prepared our local environment to build, manage, and run Tekton CI/CD
pipelines. It is now time to get started with some hands-on examples.

\

As a rule of thumb, your tasks will always start like this:

``` {.ql-syntax spellcheck="false"}
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: example-task-name
  labels:
     key: value
spec:
```

\

In the spec section, we can describe our actual task. This is where we
can define the various elements we want to use to achieve the task\'s
goal. Those elements could be parameters, workspaces, results, or steps.

Tasks are scoped to a single namespace in your cluster, but you might
have **ClusterTasks** defined for the **entire cluster** by your
administrators.

Once we trigger this task, everything will run inside a single Pod,
which will be terminated once a step fails or once all of the steps are
executed successfully.

\

## Steps

Steps are the only required objects to create a task, and that makes
sense. Steps describe the containers that will run as part of the task.
This is where the actual operations to be performed on your inputs
happen.

In the YAML file that describes the task, you define steps by adding an
array describing the steps and the order in they should be performed in.

Each step must have, at a minimum, an image to use. It is also highly
recommended to use a command value or a script field. This is because
the container\'s entry point is overwritten with an executable that
manages the step execution for Tekton.

\

A typical Step would also contain a name and would generally look like
this:

``` {.ql-syntax spellcheck="false"}
spec:steps:- image: alpine:3.12command:- /bin/bash- -c- echo Text from a step
```

\

If you want to use an image from a private registry, you can add an
ImagePullSecret Kubernetes object to the service account used by the
task.

\

Now that you understand how tasks and Steps are defined, you will see
how to use this knowledge to build your first working task.

\

## First task Hello World task

this task might not be instrumental in your day-to-day life, it will
demonstrate the basic concepts to build your first Tekton task:

First, start with a new YAML file called hello.yaml. In that file, write
below content.

``` {.ql-syntax spellcheck="false"}
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello
spec:
  steps:
   - name: 'print hello world'
     image: registry.access.redhat.com/ubi8/ubi-minimal
     command:
       - /bin/bash
       - -c
       - echo "Hello World"
```

\

Now that you have your first task defined, you can apply it to your
cluster using the kubectl tool in your terminal:

``` {.ql-syntax spellcheck="false"}
$ kubectl apply –f ./hello.yaml
```

\

It is now time to use the tkn CLI tool to manage and execute this task.
We can list the tasks that are available in the cluster by using **task
ls**, as shown here:

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task ls
NAME             DESCRIPTION   AGE
hello                          2 weeks ago
```

\

We can see the task that, just created here. Now that you know this task
is available for you to use, you can start it by using the start command
followed by the task\'s name:

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start hello 
TaskRun started: hello-run-88vs4
In order to track the TaskRun progress run:
tkn taskrun logs hello-run-88vs4 -f -n default
```

Notice that this created a TaskRun with a random name. The TaskRun,
which is the actual execution of this task, is now running in your
cluster. Because this task is running inside a Pod on the cluster, you
can\'t see the logs directly in your console.

\

We can use the **\--showlog** argument when we start the task to view
the logs directly.

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start hello --showlog
TaskRun started: hello-run-x9q6k
Waiting for logs to be available...
[print hello world] Hello World
learning-ocean:~ gaurav$ 
```

\

This started a new task run with a new random name. This time, you will
see the outputs of each step as they are executed.

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
