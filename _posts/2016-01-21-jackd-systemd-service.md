---
layout: post
title: jackd.service и проблема ограничений
date: 2016-01-21 23:25:18
comments: true
tags: systemd ru
---
Когда мне наконец-то надоело запускать jackd руками, написал простейший сервис.
Проблемой оказалось то, что сервис падал практически сразу после запуска.
Причиной являлось то, что [ограничения, устанавливаемые `/etc/security/limits.d/99-audio.conf` не наследуются сервисом.](http://www.freedesktop.org/software/systemd/man/systemd.exec.html)

В связи с этим, потребовалось добавить их в `jackd.service`. Получилось вот такая вот вещь:
{% highlight sh %}
[Unit]
Description=Running jackd for ardour4
After=sound.target

[Service]
LimitRTPRIO=infinity
LimitMEMLOCK=infinity
ExecStart=/usr/bin/jackd -d alsa -P dmix -C dsnoop -r 48000 -p 1024 -s -S 32

[Install]
WantedBy=multi-user.target
{% endhighlight sh %}

Стоит обратить внимание, что `ExecStart` установлен для использования alsa с dmix.
Если читатель не пользуется данными средствами, ему потребуется поправить параметры, чтобы привести сервис в рабочее состояние.
