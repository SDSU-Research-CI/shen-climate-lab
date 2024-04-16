This repository contain code and yaml for the Shen Climate Lab for use on the National Research Platform Nautilus hyper-cluster.

**Namespace name:** sdsu-shen-climate-lab

## Shared Data

The following PVCs (disks) are available in the sdsu-shen-climate-lab namespace and available for use:

| PVC Name | Storage Class | Options | Data Description |
| -------- | ---- | ------- | ---------------- |
| fourcastnet-training-data | rook-cephfs-tide | ReadWriteMany | This PVC contains the FourCastNet data from this [Globus collection](https://app.globus.org/file-manager?origin_id=945b3c9e-0f8c-11ed-8daf-9f359c660fbd&two_pane=false). The path on the disk is /data/FourCastNet/ | 

## Example Code

- [Globus Deployment to populate FourCastNet PVC](/globus)
