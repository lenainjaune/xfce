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
