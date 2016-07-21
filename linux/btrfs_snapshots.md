# Creating snapshot of Folder

# btrfs snapshots

## Proposed workflow
create subvolume on `scan_avoidance_training_data` [0]

    btrfs subvolume create /media/scratch/tmp_scan_avoidance_training_data 
    mv /media/scratch/scan_avoidance_training_data/*  /media/scratch/tmp_scan_avoidance_training_data/
    rmdir /media/scratch/scan_avoidance_training_data/
    mv /media/scratch/tmp_scan_avoidance_training_data /media/scratch/scan_avoidance_training_data

create snapshot

    btrfs subvolume snapshot -r /media/scratch/scan_avoidance_training_data /media/scratch/SNAPSHOT/scan_avoidance_training_data
    sync

If we wanted to copy the data of the snapshot to another directory or subvolume `/somewhere/backup`

    btrfs send /media/scratch/SNAPSHOT/scan_avoidance_training_data | btrfs receive /somewhere/backup

create snapshot of subvolume [2]

    btrfs subvolume snapshot /path/to/subvolume /path/to/subvolume/snapshot_name

# References

[0] https://www.linux.com/learn/how-create-and-manage-btrfs-snapshots-and-rollbacks-linux-part-2
[1] https://btrfs.wiki.kernel.org/index.php/Incremental_Backup
[2] https://btrfs.wiki.kernel.org/index.php/UseCases
