wireguard
=========

Manages Wireguard Configuration and Services

Requirements
------------

N/A

Role Variables
--------------

| Variable                                           | Purpose                                                                                                                              | Type              | Default          |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ----------------- | ---------------- |
| wg_update_pkg_cache                                | Update Package Cache prior to installing wireguard                                                                                   | bool              | true             |
| wg_hide_private_key                                | Hide the private key from ansible output                                                                                             | bool              | true             |
| wg_systemd_service_prefix                          | Systemd service prefix                                                                                                               | string            | 'wg-quick@'      |
| wg_config_path                                     | Path to wireguard configuration directory                                                                                            | string            | '/etc/wireguard' |
| wg_configs                                         | Dictionary of wireguard configurations                                                                                               | dictionary        | {}               |
| wg_configs.INTERFACE                               | Defines a Wireguard Interface                                                                                                        | dictionary        | `undefined`      |
| wg_configs.INTERFACE.interface                     | Interface settings for INTERFACE                                                                                                     | dictionary        | `undefined`      |
| wg_configs.INTERFACE.interface.PrivateKey          | WG Private Key for INTERFACE                                                                                                         | string            | `undefined`      |
| wg_configs.INTERFACE.interface.ListenPort          | WG Listen Port for INTERFACE                                                                                                         | integer           | `undefined`      |
| wg_configs.INTERFACE.interface.FwMark              | See [WG Documentation](https://www.wireguard.com/netns/#routing-all-your-traffic)                                                    | integer           | `undefined`      |
| wg_configs.INTERFACE.interface.Address             | IP Address for WG INTERFACE                                                                                                          | string            | `undefined`      |
| wg_configs.INTERFACE.interface.DNS                 | DNS for WG INTERFACE                                                                                                                 | string            | `undefined`      |
| wg_configs.INTERFACE.interface.Table               | Controls the routing table to which routes are added                                                                                 | string            | `undefined`      |
| wg_configs.INTERFACE.interface.PreUp               | List of script snippets which will be executed by bash                                                                               | list (of strings) | `undefined`      |
| wg_configs.INTERFACE.interface.PostUp              | List of script snippets which will be executed by bash                                                                               | list (of strings) | `undefined`      |
| wg_configs.INTERFACE.interface.PreDown             | List of script snippets which will be executed by bash                                                                               | list (of strings) | `undefined`      |
| wg_configs.INTERFACE.interface.PostDown            | List of script snippets which will be executed by bash                                                                               | list (of strings) | `undefined`      |
| wg_configs.INTERFACE.interface.SaveConfig          | If set to `true', the configuration is saved from the current state of the interface upon shutdown | bool              | `undefined` |                   |                  |
| wg_configs.INTERFACE.peer                          | Peer settings for INTERFACE                                                                                                          | dictionary        | `undefined`      |
| wg_configs.INTERFACE.peer.PEER                     | Defines a peer for interface INTERFACE                                                                                               | dictionary        | `undefined`      |
| wg_configs.INTERFACE.peer.PEER.PublicKey           | public key calculated by wg pubkey                                                                                                   | string            | `undefined`      |
| wg_configs.INTERFACE.peer.PEER.PresharedKey        | preshared key generated by wg genpsk                                                                                                 | string            | `undefined`      |
| wg_configs.INTERFACE.peer.PEER.AllowedIPs          | list of IP (v4 or v6) addresses with CIDR masks from which incoming traffic for this peer is allowed                                 | list (of strings) | `undefined`      |
| wg_configs.INTERFACE.peer.PEER.Endpoint            | an endpoint IP or hostname, followed by a colon, and then a port number                                                              | string            | `undefined`      |
| wg_configs.INTERFACE.peer.PEER.PersistentKeepalive | seconds interval, between 1 and 65535 inclusive, of how often to send an authenticated empty packet to the peer                      | integer           | `undefined`                 |

**Ubuntu Specific Variables**

| Variable         | Purpose                      | Type              | Default         |
| ---------------- | ---------------------------- | ----------------- | --------------- |
| wg_packages      | Wireguard packages to manage | list (of strings) | [ 'wireguard' ] |
| wg_package_state | Package state                | string            | 'latest'        |

Dependencies
------------

N/A

Example Playbook
----------------

```yaml
- hosts: wgnode1
  roles:
     - { role: wireguard }
  vars:
    wg_configs:
      wg0:
        interface:
          PrivateKey: 'uP8UTl8CceQ2AAiUaU1W/+PwmOy9y7/TE3JB5uLGyWw='
          ListenPort: 51820
          Address: 192.0.2.1/24
        peer:
          wgnode2:
            PublicKey: 'M6xTAj1VNpE6sXMk24jJ5/iCFOmqINVYZp8MAAJE7Eo='
            AllowedIPs: [ '192.0.2.2/32' ]
            Endpoint: '198.51.100.3:51820'

```

License
-------

BSD
