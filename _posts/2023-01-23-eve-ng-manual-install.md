---
layout: post
title: "eve-ng lab install.....what a pain."
permalink: /2023/eve-ng-manual-install
date: 2023-01-23 00:00:00 -0000
---
# The what
Eve-ng now supports clustering hardware and while being a fan of the software I have always used the community edition as a standalone instance on a cloud compute node. Ie going the OVF route. Once I learned about this premium feature in the pro edition now the cost of getting an annual license was justified in my mind. Having a three node lab running in the basement seemed like the perfect enviroment to get this running.

# The how
Anyways, this post serves as my notes from 8 hours of trying to get eve-ng installed on bare metal. Here are the steps:
- Do a manual install of Ubuntu 20.04 on the node.
  - Only install ssh server.
- Run updates after install.
- Run manual installer: `wget -O - https://www.eve-ng.net/focal/install-eve-pro.sh | bash -i`

For a master node, job done. Satellite's have a little bit more work to do.

On satellite nodes:
- Run updates.
- Install eve-agent: `sudo apt install eve-agent -y`
- Remove no longer needed packages: `sudo apt autoremove -y`
- Reboot.
- Post reboot remove existing wireguard config file: `mv /etc/wireguard/wg0.conf ~/.etc.wireguard.wg0.conf.bak`
  - This step is important otherwise the nodes will fail to be added to the cluster.

From here the satellite nodes should be able to be added to the cluster.

# The why
Even though eve-ng.net offers downloads of ISO's that can be used for building a flashdrive installer they have a small problem.....No GPT or MBR boot partitions. This applies to both pro or community editions. So it doesnt matter how you flash the drive:
- dd
- balenaetcher
- ventoy
None of them will boot.

Changing the BIOS settings from UEFI to legacy doesn't help either.

# The score
2/5 pain in the neck:
- Too much time wasted on a bad installer ISO.
- Incomplete setup of satellite's in a minor way. Ie more manual work needed to get them ready.
