# Avahi + Alpine Linux Docker Image

This repository contains a basic Dockerfile for installing Avahi
on Alpine Linux.

This is a solution for broadcasting network services on a network.
Included is a service configuration for Samba.

### Configuring Avahi Services

To announce Samba on your network, setup a file called `smb.service`
(below) in a new folder `services/`. You can announce more services
here, such as SSH or SFTP.

```
<?xml version="1.0" standalone='no'?>
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
<service-group>
 <name replace-wildcards="yes">%h</name>
 <service>
   <type>_smb._tcp</type>
   <port>445</port>
 </service>
 <service>
   <type>_device-info._tcp</type>
   <port>0</port>
   <txt-record>model=RackMac</txt-record>
 </service>
</service-group>
```

### Running

```
docker run -d \
  -v $PWD/services:/etc/avahi/services \
  --net=host \
  --name=avahi \
  --restart=always \
  b32147/avahi
```

It's possible to not use `--net=host`, and instead specify the port mapping
`-p 5353:5353/udp` and optionally giving your Docker container a hostname
with `--hostname=myhostname` but I haven't gotten it to work correctly.

