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
    sudo lvreduce -L 1G  /dev/vgstockage/lvstockage <<< "y"
#### réduit de 1GB
    sudo lvreduce -L -1G  /dev/vgstockage/lvstockage <<< "y"


## eleves :