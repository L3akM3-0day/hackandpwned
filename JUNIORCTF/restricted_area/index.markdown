---
layout: default
---

<h1>Restricted Area</h1>
<h2>Problem</h2>
<p>You are given a trial run as an admin in the Mystery Shack.
Your first task is to define access permissions to the critical information, according to the matrix.</p>

<p>The information is stored on the special storage disk (https://yadi.sk/d/WF_evB3DyyaXT). There is also information about system users and a program, which checks, if the settings are correct. If you do it right according to the matrix, you`ll receive a flag.</p>

<h2>Resolution</h2>

<p>The url https://yadi.sk/d/WF_evB3DyyaXT give us a zip file which.
there is a file passwd which contain :</p>
<pre class="code">
dipper:x:1004:1004:,,,:/home/dipper:/bin/bash
mabel:x:1005:1005:,,,:/home/mabel:/bin/bash
soos:x:1006:1006:,,,:/home/soos:/bin/bash
wendy:x:1007:1007:,,,:/home/wendy:/bin/bash
stan:x:1008:1008:,,,:/home/stan:/bin/bash
</pre>

We need to set the read permissions for the 3 account as specified to this Access Matrix to get  the flag
<img src="{{ site.baseurl}}/img/restricted_area.png" alt="restricted_area">

We can use a python script to automate each column of the Access Matrix to get the flag :

Here is my attack.py

<code class="code">
#!/usr/bin/env python3.4

import subprocess
access={
"1004":"100110010000101101101101001",
"1005":"011010101101001010010010110",
"1006":"101101011010110000001010110",
"1007":"011011100010000001011011011",
"1008":"110101111101010110000100000"
}

process=subprocess.Popen(["ls","--ignore=check*","--ignore=attack*","--ignore=passwd","--ignore=lost*"],stdout=subprocess.PIPE)
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
<code>
