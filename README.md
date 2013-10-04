muninlite
=========

Muninlite for Qnap

At the moment there is no IPKG available to install Munin-Node on your QNAP-Device. Fortunately you can use muninlite (http://sourceforge.net/projects/muninlite/). Anyway muninlite doesn't support QNAP-Devices very well, so I made some tweaks. You can either use the patch file and get muninlite directly from SourceForge, or you can use the complete Script from this repository. The Script is based on muninlite 1.0.4 (with all plugins enabled) and was tested on a QNAP TS-419 Pro.

Installation
============

1. Copy the Script to ```/share/MD0_DATA/.qpkg/Optware/local/munin/```

2. Install xinetd (```ipkg install xinetd```)

3. Add the following line to /etc/services:

```
munin           4949/tcp        lrrd            # Munin
```

4. Add the following to ```/opt/etc/xinetd.conf```:

```
munin   stream  tcp     nowait  root    /opt/local/munin/munin-node
```

5. Create ```/opt/etc/xinetd.d/munin```:

```
service munin
{
        socket_type     = stream
        protocol        = tcp
        wait            = no
        user            = admin
        group           = administrators
        #only_from       = 10.42.42.25
        server          = /opt/local/munin/munin-node
        disable         = no
}
```
6. Restart xinetd (```killall xinetd && /opt/sbin/xinetd```)

7. Configure a Munin-Machine to gather data from the QNAP-Device (as you would with any munin-node-Client).

Troubleshooting
===============

Use the xinetd-Debugmode: ```/opt/sbin/xinetd -d```

