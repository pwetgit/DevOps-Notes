#### https://developer.hashicorp.com/packer/tutorials/docker-get-started/get-started-install-cli

### Ubuntu/Debian

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install packer


# Build 'proxmox-iso' errored after 310 milliseconds 542 microseconds: could not find a supported CD ISO creation command (the supported commands are: xorriso, mkisofs, hdiutil, oscdimg)
sudo apt install mkisofs


# proxmox-iso: delete volume failed: 501 Method 'DELETE /nodes/homelab/storage/local/content/' not implemented

add terraform user persmissions to local storage (GUI)

```
