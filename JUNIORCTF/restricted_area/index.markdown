---
layout: default
---

<h1>Restricted Area</h1>
<h2>Problem</h2>
You are given a trial run as an admin in the Mystery Shack.
Your first task is to define access permissions to the critical information, according to the matrix.

The information is stored on the special storage disk (https://yadi.sk/d/WF_evB3DyyaXT). There is also information about system users and a program, which checks, if the settings are correct. If you do it right according to the matrix, you`ll receive a flag.

<h2>Resolution</h2>

The url https://yadi.sk/d/WF_evB3DyyaXT give us a zip file which contain file.

there is a file passwd which contain :
<pre class="code">
dipper:x:1004:1004:,,,:/home/dipper:/bin/bash
mabel:x:1005:1005:,,,:/home/mabel:/bin/bash
soos:x:1006:1006:,,,:/home/soos:/bin/bash
wendy:x:1007:1007:,,,:/home/wendy:/bin/bash
stan:x:1008:1008:,,,:/home/stan:/bin/bash
</pre>
