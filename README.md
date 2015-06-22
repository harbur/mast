# mast

Configuration managent on top of etcd

mast is CLI tool that uses etcd to get customizable configuration management values

In order to orchestrate a farm of containers it is often needed to customize key/values on specific subsets of containers. mast is using etcdctl to get the value of the requested key

Format:

```
mast <SERVICE> <KEY>
```

Keys that are looked for value (in priority order - first wins):

* /mast/service/SERVICE/KEY
* /mast/hostname/HOSTNAME/KEY
* /mast/metadata/METAKEY1=METAVALUE1/KEY
* /mast/metadata/METAKEY2=METAVALUE2/KEY
* /mast/metadata/METAKEYX=METAVALUEX/KEY
* /mast/global/KEY

# Dependencies

mast depends on:

* etcd (for the distributed key/value storage)
* fleet (for the machine's metadata sets)

_mast_ is only used for retrieving values, in order to set the values, _etcdctl_ can be used.

# Use Case

The most common way to use _mast_ is inside fleet unit files as such:

`mast %p KEY`

The `%p` is dynamically resolved by _systemd_ with the service name.

# Installation

Installation From CLI:

```
curl -L https://github.com/harbur/mast/raw/v0.0.1/mast > /usr/local/bin/mast
chmod +x /usr/local/bin/mast
```

Installation from Cloud-config at CoreOS:

```
coreos:
  units:
    - name: mast.service
      runtime: true
      command: start
      content: |
        [Unit]
        Description=Install Mast

        [Service]
        ExecStart=/usr/bin/bash -c '/usr/bin/sleep 10s && /usr/bin/mkdir -p /opt/bin && /usr/bin/curl -sSL https://github.com/harbur/mast/raw/v0.0.1/mast > /opt/bin/mast && chmod +x /opt/bin/mast'
```
