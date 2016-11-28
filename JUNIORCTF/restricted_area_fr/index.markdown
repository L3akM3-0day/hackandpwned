---
layout: default
---
<a href="{{site.baseurl}}/JUNIORCTF/restricted_area">English Version</a>
<h1>Restricted Area</h1>
<h2>Problem</h2>
<p>Vous êtes en essaie en tant qu'admin chez Mystery Shack.
Votre premiere tâche est de definir les permissions d'accès aux informations critique, selon cette matrice
Your first task is to define access permissions to the critical information, according to the matrix.</p>

<p>L'information est stockée sur le disk de stockage(https://yadi.sk/d/WF_evB3DyyaXT). Il y a aussi les infos concernant les utilisateurs système et un programe, qui vérifie si les paramétres sont corrects. Si vous le faite bien selon la Matrice, vous receverez le flags</p>

<h2>Resolution</h2>

<p>L'url https://yadi.sk/d/WF_evB3DyyaXT nous donne un fichier image.bin
Une fois le fichier monté, on peu lister le contenue qui est le suivant :

<pre clase="code">
check_32        lazy_white_cat   silly_red_cat    sly_black_pig
check_64        lazy_white_dog   silly_red_dog    sly_red_cat
lazy_black_cat  lazy_white_pig   silly_red_pig    sly_red_dog
lazy_black_dog  lost+found       silly_white_cat  sly_red_pig
lazy_black_pig  passwd           silly_white_dog  sly_white_cat
lazy_red_cat    silly_black_cat  silly_white_pig  sly_white_dog
lazy_red_dog    silly_black_dog  sly_black_cat    sly_white_pig
lazy_red_pig    silly_black_pig  sly_black_dog
</pre>
Le fichier passwd contient</p>
<pre class="code">
dipper:x:1004:1004:,,,:/home/dipper:/bin/bash
mabel:x:1005:1005:,,,:/home/mabel:/bin/bash
soos:x:1006:1006:,,,:/home/soos:/bin/bash
wendy:x:1007:1007:,,,:/home/wendy:/bin/bash
stan:x:1008:1008:,,,:/home/stan:/bin/bash
</pre>

<p>Nous avons besoin de mettre les permissions de lecture pour les 5 comptes comme spécifiés sur l'image Access Matrix pour avoir le flag</p>
<img src="{{ site.baseurl}}/img/restricted_area.png" alt="restricted_area">

<p>Nous pouvons utiliser un script python pour automatisé chaque column de l'image Acess Matrix pour avoir le flag :
Voici mon script attack.py</p>
<pre class="code">
#!/usr/bin/env python3.4

import subprocess
#Le 1 équivaut à Read et 0 équivaut à -
access={
"1004":"100110010000101101101101001",
"1005":"011010101101001010010010110",
"1006":"101101011010110000001010110",
"1007":"011011100010000001011011011",
"1008":"110101111101010110000100000"
}

#Liste les fichier du dossier en excluant les fichiers check* passwd et lost*
process=subprocess.Popen(["ls","--ignore=check*,"--ignore=passwd","--ignore=lost*"],stdout=subprocess.PIPE)
#stock le resultat dans un tableau
var=process.stdout.read().decode('utf-8').split('\n')
result=[]

for i in access.items():
        for j in range(len(i[1])):
                if i[1][j] == "1":
                        subprocess.call(["chown",i[0],var[j]],stdout=subprocess.PIPE)
        flag=subprocess.Popen(["./check_64"],stdout=subprocess.PIPE)
        for z in var:
                if z is not "":
                        subprocess.Popen(["chown","root",z],stdout=subprocess.PIPE)
        FLAG=flag.stdout.read().decode('utf-8').split('FLAG')[1][2:]
        if i[0] == "1004":
                result.insert(0,FLAG[:8])
        if i[0] == "1005":
                result.insert(1,FLAG[8:16])
        if i[0] == "1006":
                result.insert(2,FLAG[16:24])
        if i[0] == "1007":
                result.insert(3,FLAG[24:32])
        if i[0] == "1008":
                result.insert(4,FLAG[32:40])
print("".join(result))
</pre>

<p>En executant ce script on obtient le flag : 09D240FD704D9DCB60442F6CD3F45E47F0344B0D </p>
<p>Happy Hacking</p>
