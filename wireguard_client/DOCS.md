# Home Assistant Community Add-on: WireGuard Client

[WireGuard®][wireguard] is an extremely simple yet fast and modern VPN that
utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner,
and more useful than IPsec, while avoiding the massive headache.

It intends to be considerably more performant than OpenVPN. WireGuard is
designed as a general-purpose VPN for running on embedded interfaces and
supercomputers alike, fit for many different circumstances.

Initially released for the Linux kernel, it is now cross-platform (Windows,
macOS, BSD, iOS, Android) and widely deployable,
including via an Hass.io add-on!

WireGuard is currently under heavy development, but already it might be
regarded as the most secure, easiest to use, and the simplest VPN solution
in the industry.



## Installation

WireGuard Client add-on is pretty simple, however, can be quite complex for user that isn't
familiar with all terminology used. The add-on takes care of a lot of things
for you (if you want).

Follow the following steps for installation & a quick start:

1. Search for the "WireGuard Client" add-on in the Supervisor add-on store
   and install it.
1. use the following configuration as example: 

```yaml
interface:
  private_key: your-private-key
  address: 10.6.0.2
  dns:
    - 8.8.8.8
    - 8.8.4.4
  post_up: "iptables -t nat -A POSTROUTING -o wg0 -j MASQUERADE; iptables -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu"
  post_down: "iptables -t nat -D POSTROUTING -o wg0 -j MASQUERADE; iptables -D FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu"
  mtu: 1420
peers:
  - public_key: your-public-key
    pre_shared_key: your-preshared-key
    endpoint: 'xxxxxxxxxxxxxxx.duckdns.org:51820'
    allowed_ips:
      - 10.6.0.0/24
    persistent_keep_alive: 25
```

Please `0.0.0.0/0` is not allowed as `allowed_ips` value.

1. Save the configuration.
1. Start the "WireGuard" add-on

## WireGuard client status API

This add-on provides a simple WireGuard status API. This API is not an
official API, darn simple, and experimental, but does allow you to pull
in data from the add-on into Home Assistant.

With the use of the [Home Assistant RESTful][ha-rest] integration, one should
be able to grab some interesting data from this add-on.

Example:

```yaml
sensor:
  - platform: rest
    resource: http://a0d7b954-wireguard
```


