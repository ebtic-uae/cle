#CLE installation

This document describes how CLE can be installed in Moodle

##Moodle CLE Plugin Installation

* Download CLE.zip from GitHub at https://github.com/ebtic-uae/cle
* Log into Moodle as Admin user, go to ADMINISTRATION->Site administration->Plugins->Install plugins
* Choose "Assignment/Submission plugin(assignsubmission)" as "Plugin type"
* Upload "CLE.zip"
* Tick "Acknowledgement"
* After CLE plugin was installed, upgrade Moodle database
* Set up CLE settings:
    * Etherpad server location, Etherpad API key
    * Host
    * MySQL user for etherpad, Password, Database

##Etherpad Installation

* For CLE to work, etherpad needs to be installed. 
* Go to http://www.etherpad.org and download etherpad. As etherpad is written in javascript, you will have to have Node       (https://nodejs.org ) installed.
* If you want to use the statistics feature of CLE, etherpad needs to be setup to use MySQL (please refer to the installation documents of ehterpad)
* Once etherpad is installed, take note of
        * the API key (you can find this in the setting.json file)
        * the URL where etherpad is installed
* Also in the settings.json, make sure to enable admin access, as you need this in order to install etherpad plugins
* Once etherpad is up and running, go to the plugins page (etherpad-url/admin/plugins) and install
    * copy_paste_images (this allows to paste pictures into the assignments
    * disable_change_author_name (this ensures that students keep the name they inherit from Moodle
    * webrtc (if you want to have audio/video support).
    * headings (so that students can structure their work better).

##Fun with Pad.php

* CLE in essence is integrating the user management of Moodle and etherpad. The way this integration works with respect to authenticating Moodle users in etherpad is through cookies. Moodle writes the authentication cookie, and etherpad reads it to know that the user is allowed to see the pad. However, cookies can only be read from the same domain. So, if your etherpad server resides on a different domain (or if you use IP numbers to identify your machines), the authentication breaks.
  
* To allow etherpad to live outside the moodle domain, you can employ a trick 
    * setup a webserver on the server that runs the etherpad service
    * find pad.php (it is in the cle.zip) and copy it to the root of said webserver
    * go to the settings page of CLE and change the location of the etherpad server

* Now, moodle will - instead of writing the cookie and then calling the outside etherpad server - call a php file which in turn does nothing but set the cookie and then call etherpad.

##FAQ
* Q - How about security?
* A - Be aware that in case of you using SSL, some browsers take issue if an iframe from a SSL-enabled website points to a non-encrypted site. In other words, if your moodle installation is accessible through SSL, then etherpad should also be SSL enabled. 

* Q - how about etherpad living on a different server?
* A - You can have etherpad live on a different machine - however, due to authentication depending on cookies that are written across moodle and etherpad, the machines either have to live under the same domain (different subnets), or you have to use a trick (see Fun with pad.php)

* Q - How safe are the chats from teachers?
* A - as it is currently implemented, teachers cannot see the text chat. However, if you enable the webrtc plugin in etherpad, they might be able to engage in video chat. This needs to be further investigated

* Q - Can I write parts of the text in word?
* A - Etherpad allows in principle to import, however we do discourage that (or suggest to even disable the feature alltogether), as it deletes all propr text, and it does take away the ability of the teacher to understand which student wrote what.

* Q - Is the statistic that the teacher gets fool proof?
* A - The statistic is based on interactions that happen on the assignment tool, within etherpad. Therefore, if two students decide to work together by sitting next to each other, and one of them is typing, then the system will only "see" the work of the typing student. The teacher needs to use common sense and not forget that machines are not infailable.
