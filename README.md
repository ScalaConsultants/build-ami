VyOS build-ami - this is fork from original repository https://github.com/vyos/build-ami

--------------

**NOTE:**
Changes I've made:
1. Add ansible task to change init script with user data script... 

Now you can use your own command in user data:

```
#cloud-boothook
#!/bin/bash

# add debian repo
sudo bash -c 'cat << EOF >/etc/apt/sources.list.d/debian.list
deb http://httpredir.debian.org/debian jessie main contrib non-free
EOF'

# install awscli
sudo apt-get update
sudo curl -O https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py --user
sudo /root/.local/bin/pip install awscli --upgrade --user

# if you need to load config or other vyos commands you can do this here
bash -c 'cat << EOF >/tmp/load_config
#!/bin/vbash
source /opt/vyatta/etc/functions/script-template
configure
load
commit
save
EOF'
chmod a+x /tmp/load_config
sg vyattacfg -c /tmp/load_config
```



Build is the same as original:
```
./vyos-build-ami <VyOS ISO URL>
```


