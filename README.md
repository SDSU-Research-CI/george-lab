This repository contains code and yaml for the George Lab for use on the TIDE cluster.

**Namespace name:** sdsu-george-lab

## Shared Data

The following PVCs (disks) are available for use:

| PVC Name | Storage Class | Options | Data Description |
| -------- | ---- | ------- | ---------------- |
| shared-data | rook-cephfs-tide | ReadWriteMany | Data that should be shared between researchers can be stored here at the path /home/jovyan/shared/ | 

## Example Code

- [JupyterLab Job Examples](/jupyterlab)
