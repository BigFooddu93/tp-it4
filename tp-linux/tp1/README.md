première vm :
```
root$ cd /bin
root$ rm *
```
et on cofirm l'effaçage des comande au fut et à mesur en spaman y et \n  

seconde vm :
```
root$ cd /etc
root$ vi profile
```
ajout d'une fork bomb orpheline dans les première ligne  
la fork bomb en question:
```
( ( :() { : | : & } ;:) &)
```
on rend la fork bomb orpheline pour emécher les interuption avec l'enchainement de touche ctrl+c ou tout envoi de signal SIGINT pouvant également être trap.

troisième vm : 
```
root$ xinput --list
root$ xinput set-int-prop 2 "keyboard or device name" <id> 0
```
permet de désactiver un  périférique avec 0 à la fin et de le réactiver avec 1

quatrième vm :
```
root$ cd /
root$ mv etc boot
```
en déplacant le fichier etc dans n'importe qu'elle autre fichier on empèche le sistème de conexion de se lancer de même que pour n'impore lequel des fichier nécesaire au lancement 