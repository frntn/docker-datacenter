*It all started Feb 15 2016 from this official Docker 
[post on twitter](https://twitter.com/docker/status/699276372204773376)*

# frntn/docker-datacenter-helper

This repo will help you start an *Up & Running* **Docker Datacenter** solution made 
of the two following (commercial) products :

  * `UCP` : *Docker Universal Control Plane* offering on-prem management solution for Docker apps
  * `DTR` : *Docker Trusted Registry* offering on-prem image management and storage

Both requires a **C**ommercial **S**upport (CS) subscription.

You can get a free 30-days trial here : https://hub.docker.com/enterprise/trial

## Usage

### Start your Docker Datacenter

#### 1. spin up the `controller` and `registry` hosts. 

They will respectively run UCP and DTR
```bash
vagrant up controller registry 
# takes approx 30min on fresh install with a decent connection (1Mo/s)
```

*Note: if you have multiple network interfaces the `vagrant up` will prompt for
the good one. You can specify it in `Vagrantfile`->`config.vm.network` to
prevent this.*

#### 2. configure controller/UCP

Get controller/UCP dashboard IP address
```bash
./getip.sh controller
# results will be like 'X.X.X.X'
```

From your host browser go to `https://X.X.X.X` (the certificate is not trusted so 
you have to accept the *insecure* connection. That's the expected behavior).

Now connect using defaults `admin`/`orca` and in the settings page upload your license.

![controller-addlicense](img/controller-addlicense.png?raw=true)

It is also recommended that you change your password (upper-right corner : 
`admin`->`profile`)

![controller-editprofile](img/controller-editprofile.png?raw=true)

#### 3. configure registry/DTR

Get registry/DTR dashboard IP address
```bash
./getip.sh registry
# results will be like 'Y.Y.Y.Y'
```

From your host browser go to `https://Y.Y.Y.Y` (the certificate is not trusted so 
you have to accept the *insecure* connection. That's the expected behavior).

Now go to `Settings`->`general` to update the domain name to `registry.docker.local`
(Don't forget to hit `save and restart` at the bottom of the page)

![registry-editdomain](img/registry-editdomain.png?raw=true)

Then go to `Settings`->`License` and upload your license.

![registry-addlicense](img/registry-addlicense.png?raw=true)

Finally it is also recommended to setup a minimal authentication in
`Settings`->`Auth`

![registry-adduser](img/registry-adduser.png?raw=true)

#### 4. spin up 2 additional nodes and join the controller

```bash
# if you have NOT change the controller password
./helper.sh

# if you have change the controller password
UCP_ADMIN_PASSWORD=mynewpass ./helper.sh 
``` 

### Use your Docker Datacenter

The lab for UCP [available on github](https://github.com/docker/ucp_lab) gives
some example on how to deploy your first applications

## Why this helper

It can be difficult to have a valid setup while following the documentation 
which is still in very early stage and contains inconsistency or 
incompatibility between UCP and DTR. 

One example is the UCP documentation explains how to install CS engine 1.9 
and then pulls UCP latest which turns out to be 0.8 and only valid for CS 
engine 1.10.

*(Note: I'd be happy to help and contribute on that, but this is the 
commercial product and it's not available on github)*

## Miscellaneous

  * Docker Trusted Registry [official product page](https://www.docker.com/products/docker-trusted-registry) :
  * Docker Universal Control Plane [official product page](https://www.docker.com/products/docker-universal-control-plane) :

![official-logo](img/docker-datacenter.jpg?raw=true)

