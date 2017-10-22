Documentation Week 2 Homework:
EASY WINS: CREATE SUMMARY REPORT

Define the problem (what do you want to report): There is a new junior engineer in my company that is prepping IOS devices for upgrades.  I would like an automated way for me to check on his work (so we don't have a bunch of devices go down).  Mostly, I would like to check the current version of the device and how he set the boot variable (bootvar) so there isn't any issues.

Figure out how to get the information from the network devices: Ansible to pull information.  The only way I know how to get the bootvar (show bootvar) is via the ios_commands module, but I would like to look into NAPALM and ntc-ansible at a later time.  Some information about current version can be pulled from snmp_facts or ios_facts, but I chose ios_facts.
	
Define the report format: The remote will be somewhat similar to the uptime example except have two entries per devices (one regarding current image location and another with the image in bootvar).  I wanted to start simple, but would like to do some more formatting in the future to fancy this up (possibly a jinja2 tempalte or export to HTML like the uptime example).

Create and test the playbook that collects the information and generates the report: The playbook used is bootvarFinal.yml and it puts the result in /results/bootvar.log.  Here is an example of the playbook being run:

jwilkes@ubuntu:~/NETWORKAUTOMATIONSOLUTIONS/EasyWins (HWweek2)*$ ansible-playbook bootvarFinal.yml 

PLAY [Collect Show Bootvar info from all Devices and save to file] *******************************************************

TASK [file] **************************************************************************************************************
ok: [R1]

TASK [ios_command] *******************************************************************************************************
ok: [R2]
ok: [R1]
ok: [R3]

TASK [ios_facts] *********************************************************************************************************
ok: [R2]
ok: [R1]
ok: [R3]

TASK [lineinfile] ********************************************************************************************************
changed: [R2]
changed: [R1]
changed: [R3]

PLAY RECAP ***************************************************************************************************************
R1                         : ok=4    changed=1    unreachable=0    failed=0   
R2                         : ok=3    changed=1    unreachable=0    failed=0   
R3                         : ok=3    changed=1    unreachable=0    failed=0   

jwilkes@ubuntu:~/NETWORKAUTOMATIONSOLUTIONS/EasyWins (HWweek2)*$ cat results/bootvar.log
R2                   [u'BOOT variable = bootflash:/IM-AN-IDIOT.bin,12;']
R2                   bootflash:packages.conf

R1                   [u'BOOT variable = bootflash:/csr1000v-universalk9.03.16.06.S.155-3.S6-ext.SPA.bin,12;']
R1                   bootflash:/csr1000v-universalk9.03.16.06.S.155-3.S6-ext.SPA.bin

R3                   [u'BOOT variable = bootflash:/csr1000v-universalk9.03.16.06.S.155-3.S6-ext.SPA.bin,12;']
R3                   bootflash:/csr1000v-universalk9.03.16.06.S.155-3.S6-ext.SPA.bin


Write documentation:
This was an effort to produce something that could be useable, but stay as simple as possible.  The following (amoung others) are areas that I could see improvement needed:
-As in the uptime example, separate the playbook into three of more (main playbook referencing others, playbook that collects information, playbook that processes data into report).
-Other means of connecting current boot variable data other than ios_commands (possibly napalm-ansible or ntc-ansible).
-Better formatting of report: one line per device, show only necessary information (just 'bootflash:/<file>'), labels to show "current image" and "bootvar image", export into HTML or other format, ways to compare what was outputted compared to what I was expecting
-I see this being as a first step for setting up playbooks to do devices upgrades.
