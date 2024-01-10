---
author: Gaurav Sharma
description: Multiple Step Tekon Task and Script In many cases, your
  tasks will have more than one step. This is why the steps field is a
  list. You can specify mu
keywords: tekton task with multiple step,how to write a tekton task with
  multiple steps,how to write a script in tekton task,tekton tutorials
  step by steps,tekton tutorials for beginnners,tekton tutorials for
  beginners in simple language,devops tutorials learning ocean,devops
  tutorials by gaurav sharma,tekton turoials in simple language
lang: en
language: English
next-head-count: 29
revisit-after: 20 days
robots: index, follow
title:
- Task - Multiple Step - Learning-Ocean
- Task - Multiple Step - Learning-Ocean
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
# Multiple Step Tekon Task and Script

In many cases, your tasks will have more than one step. This is why the
steps field is a list. You can specify multiple steps using various
images.

create a file with the name multiple-step-task.yaml with below content

``` {.ql-syntax spellcheck="false"}
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: multiple-steps
spec:
  steps:
    - name: first
      image: ubuntu
      command:
        - /bin/bash
        - -c
        - echo "First step"
    - name: second
      image: centos
      command:
        - /bin/bash
        - -c
        - echo "Second step"
```

For the second task, we can use a different image. In this case, we are
using ubuntu image in first step and centos image in second task.

\

now we are ready to apply this task to our cluster and look at the
task\'s output with multiple steps.

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ kubectl apply -f multiple-step-task.yaml 
task.tekton.dev/multiple-steps created
learning-ocean:~ gaurav$
```

\

Now that the task is created, now we can list it using below command.

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task ls
NAME             DESCRIPTION   AGE
hello                          2 weeks ago
multiple-steps                 1 minute ago
learning-ocean:~ gaurav$ 
```

\

Go ahead and run this task to see the outputs of each echo statement:

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start multiple-steps --showlog
TaskRun started: multiple-steps-run-2fgnx
Waiting for logs to be available...
[first] First step
[second] Second step
learning-ocean:~ gaurav$ 
```

\

The log prefix is color-coded to make it easier to distinguish between
the various steps. If you have a terminal that does not support ANSI
color codes, you can disable this feature with the **\--no-color**
argument:

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start multiple-steps --showlog --no-color
```

\

Apart from the colors, now using your terminal defaults, the results
should be the same.

\

## Using scripts

Sometimes, we want to perform an operation that is more complex in your
steps\' command field. To do so, you can use a script instead of the
command you used previously. You can only have a single command or a
single script, but not both.

\

To do so, start with a new YAML file called **task-script.yaml** with
below content

``` {.ql-syntax spellcheck="false"}
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: task-script
spec:
  steps:
    - name: step-with-script
      image: ubuntu
      script: |
        #!/usr/bin/env bash
        echo "this is multi line"
        echo "shell script"
        echo "what we want to execute in single step"
        echo "Learning-Ocean Happy Learning"
```

Here, instead of using a command, a script is used, followed by a pipe
(\|) symbol. All of the lines at the same indentation level are then
executed as part of the script.The script\'s first line used a shebang
(#!) to tell the operating system which interpreter to use to parse the
script that followed. this is a Bash script that starts with echo
statements.

You can then apply this task to your cluster and run it with the Tekton
CLI tool to see the output:

``` {.ql-syntax spellcheck="false"}
Gauravs-MacBook-Air:~ gaurav$ kubectl apply -f task-script.yaml 
task.tekton.dev/task-script created
Gauravs-MacBook-Air:~ gaurav$
```

let\'s start the logs and see the logs using the below commands.

``` {.ql-syntax spellcheck="false"}
learning-ocean:~ gaurav$ tkn task start task-script --showlog
TaskRun started: task-script-run-tpwsb
Waiting for logs to be available...
[step-with-script] this is multi line
[step-with-script] shell script
[step-with-script] what we want to execute in single step
[step-with-script] Learning-Ocean Happy Learning
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
