# Ubuntu 24.04 WSL2 instance provisioned using `cloud-init`

Why waste time spinning a vanilla Ubuntu Virtual Machine on Hyper-V / VM-Ware when you can get going with WSL2.

The `user-data` file provides a pre-configured Ubuntu-24.04 WSL2 with all the bells-and-whistles you need to get
running in no time, plus no fighting with Virtual Switches and Enterprise Hypervisor issues.


## Pre-requisites

> NOTE: versions reflected are at the time of creation of this repository and can change

- __Windows Terminal__: version 1.22.12111.0
- __Windows Subsystem for Linux 2__: verson 2.5.10.0

## Steps

1. (in POWERSHELL) Locate your `USERPROFILE` directory on Windows:

```powershell
cd $env:USERPROFILE
```
2. (in POWERSHELL) in the `USERPROFILE` directory create a `.cloud-init` directory:

```powershell
mkdir .cloud-init
```

3. create a file in the `.cloud-init` directory called `Ubuntu-24.04.user-data` and copy the content.
4. (in POWERSHELL) Install a new Ubuntu-24.04 WSL2 image using:

```powershell
wsl --install -d Ubuntu-24.04
```

This will install the Image + start to provision the image using `cloud-init`. It might take a couple of minutes
and once done, drop you in the Linux Shell

5. (in Linux) exit from the Linux Shell

```bash
exit
```
6. (in POWERSHELL) stop (terminate) the Ubuntu-24.04 instance (this is needed to perform some final provisioning)

```powershell
wsl --terminate Ubuntu-24.04
```
7. (in POWERSHELL) Restart the instance again:

```poweshell
wsl -d Ubuntu-24.04
```
8. (in POWERSHELL) Teardown the instance (optional):

```powershell
wsl --unregister Ubuntu-24.04
```

> NOTE: any changes to the `user-data` file will __NOT__ be available directly to the running instances. You will need to teardown
>       the instance and re-install the image again.

## Additional Provisioning Logic

- use the `packages` list to add more APT packages to the instance
- use the `write_files` list to generate any files with content as root for initial boot setup
- use the `run_cmd` list to run any commands as root in the provisioning phase (use `chown` to make files / directory with a specific owner)

## Added Benefits

Any web containers or services run within the WSL2 instance is available to your browser under `localhost` without any complicate networking setup -
e.g., an `nginx` container exposed on port `8080` will be available to your Windows system under `http://localhost:8080`.

## Docs

- [Ubuntu Docs for WSL2 Automatic Setup](https://documentation.ubuntu.com/wsl/latest/howto/cloud-init/)
- [cloud-init Examples](https://cloudinit.readthedocs.io/en/latest/reference/examples.html)
