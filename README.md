<!---
RouterOS Skripts, a collection of scripts for MikroTik RouterOS automation.

Copyright (C) 2017++ Steff Lukas <steff.lukas@luossfi.org>

This program is free software: you can redistribute it and/or modify it under
the terms of the GNU Lesser General Public License as published by the Free
Software Foundation, either version 3 of the License, or (at your option) any
later version.

This program is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE. See the GNU Lesser General Public License for more
details.

You should have received a copy of the GNU Lesser General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.
--->

# MikroTik RouterOS Skripts
This project contains scripts to automate the MikroTik RouterOS ([www.mikrotik.com](http://www.mikrotik.com)) which I developed as I needed them.

Each script is located inside its own directory along with a readme which explains how to use it. If you have questions or ideas feel free to contact me!

## Available Scripts
### [DHCP Lease to DNS Record](./DhcpLeaseToDnsRecord/)
This script automatically creates a static DNS record for every address leased to a client. If the lease is freed up again, the according DNS record will be removed.

