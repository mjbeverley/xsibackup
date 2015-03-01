XSIBACKUP 4.2.3 Automated Backups for ESXi 5.1 and above (5.X & 6.X series).

 Copyright (C) 2013-2015  33HOPS, Sistemas de Información y Redes, S.L.

	You are allowed to use this software for personal or 
	commercial use. You are allowed to redistribute it
	without any modification. You can modify it's source 
	code freely just as long as you do not redistribute 
	the modified source code.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY. USE AT YOUR OWN RISK.


ONE-LINER INSTALL (recommended):

# wget http://33hops.com/downloads/xsibackup.zip -O xsibackup.zip;unzip -o xsibackup.zip;chmod 0700 xsibackup*

Change directory (cd) to the desired install directory, i.e.: # cd /vmfs/volumes/datastore1
Cut and paste the above one-liner to install XSIBackup in the current
working directory.

IMPORTANT:

Since version 4.2.3 XSIBackup will change its license model to no-license
and will be hosted at github.com: https://github.com/danjfk/xsibackup
All international copyright laws will be applied by default.

Q: How does this affect me?
A: Most probably it will not affect you in any way.

Q: What can I do with XSIBackup?.
A: You can download it and use it freely for personal or bussiness use.
You can change it's source code and use the resulting code. You can 
redistribute it freely in an unmodified form.

Q: What can't I do with XSIBackup?.
A: You can't modify it's source code and redistribute the resulting code.

Q: What happens with the older versions available at sourceforge.net
A: The last GNU v3 version will be kept at http://sourceforge.net/projects/xsibackup
for some time.

About SSH keys:

Link to your secondary server --link-srv=[second.ESXi.IP]
if you want to syncronize over IP to a secondary ESXi box and do 
not forget you have to rerun this command on every reboot as the
ESXi SSH keys are changed on every reboot.

About rsync transfers:

Delta algorithm can be slow on big files, in fact from an efficiency
point of view its only a way to change bandwidth need for CPU cicles. 
If you have a gigabit connection over a LAN it might be faster to 
transfer the whole files than the incremental info through delta 
algorithm. The word "might" means this will depend on your CPU, 
memory and file sizes.

Please read this article to understand the pros and cons: 

http://33hops.com/blog_xsibackup-rsync-considerations.html

The argument --backup-point keeps its syntax unchanged.

[IP or URL of remote host]:[port]:[/path/in/remote/host]:[Transfer method (F|D)]

If you receive any error and want to contact us please provide the 
argument list as parsed to XSIBackup as well as the output of the
program otherwise we cannot figure out what's going wrong.

XSIBackup is compatible with ESXi 5.1.0 and ESXi 5.5.0. It is not 
compatible with versions prior to 5.1, mainly becouse of the differences 
between the bash interpreters bundled with ESXi. You can always try to 
fix it but at 33HOPS we thought it wasn't worth the time as it's a 
lot easier to move to 5.1 from any older ESXi version. 

You can use any character in the variable values with the only exception 
of the equal sign (=) and doublequote ("). If you use an equal sign in a 
variable value the variable will be truncated at this symbol and if you 
use a doublequote you will get this error:

root# -sh: syntax error: bad function name

ADVICE:

(*)Before naming your virtual machines and directory structure 
ask yourself if you really need them to have interstitial spaces as it will 
normally not give you any advantage but can give you lots of headaches at the 
time of working. The most advisable way of naming your VMs is with a fixed 
lenth code where every part of the code has its meaning, per instance WX001, 
W7002, LR003 standing for Windows XP #1, Windows 7 #2 and Linux Red Hat #3 
respectively. This way you can programatically process the VM names in case 
you need it in a much more convenient way. 

FACTS:

XSIBackup will delete/ commit all snapshots before making a backup. Deleting 
a snapshot does not delete information as all data in the snapshot is commited
to the base .vmdk disks. In any case take it on account in case you want to
preserve an existing snapshot and back it up.

In order to allow outgoing e-mail comunicacions through the configured SMTP 
port XSIBackup will open the firewall by adding a new service named SMTPout 
and an associated outgoing rule. This is far more convenient than having to 
open it up manually and does not affect security as the port is opened just 
before sending the e-mails and is closed right after.

You have to copy xsibackup files to a persistent path in your system like: 

/vmfs/volumes/datastore1 

Also do make sure they have the necesary rights by issuing the following 
command from the same directory

chmod 0700 xsibackup*

Then you can execute xsibackup it by issuing

./xsibackup from the same directory

Or

/full/path/to/xsibackup

Of course if you execute it without any argument it will simply print 
out the help.

DESCRIPTION

With ©XSIBackup you can choose to carry out a "hot backup" (while the 
virtual machines are running) or a "cold backup" (switches off every 
virtual machine before copying it) by setting the option 
--backup-how=hot(default)|cold. 

If you choose "cold" © XSIBackup will issue a shutdown to the virtual
machine instance and wait 30 seconds for the virtual machine to be 
shutdown cleanly. If after the initial 30 seconds period the VM 
continues to be on the program will wait 30 more seconds checking the 
VM state every 10 seconds after which © XSIBackup will consider the 
virtual machine can't be shutdown and it will be powered off. If the 
VM does not have © VMware Tools installed on it © XSIBackup will simply 
power it off.

Please note that when a VM can't be shutdown cleanly most of the times 
it is a program or service that keeps it from being shut down the right 
way. Powering off is the last recurse for © XSIBackup to be able to turn 
off the VM and back it up. If you see VMs that were powered off before 
backup in the reports please check the operating system and fix whatever
is wrong (of course check that you have the last version of VMware Tools 
installed in every VM), most of the times nothing serious will happen but 
in certain circumstances you could lose data by forcing a power off.


MANUAL

INSTALL:

First of all you need to allow SSH connections to your ESXi box, you can 
acomplish this by using the vSphere Client. Click on the server node and 
go to the Configuration tab, then look for the Security Profile entry in 
the left side menu under the "Software" heading and click on it. You will 
see the firewall entries on the right. Click on the Properties link on the 
right and checkmark the SSH server and SSH client entries, then reboot the 
server. 

Once you have reboot the server simply copy xsibackup file to your 
/vmfs/volumes/datastore1 folder by using any SSH/SCP client. You can use 
WinSCP at http://winscp.net/eng/download.php or any Linux/Unix command line 
like this (from the same directory where your downloaded xsibackup file is) 
linux# scp xsibackup root@[yor esxi IP]:/vmfs/volumes/datastore1

The easy way:

Cut & paste the following line in your ESXi command line and press enter. 
You can get this URL by clicking the right mouse button and choosing 
"copy link url" in many browsers.

wget http://sourceforge.net/projects/xsibackup/files/xsibackup_4.2.3.zip/download -O xsibackup.zip

This will download version 4.0.2 to the file xsibackup.zip in your local 
directory structure. You can then unzip it

unzip xsibackup.zip

Asign the apropiate permissions

root# chmod 0700 xsibackup

And do a quick test by executing the script without any arguments

root# ./xsibackup

If you see the help then everything went right and you can start using 
xsibackup.

USAGE:

Example 1 (check the process and the e-mail submission before a hot backup):
xsibackup --backup-point=/vmfs/volumes/backup --backup-type=running 
--mail-from=email.sender@yourdomain.com --mail-to=email.recipient@anotherdomain.com 
--smtp-srv=smtp.yourdomain.com --smtp-port=25 --smtp-usr=username 
--smtp-pwd=password --test-mode=true

Example 2 (backup all running VMs while on, --backup-how parameter is omited 
as hot backup is the default):
xsibackup --backup-point=/vmfs/volumes/backup --backup-type=running 
--mail-from=email.sender@yourdomain.com --mail-to=email.recipient@anotherdomain.com 
--smtp-srv=smtp.33hops.com --smtp-port=25 --smtp-usr=username --smtp-pwd=password

Example 3 (cold backup 2 given VMs excluding two disks from LINUXVM2):
xsibackup --backup-point=/vmfs/volumes/backup --backup-how=cold --backup-type=custom 
--backup-vms="WINDOWSVM1,LINUXVM2!scsi1:0;disk2" --mail-from=email.sender@yourdomain.com 
--mail-to=email.recipient@anotherdomain.com --smtp-srv=smtp.yourdomain.com:25 
--smtp-port=25 --smtp-usr=username --smtp-pwd=password

Example 4 (hot backup 3 given VMs):
xsibackup --backup-point=/vmfs/volumes/backup --backup-type=custom 
--backup-vms="WINDOWSVM1,LINUXVM2,New Virtual Machine" --mail-from=email.sender@yourdomain.com 
--mail-to=email.recipient@anotherdomain.com --smtp-srv=smtp.yourdomain.com:25 
--smtp-port=25 --smtp-usr=username --smtp-pwd=password

Example 5 (hot backup 3 given VMs mirrored to second ESXi box usind the Delta Algorithm):
xsibackup --backup-point="192.168.3.33:22:/vmfs/volumes/datastore2:D" --backup-type=custom 
--backup-vms="WINDOWSVM1,LINUXVM2,New Virtual Machine" --mail-from=email.sender@yourdomain.com 
--mail-to=email.recipient@anotherdomain.com --smtp-srv=smtp.yourdomain.com:25 
--smtp-port=25 --smtp-usr=username --smtp-pwd=password

OPTIONS:

--install-cron

This will install the cron system and create the crontab at 
/vmfs/volumes/datastore1/xsibackup-cron. 
You can add as many XSIBackup commands as you want into this file,
one per line. The only thing you have to do is add the parameter --time, 
i.e. --time="Mon 23:30". A log file will hold all the XSIBackup output 
at /vmfs/volumes/datastore1/xsibackup-cron.log. If you want a backup to 
be scheduled every day you need 7 lines into the crontab (xsibackup-cron), 
one per day. You can combine lines however you want thus having full 
flexibility. You can uninstall very easyly by running --install-cron again.

Cron line example: 

/vmfs/volumes/datastore1/xsibackup --time="Fri 04:22" 
--backup-point=/vmfs/volumes/backup --backup-type=running 
--mail-from=email.sender@yourdomain.com 
--mail-to=email.recipient@anotherdomain.com --smtp-srv=smtp.yourdomain.com 
--smtp-port=25 --smtp-usr=username --smtp-pwd=password

--backup-point		

A) Full path to the backup mount point in the local server, it will 
tipically be under /vmfs/volumes, i.e. /vmfs/volumes/backup.

B) Full path in a remote ESXi host by using the following syntax:
--backup-point="IP.OF.REMOTE.SERVER:PORT:/full/path/todatastore"
--backup-point="IP.OF.REMOTE.SERVER:PORT:/full/path/todatastore:METHOD(F,D)"
Example: --backup-point="192.168.1.200:22:/vmfs/volumes/datastore2:F
METHOD (F,D): (F)ull or (D)elta. If F is chosen all bits will be copied, if D is
chosen only differential bits will be copied but can take longer under certain
circumstances. Its up to you to decide wich one is best for you. D is default.
You need to previously link the remote server to this host by using --link-srv option.

--backup-how (hot | cold)

Hot backup is the default method, it makes a backup while the VM is on. 
You can chose to make a cold backup and the VM will be cleanly shutdown 
befor backup and switched on right after.

--backup-type (custom | all | running)

Custom: if this methos is chosen then a list of the VMs to backup must 
be passed to the --backup-vms option. 
All: backup -all- VMS. 
Running: backup only running virtual machines.

--backup-vms

List of virtual machines to backup as a comma separated list, i.e: 
--backup-vms=VM1,VM2!disk1;disk2,VM3
You can exclude disks by adding an exclamation sign [!] followed 
by a list of disks delimited by a semicolon [;] as in the example
above. You can use any string, full or partial, that may help 
identify the disk by its name or by its device descriptor. 
Take on account that if you use an ambiguous string per instance 
"scsi", more than one disk may be excluded. This parameter is only 
needed if "custom" is selected as the --backup-type
NOTE: do remember to quote this string if it contains spaces.

--mail-from

E-mail address as from where the HTML e-mail report will be sent.

--mail-to

E-mail address to which the HTML e-mail report will be sent.

--subject

Subject of the email to send, per instance --subject="Your very own subject"

--smtp-srv

SMTP server that we will use to send the HTML e-mail report through.

--smtp-port

SMTP server port used for communication

--smtp-usr

SMTP username we will use in the plain text SMTP authentication. Please note 
this is the only authentication method supported by esxbackup by now.

--smtp-pwd

SMTP password used for authentication against the SMTP server.

--test-mode=true

Perform a general test without backing up the selected virtual machines. 
You can try your configuration and the e-mail receipt before putting it 
into production.

--link-srv		

This command needs an argument like this --link-srv=192.168.0.100 
It generates a DSA key locally and adds it to the authorized_keys 
file at the remote host allowing to communicate without a password.
