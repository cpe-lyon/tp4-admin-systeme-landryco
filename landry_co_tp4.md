# Exercice 1. Gestion des utilisateurs et des groupe

1. __Commencez par créer deux groupesgroupe1 et groupe2__<br>

`sudo addgroup groupe1; sudo addgroup groupe2`

2. __Créez ensuite 4 utilisateursu1,u2,u3,u4avec la commandeuseradd, en demandant la création deleur dossier personnel et avecbashpour shell__<br>

`sudo useradd u<num> -m -s /bin/bash`

3. __Placez les utilisateurs dans les groupes :-u1, u2, u4dansgroupe1-u2, u3, u4 dans groupe2__<br>

`sudo usermod u<num> -a -G groupe<num>`

4. __Donnez deux moyens d’aﬀicher les membres de groupe2__

`sudo grep 'groupe2' /etc/group` | `sudo `

5. __Faites de groupe1 le groupe propriétaire de /home/u1et/home/u2 et de groupe2 le groupe propriétairede/home/u3et/home/u4__

`sudo chown <user>:<groupe> <dossier>`

6. __Remplacez le groupe primaire des utilisateurs :__<br>
 - groupe1 pour u1 et u2
 - groupe2 pour u3 et u4
 
 `sudo usermod -g <group> <user>`
 
 7. __Créez deux répertoires/home/groupe1et/home/groupe2pour le contenu commun aux groupes, etmettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossierassocié.__
 
 `sudo mkdir groupe1 ; sudo mkdir groupe2`<br>
 `sudo chown -R root:groupe<num> groupe<num>`<br>
 `sudo chmod -R 730 groupe<num>`
 
 8. __Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommerou supprimer ce fichier?__
 
 `sudo chmod +t groupe1`
 `sudo chmod +t groupe2`
 
 9. __Pouvez-vous vous connecter en tant queu1? Pourquoi?__<br>
 
 `Non, nous n'avons pas défini de mot de passe à la création`
 
 10. __Activez le compte de l’utilisateuru1et vérifiez que vous pouvez désormais vous connecter avec soncompte.__<br>
 
 `sudo passwd u1`<br>
 `mot de passe`<br>
 `su u1`<br>
 
 11. __Quels sont l’uid et le gid deu1?__<br>
 
 `grep 'u1' /etc/passwd`<br>
 `u1:x:1001:1001::/home/u1:/bin/bash`
 
 12. __Quel utilisateur a pour uid1003?__<br>
 
 `grep '1003' /etc/passwd`<br>
 `u4:x:1003:1002::/home/u4:/bin/bash`
 
 13. __Quel est l’id du groupegroupe1?__<br>
 
 `grep 'groupe1' /etc/group`<br>
 `groupe1:x:1001:u1,u2,u4`
 
 14. __Quel groupe a pourguid1002? (Rien n’empêche d’avoir un groupe dont lenomserait 1002...)__<br>
 
 `grep ':1002:' /etc/group`<br>
 `groupe2:x:1002:u2,u3,u4`
 
 15. __Retirez l’utilisateuru3du groupegroupe2. Que se passe-t-il? Expliquez__<br>
 
 `gpasswd -d u3 groupe2`
 
 16. __Modifiez le compte de u4 de sorte que :__
 
 - il expire au 1er juin 2019 : `sudo chage -E 18048 u4`
 - il faut changer de mot de passe avant 90 jours : `sudo chage -M 90 u4`
 - il faut attendre 5 jours pour modifier un mot de passe : `sudo chage -m 5 u4`
 - l’utilisateur est averti 14 jours avant l’expiration de son mot de passe : `sudo chage -W 14 u4`
 - le compte sera bloqué 30 jours après expiration du mot de passe : `sudo chage -I 30 u4`
 
 17. __Quel est l’interpréteur de commandes (Shell) de l’utilisateurroot?__<br>
 
 `sudo grep 'root' /etc/passwd`
 `root:x:0:0:root:/root:/bin/bash` (/bin/bash)
 
 18. __à quoi correspond l’utilisateur nobody?__<br>
 
 L'utilisateur nobody est un compte avec un accès hyper restreint. Le compte n'a aucun droit sur aucun fichier.
 
 19. __Par défaut, combien de temps la commandesudoconserve-t-elle votre mot de passe en mémoire?Quelle commande permet de forcersudoà oublier votre mot de passe?__
 
 `man sudo`
 On trouve que le mot de passe est valide pendant 15mins.
 
 # Exercice 2. Gestion des permissions
 
 1. __Dans votre$HOME, créez un dossiertest, et dans ce dossier un fichierfichiercontenant quelqueslignes de texte. Quels sont les droits surtestetfichier?__
 
 Le dossier test possède les droits 775
 Le fichier possède les droits 664
 
 2. __Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher entant queroot. Conclusion?__
 
 
Impossible de l'afficher/modifier en tant que user. En revanche root aura les accès quoi qu'il arrive car le système de droit ne s'applique pas à lui.

3. __Redonnez vous les droits en écriture et exécution surfichierpuis exécutez la commandeecho "echoHello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’unfichier s’il existe déjà. Que peut-on dire au sujet des droits?__

Nous pourrons modifier le contenu du fichier mais pas le visualiser

4. __Essayez d’exécuter le fichier. Est-ce que cela fonctionne? Et avec sudo? Expliquez.__

Non, l'execution est refusé. Avec sudo en revanche il est possible d'executer le fichier même si cela ne fait rien

5. __Placez-vous dans le répertoiretest, et retirez-vous le droit en lecture pour ce répertoire. Listez lecontenu du répertoire, puis exécutez ou aﬀichez le contenu du fichierfichier. Qu’en déduisez-vous?Rétablissez le droit en lecture surtest__

On ne pourra pas lister le contenu du dossier test. En revanche il est possible d'executer le fichier. Le droit de lecture sur un dossier n'impact pas son contenu.

6. __Créez dans test un fichiernouveauainsi qu’un répertoiresstest. Retirez au fichiernouveauet aurépertoiretestle droit en écriture. Tentez de modifier le fichiernouveau. Rétablissez ensuite le droiten écriture au répertoiretest. Tentez de modifier le fichiernouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations?__

Les fichiers n'héritent pas des droits d'un dossier

7. __Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoiretest.Tentez de créer, supprimer, ou modifier un fichier dans le répertoiretest, de vous y déplacer, d’enlister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires?__

Sans droit d'execution, aucune action n'est possible.

8. __Rétablissez le droit en exécution du répertoiretest. Positionnez vous dans ce répertoire et retirez luià nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoiretest, de vous déplacer dansssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence desdroits que l’on possède sur le répertoire courant? Peut-on retourner dans le répertoire parent avec ”cd..”? Pouvez-vous donner une explication?__

Les droits sont dynamiques et s'applique dès leurs modifications. Pour l'exemple, une fois le droit d'execution retiré, plus aucune action n'est possible.

9. __Rétablissez le droit en exécution du répertoiretest. Attribuez au fichierfichierles droits suﬀisantspour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.__

`chmod 640 test`

10. __Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture,ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.__

`umask 077 test`

11. __Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos réper-toires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.__

`umask 022 test`

12. __Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture auxmembres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.__

`umask 037 test`

13. __Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vouspourrez vous aider de la commandestatpour valider vos réponses)__

```chmod u=rx,g=wx,o=r fic = chmod 534 -r-x--wx-r--
chmod uo+w,g-rx fic = chmod 706 -rwx-x-rw-```

14. __Aﬀichez les droits sur le programmepasswd. Que remarquez-vous? En aﬀichant les droits du fichier/etc/passwd, pouvez-vous justifier les permissions sur le programmepasswd?__

```cd /etc 
ls -l passwd    
-rw-r--r-- 1 root root 1801 sept. 30 21:15 /etc/passwd```

Le fichier est accessible par son propriétaire en lecture / ecriture, et est uniquement lisible pour les autres utilisateurs.

Le fichier passwd contient des données extrèmements sensibles pour le système UNIX. Il est donc logique que uniquement root puisse modifier se fichier même si ceci est déconseillé.
