# Reference for building PhotonOS Virtual Appliance (OVA) using Packer

## Requirements

* MacOS or Linux Desktop
* vCenter Server or Standalone ESXi host 6.x or greater
* [VMware OVFTool](https://www.vmware.com/support/developer/ovf/)
* [Packer](https://www.packer.io/intro/getting-started/install.html)


> `packer` builds the OVA on a remote ESXi host via the [`vmware-iso`](https://www.packer.io/docs/builders/vmware-iso.html) builder. This builder requires the SSH service running on the ESXi host, as well as `GuestIPHack` enabled via the command below.
```
esxcli system settings advanced set -o /Net/GuestIPHack -i 1
```

Step 1 - Clone the git repository

```
git clone https://github.com/lamw/photonos-appliance.git
```

Step 2 - Edit the `photon-builder.json` file to configure the vSphere endpoint for building the PhotonOS appliance

```
{
  "builder_host": "192.168.30.10",
  "builder_host_username": "root",
  "builder_host_password": "VMware1!",
  "builder_host_datastore": "vsanDatastore",
  "builder_host_portgroup": "VM Network"
}
```

**Note:** If you need to change the initial root password on the PhotonOS appliance, take a look at `photon-version.json` and `http/photon-kickstart.json`. When the OVA is produced, there is no default password, so this does not really matter other than for debugging purposes.

Step 3 - Start the build by running the build script which simply calls Packer and the respective build files

```
./build.sh
````

If you wish to automatically deploy the PhotonOS appliance after successfully building the OVA. You can edit the `photon-dev.xml.template` file and change the `ovftool_deploy_*` variables and run `./build.sh dev` instead.
