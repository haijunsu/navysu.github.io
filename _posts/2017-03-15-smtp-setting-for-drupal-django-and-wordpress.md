---
id: 413
title: SMTP setting for Drupal, Django, and WordPress
date: 2017-03-15T10:52:58+00:00
author: Navy Su
layout: post
---
Drupal, Django, and WordPress are using localhost as SMTP server by default. But there is a way to use another SMTP server to send email.

  * Django: Mail is sent using the SMTP host and port specified in the [<code class="xref std std-setting docutils literal"><span class="pre">EMAIL_HOST</span></code>](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-EMAIL_HOST){.reference.internal} and [<code class="xref std std-setting docutils literal"><span class="pre">EMAIL_PORT</span></code>](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-EMAIL_PORT){.reference.internal} settings. The [<code class="xref std std-setting docutils literal"><span class="pre">EMAIL_HOST_USER</span></code>](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-EMAIL_HOST_USER){.reference.internal} and [<code class="xref std std-setting docutils literal"><span class="pre">EMAIL_HOST_PASSWORD</span></code>](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-EMAIL_HOST_PASSWORD){.reference.internal} settings, if set, are used to authenticate to the SMTP server, and the [<code class="xref std std-setting docutils literal"><span class="pre">EMAIL_USE_TLS</span></code>](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-EMAIL_USE_TLS){.reference.internal} and [<code class="xref std std-setting docutils literal"><span class="pre">EMAIL_USE_SSL</span></code>](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-EMAIL_USE_SSL){.reference.internal}settings control whether a secure connection is used. <https://docs.djangoproject.com/en/1.10/topics/email/>
  * Drupal: install SMTP Authentication Support
  
    <https://www.drupal.org/project/smtp>
  * WordPress: WP Mail SMTP <https://wordpress.org/plugins/wp-mail-smtp/>