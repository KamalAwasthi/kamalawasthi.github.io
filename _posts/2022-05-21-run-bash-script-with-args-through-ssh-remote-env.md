---
title: Run bash script with args through ssh (remote env)
date: 2022-05-21 00:00:00 +0530
categories:
- Technical
tags:
- ssh
- shell
- bash
layout: single
excerpt: Running a bash script with command line argument on a remote env through
  ssh
header:
  overlay_color: "#333"
  teaser: ''
  upload_image: ''
last_modified_at: 2022-05-21 00:00:00 +0530

---
We can connect securely to a remote Linux env through ssh by using a command similar to the following:

    ssh <username>@<ip/url> -i <private_key> 

but what if we want to run a bash script that we have at our local system after connecting to the remote environment (env). 

To achieve this we want to call out a **fresh** the bash **session** with the local script so that each command inside our script is securely transferred and executed on the bash session that gets open upon ssh call:

    ssh <username>@<ip/url> -i <private_key> 'bash -s' < example_script.sh

Things become tricky when we get even more ambitious and wish to pass the command line argument to our script that is going to be run over remote env.  Please note that using the ordinary method that we do in our local will not work as additional arguments will be interpreted as additional flags/arguments for ssh.

This will not work:

    ssh <username>@<ip/url> -i <private_key> 'bash -s' < example_script.sh 123 "Kamal"

To achieve this we need to put '--' after triggering a fresh bash session.

This will do the job:

    ssh <username>@<ip/url> -i <private_key> 'bash -s' -- < example_script.sh 123 "Kamal"

we can also give named argument using flags:

    ssh <username>@<ip/url> -i <private_key> 'bash -s' -- < example_script.sh -i 123 -n "Kamal"

Here, -i flag indicates the id of the person and -n indicated the name. We can use these flags inside the bash script to read arguments in any order.

That is it for today! Happy Coding! 

Namaste! 