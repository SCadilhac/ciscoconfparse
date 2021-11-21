
Fork of ciscoconfparse without DNS resolver to work with GraalVM.

To be used in Netshot 0.16.2+:

```bash
su - netshot -s /bin/bash
cd /usr/local/netshot
mkdir python && cd python
graalpython -m venv venv
source venv/bin/activate
/usr/local/netshot/python/venv/bin/graalpython -m pip install --upgrade pip
pip3 install loguru
pip3 install git+https://github.com/SCadilhac/ciscoconfparse
```

Add the following line to `netshot.conf`:

```
netshot.python.virtualenv = /usr/local/netshot/python/venv
```

In your compliance script, you can import the installed package:

```js
import site
from ciscoconfparse import CiscoConfParse

# Script template - to be customized
def check(device):
  ## Grab some data:
  config = device.get('running_config')
  parse = CiscoConfParse(config.splitlines(), syntax='ios')
  for intf_obj in parse.find_objects_w_child('^interface', '^\s+shutdown'):
    debug(intf_obj.text + " is shutdown")
```

