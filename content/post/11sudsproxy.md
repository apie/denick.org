+++
title = "How to use a SOCKS proxy with Suds"
date = "2024-03-18T19:00:00+02:00"
category = "blogposts"
draft = false
+++

The proxy option in *Suds* (a Python SOAP library) doesn't seem to work. After searching for an answer to no avail I decided to ask ChatGPT.
<!--more-->

It came up with a working answer right away: use *pysocks* and monkeypatch a default proxy:

```
from suds.client import Client
import socks
import socket

# Configure SOCKS proxy
socks.setdefaultproxy(socks.PROXY_TYPE_SOCKS5, "your_proxy_host", your_proxy_port)
socket.socket = socks.socksocket

# Example SOAP client initialization
url = 'http://example.com/your_wsdl_url?wsdl'
client = Client(url)

# Now you can use the 'client' object to make SOAP requests
```

A useful answer for all of us that are stuck with *Suds*. For new projects I recommend the use of *Zeep*.
# Resources:
- Suds: https://suds-py3.readthedocs.io/en/latest/
- PySocks: https://github.com/Anorov/PySocks
- Zeep: https://docs.python-zeep.org/en/master/
