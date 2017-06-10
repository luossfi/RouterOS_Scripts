<!---
DHCP Lease to DNS Record script for Miktrotik RouterOS.

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

# DHCP Lease to DNS Record
This script automatically creates a static DNS record for every address leased to a client, if the DHCP server could get a host name for this client.
If the lease is freed up again, the according DNS record will be removed.

## Setup
1. Go to `/ip dhcp-server `.
2. Put the script's content to the desired DHCP server's `lease-script` property.
3. Adjust the ttl (time to live) value in the scripts setup area to a value you prefer, this will be set in new DNS records.
4. Go to `/ip dhcp-server lease` and delete all existing **dynamic** entries and disable and re-enable all **static** entries.
5. The next time a client requests an IP address from the DHCP server the DNS record will be created (use `/ip dns static print` to see them).

## Requirements
* Tested with RouterOS version 6.39.2.
* Each leased IP address must match exactly one of the networks defined here: `/ip dhcp-server network`

## Warnings
* The script will also remove **manually** created DNS records if a DHCP lease if freed up with the same IP address.
* The script will adjust the DNS record's name also for **manually** created records if a new DHCP lease contains the same IP address.
* The script will create multiple DNS records with the same name if a client requests an IP address for different MAC addresses.
However the router seems to be able to deal with this.

## How The Script Works
The script is invoked each time the DHCP server leases/frees an IP address to a client.
* Case new DHCP lease is created:
  1. Get host name from DHCP lease and continue if it exists.
  2. Use lease's IP address to find matching network.
  3. Extract the domain from the first matching network
  4. Create fully qualified domain name from host name and domain if domain is set, otherwise use only the host name.
  5. Check if a DNS record exists for lease's IP address. If a record exists and the record's name is different from current
       fully   qualified domain name then update it, otherwise do nothing.
  6. If no DNS record exists, create one for lease's IP address, fully qualified domain name and configured TTL.
* Case DHCP lease is freed:
  1. Find all DNS record for lease's IP address.
  2. Iterate over these records and remove them.
## Miscellaneous
* Logging topics: debug, script