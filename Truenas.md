```bash 
wget https://download.truenas.com/TrueNAS-SCALE-Bluefin/22.12.3.3/TrueNAS-SCALE-22.12.3.3.iso
```


uncheck Enable Host Path Safety Checks.


### 🔄 Rename Dataset
To Rename trueNAS dataset
Stop any sharing first, then:
```bash
zfs rename pool/dataset pool/dataset_new
```