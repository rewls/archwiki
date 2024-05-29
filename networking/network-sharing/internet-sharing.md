# Internet sharing

## 2 Configuration

### 2.2 Enable packet forwarding

> ##### Warning
>
> - Enabling IP forwarding without a properly configured firewall is a security risk.

- To check the current packet forwarding settings, run:

```shell
# sysctl -a | grep forward
```

- You will note options for controlling forwarding per default, per interface, as well as separate options for IPv4/IPv6 per interface. For detailed description of all available options, see the kernel documentation, https://docs.kernel.org/networking/ip-sysctl.html.

- To enable IPv4 and IPv6 packet forwarding, configure sysctl to set these settings:

```
net.ipv4.ip_forward = 1
net.ipv4.conf.all.forwarding = 1
net.ipv6.conf.all.forwarding = 1
```

- To make changes persistent across reboots, see Sysctl#Configuration.

- You might consider writing settings to a file with a descriptive filename, such as `/etc/sysctl.d/30-ipforward.conf`.
