# mlx4vf-helper
Helper script for setting up VF on SR-IOV enabled Mellanox ConnectX-3 HCA


## Installation
```bash
$ git clone https://github.com/storpool/mlx4vf-helper
$ cd mlx4vf-helper
$ sudo cp mlx4vf-helper /sbin/
$ sudo cp mlx4vf-helper.conf /etc/
```
## Usage
```
mlx4vf-helper {interfaceName}
```
### CentOS
Create /sbin/ifup-local and place following lines
```bash
sudo cat >>/sbin/ifup-local <<EOF
#!/bin/bash

[ -x /sbin/mlx4vf-helper ] && /sbin/mlx4vf-helper "$1"
EOF
chmod a+x /sbin/ifup-local
```
### Debian
Add the following line in the configuration of the primary interface (/etc/network/interfaces)
```
post-up /sbin/mlx4vf-helper $IFACE
```

## Configuration
### Interface naming
**VFRENAME** 
*Default*: VFRENAME=1
Rename the VF interfaces using the following tmplate. (set to empty string to disable renaming)

**VFNAME_TEMPLATE** 
*Default*: VFNAME\_TEMPLATE="\_PFNAME__vf_VFID_"
\_PFNAME_ - name of the primary interface
\_VFID_ - ID of the VF
For example the if there are 2 VF on eth2, their names will be ***eth2_vf0*** and ***eth2_vf1***

**VF_UP_AFTER_RENAME**
*Default*: (unset)
When VF interface renaming is set, we are bringing down the interface before renaming it. If this variable is set to anything the new interface will be set to up.

### vf VLAN setup
**{vfname}\_VLAN**
Set VLAN ID to the given VF interface.
For example on the above setup of two VF on eth2 to have vlans 24 and 42:
```
eth2_vf0_VLAN=24
eth2_vf1_VLAN=42
```

# Copyright

 The MIT License (MIT)

 Copyright (c) 2015 StorPool Storage AD

 Permission is hereby granted, free of charge, to any person obtaining a copy
 of this software and associated documentation files (the "Software"), to deal
 in the Software without restriction, including without limitation the rights
 to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 copies of the Software, and to permit persons to whom the Software is
 furnished to do so, subject to the following conditions:

 The above copyright notice and this permission notice shall be included in all
 copies or substantial portions of the Software.

 THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
 SOFTWARE.