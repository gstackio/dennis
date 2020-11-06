# Dennis

This fork of Dennis is used to auto-switch to different DNS servers. Typically
when connected to a VPN that pushes DNS servers that will only resolve
specific names for specific domains.

Then you need to switch to those DNS servers only for resolving specific
names, ending with specific suffixes.

It is meant to be used in conjunction with the
[resolver](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man5/resolver.5.html)
system.


## Installation

Dennis is most easily installed using

    $ npm install dennis

Dennis needs a configuration file, which is read from
`~/.dennis.json`. Then run

    $ bin/dennis install

to get instructions for how dennis can be run in the background.

## Configuration

An example Dennis configuration located in `~/.dennis.json` looks like:

```json
{
  "port": 53,
  "domains": {
    "restricted.example.com": {
      "host": "1.1.1.1"
    }
  },
  "default": "9.9.9.9"
}
```

This configuration will query `1.1.1.1` for any hostname ending with
`restricted.example.com` and will query `9.9.9.9` for anything else.

After this, set Dennis as your default DNS server:

```
sudo networksetup -setdnsservers Wi-Fi "127.0.0.1"
```

Instead of `Wi-Fi`, you can use any valid macOS network service, as listed
with `networksetup -listallnetworkservices`.


The options for the configuration are:

* `port` (default: 1553) -- the port which Dennis will bind to
* `domains` (required)
* `default` (required)

Each key of `domains` (and the `default` one too) is a domain to proxy, for
which the following options are accepted:

* `host` (required) -- the host to proxy the request to
* `port` (default: 53) -- the port to use
* `proto` (default: udp) -- the protocol to use, `udp` or `tcp`
* `timeout` (default: 1000) -- the number of milliseconds before
  considering the request timed out  

## Tear down

When stopping to use Dennis, tear it dow with this command:

```
launchctl unload ~/Library/LaunchAgents/dennis.plist
sudo networksetup -setdnsservers Wi-Fi "Empty"
```
