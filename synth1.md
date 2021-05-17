

    dd if=/dev/zero of /dev/sdb2 bs=1k


    mdadm --detail /dev/md1


> > [!TIP]
>     mdadm --detail /dev/md0

- list fs and partitions :
    
        sudo fdisk -l

- erase sdb5 instead of sdb2 :  
        
        dd if=/dev/zero of /dev/sdb5 bs=1k


