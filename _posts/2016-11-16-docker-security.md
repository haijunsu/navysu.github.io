---
id: 192
title: Docker security
date: 2016-11-16T15:33:30+00:00
author: Navy Su
layout: post
guid: http://navysu.x10host.com/?p=192
permalink: /2016/11/16/docker-security/
categories:
  - docker
  - Linux
tags:
  - centos
  - docker
  - Fedora
  - linux
  - security
  - ubuntu
---
Detaching from the container without stopping Ctrl-P Ctrl-Q

Create docker user<!--?prettify linenums=true?-->

<pre class="prettyprint">$ sudo useradd dockeradmin
$ sudo passwd dockeradmin
$ sudo usermod -aG docker dockeradmin</pre>

1. Users are not namespaced. Root in container is root on host. Create a user in Dockerfile. Change to the user via USER or su/sudo/gosu<!--?prettify linenums=true?-->

<pre class="prettyprint">RUN groupadd -r user && useradd -r -g user user
USER user</pre>

2. Set container FS to read-only

<pre class="prettyprint">$ docker run --read-only debian touch x
touch: cannot touch 'x': Read-only file system
</pre>

3. Set Volumes to read-only/Use Data Volume Containers

<pre class="prettyprint">$ docker run -v $(pwd)/secrets:/secrets:ro debian touch /secrets/x
touch: cannot touch '/secrets/x': Read-only file system

$ docker run --volumes-from my-secret-container myimage
</pre>

4. Drop capabilities

<pre class="prettyprint">$ docker run --cap-drop SETUID --cap-drop SETGID myimage
$ docker run --cap-drop ALL --cap-add ...
</pre>

5. Set CPUSHARES

<pre class="prettyprint">$ docker run -d myimage
$ docker run -d -c 512 myimage
</pre>

6. Set Memory limits

<pre class="prettyprint">$ docker run -m 512m myimage
</pre>

7. Defang setuid/setgid binaries

<pre class="prettyprint">// to find them
$ docker run debian \
   find / -perm +6000 -type f -exec ls -ld {} \; 2&gt; dev/null

// to defang them
FROM debian:wheezy
RUN find / -perm +6000 -type f -exec chmod a-x {}; \; || true
</pre>

9.

8. Auditing (Immutable infrastructure, Audit images, not containers)
  
tools:

<pre class="prettyprint">$ docker diff ...
$ scalock
$ twistlock
$ clair
</pre>

&nbsp;

Reference: <a href="https://www.youtube.com/watch?v=A32Yjizt2_s" target="_blank">https://www.youtube.com/watch?v=A32Yjizt2_s</a>