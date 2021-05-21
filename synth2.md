## profs :


    sudo pvcreate /dev/sdc
- - `sudo pvcreate /dev/md2`



   `sudo pvcreate /dev/sdd`

<!-- sudo vgcreate vgstockage /etc/sdc /etc/sdd -->
<!-- sudo vgcreate vgstockage sdc sdd -->
<!-- sudo pvlist -->
<!-- sudo pvl -->

> creating vgstockage
    sudo vgcreate vgstockage /dev/sdc /dev/sdd

    sudo lvcreate -L 1G -n lvstockage vgstockage
    
    sudo lvs
- re lister les stockages  
    `sudo lvs`


- augmenter la taille d'un 1GB en précisant un chemin complet *obligatoire*    
`sudo lvextend -L+1G  /dev/vgstockage/lvstockage`


- lister les stockages  
*on remarque que les stockages ont été redimensionnées*  
   ` sudo lvs`

> lvstockage  
    sudo lvextend -L+1G  /dev/vgstockage/lvstockage  
    sudo lvs

    sudo lvextend -L+1G  /dev/vgstockage/lvstockage

    sudo lvs

> [!WARNING]
> pas de places car impossible de créer virtuellement si on ne les mutualise pas physiquement  
> Virtualbox il donne l'illusion, d'ou le fait que cest possible mais des quon ecris les données dès quil est plein.  
> Il va lever l'exception avec un msg derreur, ou message système "Freenas: permission non accordée"   

####                # réduit à 1GB
>>    `sudo lvreduce -L 1G  /dev/vgstockage/lvstockage <<< "y"`
#### réduit de 1GB
>>   `sudo lvreduce -L -1G  /dev/vgstockage/lvstockage <<< "y"`


## eleves :

- installer LVM  
`sudo apt-get update -y && sudo apt-get install -y lvm2`



- init pvcreate sdb  
`sudo pvcreate /dev/sdb`


- init pvcreate sdc  
`sudo pvcreate /dev/sdc`


- creating vgstockage  
`sudo vgcreate vgstockage /dev/sdc /dev/sdd`



- lvstockage avec 75%  
`sudo lvcreate -l 75%VG -n lvstockage vgstockage`


- lister les stockages    
   ` sudo lvs`
###### formater en ext4  
<!-- sudo mkfs.ext4 /dev/vgstockage/ -->
>>    `sudo mkfs.ext4 /dev/vgstockage/lvstockage`


###### montage persitant avec fstab
1. `sudo mkdir -p /mnt/TP1`  
1. `sudo nano /etc/fstab`  
1. `echo "/dev/vgstockage/lvstockage     /mnt/TP1     ext4     errors=remount-ro     0     1    " >> /etc/fstab`  



<!-- sudo fdisk -l -->

- reboot for fstab  
`sudo reboot`  

- vérif  
`sudo df -h | grep /mnt/TP1  `




<!-- - Ajoutez trois nouveaux disques durs SATA dans votre VM et  -->

<!-- - lancez la création d'un  nouveau volume* (en RAID 5).  -->

- initier en RAID 5 :  
`cat /proc/partitions`  
`sudo mdmadm --create --verbose /dev/md3 --level=5 --raid-devices=3 /dev/sdd /dev/sde /dev/sdf --spare-devices=0`  

- Initialisez ce volume en tant que PV*  
   `sudo pvcreate /dev/$sdd`

- puis ajoutez* ce dernier dans le VG **vgstockage**.   
`sudo vgextend /dev/vgstockage/lvstockage /dev/$sdd`


- augmentez la capacité du LV* : de 4 Go supplémentaires.  
`sudo lvextend -L+4G  /dev/vgstockage/lvstockage`


- Comparez la taille du LV* à celle de la partition*    

    `sudo cat /proc/partitions`  
    `sudo df -h`  
    
    `sudo lvs`  
    
    `sudo vgdisplay vgstockage`  
    <!-- `sudo vgdisplay` -->


###### *Pour utiliser la totalité de l’espace du LV* :  
- étendre son système de fichiers*.  

> [!NOTE]
> Après cette opération, vérifiez que la totalité de l’espace de stockage est bien utilisée par le système de fichier.   
- Affichez les propriétés du VG* :   
> *pour vérifier qu'il lui reste encore de l'espace non alloué.*  
`sudo vgdisplay vgstockage`



Last login: Fri May 21 15:24:19 2021  
```bash
etudiant@debian-stretch:~$ history  
    1  sudo reboot  
    2  sudo shutdown now  
    3  ip a  
    4  ls  
    5  cat /proc/partitions  
    6  sudo shutdown now  
    7  cat /proc/partitions  
    8  sudo pvcreate /dev/sdb  
    9  sudo pvcreate /dev/sdb/  
   10  sudo apt-get update
   11  sudo lvs
   12  su
   13  sudo nano /etc/fstab 
   14  sudo mkdir -p /mnt/TP1
   15  sudo nano /etc/fstab
   16  mount
   17  df -h
   18  sudo fdisk -l
   19  ls /mnt/TP1
   20  sudo nano /etc/fstab
   21  sudo reboot
   22  ip a ###
   23  cat /proc/partitions
   24  sudo vgextend /dev/group1 /dev/sda6
   25  att
   26  PK ?
   27  att je viens disc
   28  sudo vgextend /dev/group1 /dev/sdd
   29  ip a
   30  sudo apt-get update
   31  sudo apt-get install ssh
   32  sudo reboot
   33  history
```



-- avant montage disque sata


```bash
etudiant@debian-stretch:~$ sudo cat /proc/partitions
major minor  #blocks  name

   8        0    8388608 sda
   8        1    7863296 sda1
   8        2          1 sda2
   8        5     522240 sda5
   8       16    2097152 sdb
   8       32    2097152 sdc
 254        0    3137536 dm-0

```

--- après montage sata :

```bash
etudiant@debian-stretch:~$ cat /proc/partitions
major minor  #blocks  name

   8       32    2097152 sdc
   8       48    8388608 sdd
   8       49    7863296 sdd1
   8       50          1 sdd2
   8       53     522240 sdd5
   8       64    2097152 sde
   8       16    2097152 sdb
   8        0    2097152 sda
   8       80    2097152 sdf
 254        0    3137536 dm-0
```
