---
author: Gaurav Sharma
description: Task With Parameters One of the goals that you should aim
  for when building your tasks is to make them reusable as much as
  possible. A simple way to
keywords: Task With Parameters,how to add paramerter in tekton task,how
  to add string parameter in tekton task,how to add array parameter in
  tekton task,tekton tutorials step by step ,tekton tutorials for
  beginners,tekton devops tutorials,what is tekton ,tekton tutorials for
  beginners,best tekton tutorials
lang: en
language: English
next-head-count: 29
revisit-after: 20 days
robots: index, follow
title:
- Task With Parameters - Learning-Ocean
- Task With Parameters - Learning-Ocean
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
# Task With Parameters

One of the goals that you should aim for when building your tasks is to
make them reusable as much as possible. A simple way to reuse a task in
different contexts is to add parameters to them. You can then substitute
the values of those parameters in the steps that compose your task.

\

## String Parameter

Create a new task with more reusable

**The goal is now to make this task reusable in different contexts.
Instead of always outputting Hello World, we now want it to say hello to
anyone.**

you can create a new file called **name-param.yaml** with below
contents.

``` {.ql-syntax spellcheck="false"}
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: name-param
spec:
  params:
    - name: name
      type: string
  steps:
    - image: ubuntu
      command:
        - /bin/bash
        - -c
        - echo "Hello $(params.name)"
```

Parameters are added in the spec section of the task. This new params
field will contain a list of parameters. For each parameter, you will
add a name and a type. The type can be either **string or array**.

now we can use **\$(params.\<NAME_OF_PARAMETER\>).** It will be replaced
with the value of the given parameter.

Now we are ready to run your first parametrized task. Start this new
task just as you would with any other normal task:

let\'s apply this task in our Kubernetes cluster using the below command

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ kubectl apply -f 04-name-param.yaml 
task.tekton.dev/name-param created
learning-ocean:~ gaurav$ 
```

\

let\'s start the task using the below command

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start name-param --showlog
? Value for param `name` of type `string`? Learning-Ocean
TaskRun started: name-param-run-2d65g
Waiting for logs to be available...
[unnamed-0] Hello Learning-Ocean
learning-ocean:~ gaurav$ 
```

\

This time, we should be prompted by the tkn CLI tool to add a value for
the **name** parameter. we can also pass in the values for the
parameters directly in the command line by using the -p argument:

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start name-param --showlog -p name=Learning-Ocean
TaskRun started: name-param-run-g5s5b
Waiting for logs to be available...
[unnamed-0] Hello Learning-Ocean
learning-ocean:~ gaurav$ 
```

\

The result will be the same, but Tekton won\'t prompt you for the
parameter value.

\

## Array Parameters

We can also use parameters of type array. These parameters will contain
an array of values that can then be expanded anywhere an array is used.
This type of parameter can be convenient when you expect an unknown
number of arguments to a command or a script that you might want to run.

\

In this next example, we will create a task that can take any number of
student\'s items and then list them in the form of students list.

``` {.ql-syntax spellcheck="false"}
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: array-params
spec:
  params:
    - name: students
      type: array
  steps:
    - name: printstudents
      image: ubuntu
      args:
        - $(params.students[*])
      script: |
        #!/bin/bash
        for i in $@
        do
          echo $i
        done
```

\

When you run this task, you will be asked to enter a value for items.
You can type in a list of comma-separated values.

\

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ kubectl apply -f array-param.yaml 
task.tekton.dev/array-params created
```

\

start the task below command

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start array-params --showlog
? Value for param `students` of type `array`? gaurav,saurav,manish
TaskRun started: array-params-run-xkckn
Waiting for logs to be available...
[printstudents] gaurav
[printstudents] saurav
[printstudents] manish
learning-ocean:~ gaurav$ 
```

\

\

## Adding a default value

We can also add a default value to our parameters. You can do this by
**adding a default** field to the param object.

let\'s see with example create a new file default-value-param.yaml with
below content.

``` {.ql-syntax spellcheck="false"}
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: default-param
spec:
  params:
    - name: name
      type: string
      default: gaurav
  steps:
    - name: hello world
      image: ubuntu
      command:
        - /bin/bash
        - -c
        - echo "Hello $(params.name)"
```

\

apply this new file to Kubernetes cluster using below command:

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ kubectl apply -f design/default-param.yaml 
task.tekton.dev/default-param created
learning-ocean:~ gaurav$ 
```

\

now, start this task, you will see the default value proposed by the tkn
CLI tool. Hit Enter to accept the default value suggested:

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start default-param --showlog
? Value for param `name` of type `string`? (Default is `gaurav`) gaurav
TaskRun started: default-param-run-mpns7
Waiting for logs to be available...
[hellouser] Hello gaurav
learning-ocean:~ gaurav$ 
```

You can also use \--use-param-defaults to start this task run
immediately with the default values for those parameters:

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start default-param --showlog --use-param-defaults
TaskRun started: default-param-run-chv28
Waiting for logs to be available...
[hellouser] Hello gaurav
learning-ocean:~ gaurav$ 
```

Task parameters can be used in one or many steps if you need to. That
can be useful to share inputs for your steps.

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
