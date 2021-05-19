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
> Virtualbox il donne l'illusion, dou le fait que cest possible mais des quon ecris les données dès quil est plein.  
> il va lever l'exception avec un msg derreur, ou message système "Freenas: permission non accordée"   

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
