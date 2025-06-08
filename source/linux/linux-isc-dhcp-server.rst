.. index:: linux, debian, dhcpd

.. meta::
   :keywords: linux, debian, dhcpd

.. _linux-isc-dhcp-server:

isc-dhcp-server failed to start
===============================

The *isc-dhcp-server* included in Debian will attempt to start a
DHCPv6 instance on servers which have a dual-stack (IPv4 & IPv6) config.

If DHCPv6 is unconfigured because for example, Router Advertisements are
used for configuring IPv6 hosts, then the service will fail to start.
The DHCP(v4) is running but Systemd reports the service as failed.

One work-around is to force *isc-dhcp-server* to only start the v4
instance, add the following line to */etc/default/isc-dhcp-server*:

``INTERFACESv4=eth0``

where *eth0* is the interface on which DHCP requests should be serviced.

After restarting the service, the DHCP server shall now only run on v4
and as long as the v4 config is correct, Systemd will report the service
as successfully started.
