# xfce
Tout ce qui concerne ce super DE

## refreshing
https://www.makeuseof.com/tag/refresh-linux-desktop-without-rebooting/

# Cloner paramétrage XFCE4
## Tuer processus XFCE
Uniquement si toutes les sessions sont fermées :
```sh
P=$( ps ax -o pid,cmd | grep -i xfce | grep -v "grep " ) ; [[ ! $( echo -e "$P" | grep -v xfconfd ) ]] && for p in $( echo -e "$P" | cut -d ' ' -f 2 ) ; do kill $p ; done
```

## Copier paramétrage
Nota : User Target peut être SKEL pour l'ajouter pour tous les nouveaux utilisateurs.
```sh
US=feve
UT=root

HS=$( [[ $US == root ]] && echo /root || echo /home/$US )
HT=$( [[ $UT == root ]] && echo /root || ( [[ $UT == SKEL ]] \
 && ( mkdir -p /etc/skel/.config ; echo /etc/skel ) || echo /home/$UT ) )
mv "$HT/.config/xfce4" "$HT/.config/xfce4.old"
cp -r "$HS/.config/xfce4" "$HT/.config/"
[[ $UT == SKEL ]] && chown -R root: "$HT/.config/" || chown -R $UT: "$HT/.config/"
```
Nota : je ne sais plus pourquoi mais il faut aussi dans certains cas le dossiers menus.
TODO : clarifier la nécessité du dossier menus

## Si ça ne marche pas
Cette méthode fonctionnera dans tous les cas, mais impose de restaurer les données depuis le profil initial ! De fait, il vaut mieux l'appliquer quand le profil initial a été peu utilisé.

Redémarrer, ne PAS ouvrir de session après.

Copier dans /etc/skel/.config le paramétrage que l'on souhaite appliquer pour tout nouveau utilisateur (voir "copier paramétrage")

Supprimer et recréer l'ancien utilisateur après avoir sauvegardé son profil home.
```sh
root@host:~# mv /home/Y /home/_ancien_Y
root@host:~# deluser Y
root@host:~# adduser Y
```
=> le profil /home/Y est recréé avec le bon paramétrage !

Restaurer les données (documents), profils (Firefox, Thunderbird), configurations (raccourcis, ssh, etc.) depuis l'ancien profil :
```sh
root@host:~# mv /home/_ancien_Y/ /home/Y/
root@host:~# mv /home/_ancien_Y/.mozilla /home/Y/
root@host:~# mv /home/_ancien_Y/.ssh/ /home/Y/
```
