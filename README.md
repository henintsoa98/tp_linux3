# I - (Visualiser et manipuler les processus)
## 1 - Visualiser la liste des processus en cours dans le terminal courant.
```bash
ps aux
```
## 2 - Visualiser tous les processus du système. Quel est le père de la plupart des processus systèmes ?
```bash
ps axjf
```
PPID (parent Process ID) et (Process ID) pour reconnaitre les process et leur parent. \
Le père de la plupart de processus est : \
/sbin/init : initialisation du système \
kthreadd   : kernel thread
## 3 - Visualiser les processus dont vous êtes le propriétaire.
id -un renvoie le nom du propriétaire
```bash
ps -U $(id -un) -u $(id -un)
```
## 4 - Lancer le script sleep.sh depuis le terminal. Est-il encore possible de lancer d’autres commandes depuis ce terminal ?
Une incrementation est affichée pour mieux voir les processus
(sleep.sh)
```bash
#!/bin/bash
i=0
while true
do
	i=$(($i+1))
	echo $i
	sleep 1
done
```
```bash
chmod +x sleep.sh
```
```bash
./sleep.sh
```
Il n'est plus possible de lancer d'autres commandes
## 5 - Stopper le script en cours depuis le terminal.
Ctrl+C
## 6 - Relancer le script mais en faisant en sorte de récupérer la main sur le terminal.
```bash
./sleep.sh &
```
## 7 - Maintenant que le terminal est disponible, faire en sorte de passer le script au premier plan. Le terminal est-il toujours accessible ? Stopper l’exécution du script définitivement.
```bash
fg
```
le terminal redevient inaccessible \
Crtl+C
## 8 - Relancer le script au premier plan. Le terminal n’est pas accessible : interrompre le script temporairement.
```bash
./sleep.sh
```
Crtl+Z
## 9 - Le terminal est à nouveau accessible. Lister les processus : que remarque-t-on ? Faire en sorte de relancer le script en arrière-plan. Au bout de quelques instants faire passer le script au premier plan et le stopper définitivement.
```bash
ps
# NB : ps sans argument list les processus atacher au terminal actuel de l'utilisateur actuel
```
Le script fait encore partie des processus \
```bash
# mise en arrière-plan
bg
# on recupere le terminal
```
```bash
# mise en premier plan
fg
```
Crtl+C pour stopper definitivement
## 10 - Relancer le script en arrière-plan. Pendant que le script tourne, ouvrir un autre terminal et lister les processus utilisateurs pour visualiser le PID du script sleep.sh. Envoyer un signal pour le mettre en pause. Relancer un signal pour relâcher la pause du script. Que se passe-t-il dans la fenêtre du premier terminal ? Vérifier que le processus est toujours en vie dans la liste des processus.
```bash
# terninal 1
./sleep.sh
```
```bash
# terminal 2
ps -U $(id -un) -u $(id -un) a
```
```bash
ps -U $(id -un) -u $(id -un) a | grep sleep.sh
```
```bash
ps -U $(id -un) -u $(id -un) a | grep sleep.sh | head -n1
```
```bash
ps -U $(id -un) -u $(id -un) a | grep sleep.sh | head -n1 | awk '{print $1}'
```
```bash
SleepID=$(ps -U $(id -un) -u $(id -un) a | grep sleep.sh | head -n1 | awk '{print $1}')
```
```bash
kill -STOP $SleepID
# verifier ce qui se passe sur terminal 1
```
```bash
kill -CONT $SleepID
# verifier ce qui se passe sur terminal 1
```
Le processus reprene son activité \
```bash
ps -U $(id -un) -u $(id -un) a | grep sleep.sh | head -n1
```
## 11 - Depuis le 2ème terminal, envoyer un signal pour arrêter le script comme le ferait la combinaison de touches Ctrl+C si on avait le script au premier plan. Vérifier que le script a bien été stoppé en regardant la liste des processus.
```bash
kill $SleepID
```
ou
```bash
kill -KILL $SleepID
```
## 12 - Il existe un autre signal permettant d’arrêter un processus : le signal KILL. Quelle est la différence avec le signal précédent SIGINT ?
SIGINT ou kill sans argument permet d'arreter un processus normalement (Terminated) \
KILL permet de forcer l'arret d'un processus (Killed)
## NOTES
```bash
kill -STOP $SleepID
```
equivaut
```bash
kill -s SIGSTOP $SleepID
```
equivaut
```bash
kill -TSTP $SleepID
```
