---
layout: post
title: systemctl edit и перезапись ExecStart
date: 2016-01-21 22:48:31
tags: systemd ru
comments: true
description: Как и зачем использовать новую команду edit
---

Похоже, упустил введение этой возможности в systemd [почти год назад](https://github.com/systemd/systemd/blob/master/NEWS#L1364).
`systemctl edit` позволяет редактировать юниты без необходимости копирования их вручную и прочих развлечений.

По-умолчанию, эта команда откроет файл, в котором пользователь может без проблем объявить необходимые ему значения в каких-либо секциях.

В случае, если необходимо переопределить существующий параметр полностью(такой, как например ExecStart), достаточно сначала присвоить ему пустое значение, а затем указать то, что нужно.

Например, мне понадобилось переопределить ExecStart в mpd.service чтобы иметь больше подробностей в логах, а также добавить ещё несколько мелочей.

В результате, мой `$HOME/.config/user/mpd.service.d/override.conf` превратился вот в такое чудо:

{% highlight sh %}
[Service]
Environment=PULSE_PROP=application.icon_name=mpd
ExecStart=
ExecStart=/usr/bin/mpd --verbose --no-daemon
PIDFile=$HOME/mpd/mpd.pid
{% endhighlight sh %}
