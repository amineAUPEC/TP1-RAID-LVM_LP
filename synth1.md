


# 

- no swap
- not a lot of software
- install grub on /sda








> [!CAUTION]
> do not reference it on fstab  
> do not mount the volume 

### erase sdb2 volume 
    dd if=/dev/zero of /dev/sdb2 bs=1k


    mdadm --detail /dev/md1


###  afficher leur taille :
    df -h


> > [!TIP]
>    /dev/md[0-9]+ is the name of RAID volume,   
>  so it's starting from md1 in our case  
>     `mdadm --detail /dev/md0`   

- list fs and partitions :
    
        sudo fdisk -l

- erase sdb5 instead of sdb2 :  
        
        dd if=/dev/zero of /dev/sdb5 bs=1k


