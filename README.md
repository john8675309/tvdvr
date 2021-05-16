# TVDVR
This is an application that allows you to use IPTV streams and DVR them.  
It requires tv_grab_na_dd and a subscription to schedules direct (other XMLTV services may be supported, you will need to hack the script).  

## Reason For Being
We like to watch TV shows on Saturday and Sunday mornings, we don't watch TV at all during the week.  We also don't like to waste a lot of time watching the shows, with the commercials.  
This simple php application allows us to record the shows from something like an hdhomerun or other IPTV provider, strip the commercials, scp to our plex server, watch and get on with our day.

## Required Applications:  
Sqlite3  
PHP  
- Curl  
- PDO  
- Sqlite  

FFmpeg  

Comskip https://github.com/erikkaashoek/Comskip  
- Comcut (edited, included)  
- Comskip ini file (included)  

tv_grab_na_dd (xmltv)  

## Install:  
```
git clone https://github.com/john8675309/tvdvr  
mkdir /videos/tv  
cp -pr tvdvr/* /videos/tv/  
cd /videos/tv  
cp comcut /usr/bin/  
chmod +x /usr/bin/comcut  
cp comskip.ini /etc/  
apt-get install -y autoconf libtool git build-essential libargtable2-dev libavformat-dev libsdl1.2-dev php7.3-curl php7.3-pdo php7.3-sqlite sqlite3 ffmpeg xmltv  
git clone https://github.com/erikkaashoek/Comskip  
cd Comskip  
./autogen.sh  
./configure  
make  
make install
cd /videos/tv/
mkdir {copying,done,downloaded,processfiles,waiting} 
nano tv  
```  
Edit the variables if you followed the above steps probably not much will need to be configured except for where you want to ssh to and the id_rsa key.  

After this next thing to do would be to configure schedules direct: https://schedulesdirect.org/  
you probably shoudn't run this a root so when you run this command do it as a user NOT as root (You have been warned).  
```
tv_grab_na_dd  --configure
```
Edit the tv file again and set the location of the xmltv config file for example: /home/user/.xmltv/tv_grab_na_dd.conf  
create a cronjob for all the required stuff. Do this as your user no more root!  
```
crontab -e
```  
```
30 01 * * * /videos/tv/tv --loadShows --loadDatabase >> /videos/tv/log 2>> /videos/tv/err.txt
* * * * * /videos/tv/tv --checkTime >> /videos/tv/log 2>> /videos/tv/err.txt
* * * * * /videos/tv/tv --convert >> /videos/tv/log 2>> /videos/tv/err.txt
```  
Change permissions on /videos directory.
```
chown -R user:user /videos
```
## Command Line Flags  
Command line flags are run in the order you pass them, for example on the nightly if you run ./tv --loadDatabase --loadShows it will download from Schedules Direct and then find new recordings.  
```
--loadDatabase Download from Schedules Direct save the shows in a guide.xml  
--loadShows Look through all the programs in guide.xml and load any recordings into the database.  
--checkTime Look for any recordings to stop/start and starts and stops them.  
--loadChannels Loads all the channels from a .m3u http server, this will help map Schedules Direct channel to your provider channel  
--fakeShow We all need a way to test! this reads the Global fakeShowUrl (video url) and fakeShowLength (How long to record)  
--convert Convert rip the commercials out and scp the file.  
```

## Variables In The tv file  
```
$GLOBALS['tvgrab']="/usr/bin/tv_grab_na_dd"; #The tv_grab_na_dd script location  
$GLOBALS['grabFile']="/videos/tv/guide.xml"; #The location of the guide.xml after tv_grab_na_dd is run  
$GLOBALS['config']="/home/user/.xmltv/tv_grab_na_dd.conf"; #The Location of the tv_grab_na_dd Config file  
$GLOBALS['recordingDir']="/videos/tv/downloaded/"; #Where the first recording directory is  
$GLOBALS['waitingDir']="/videos/tv/waiting/"; #Waiting to be have the commericials removed.  
$GLOBALS['processDir']="/videos/tv/processfiles/"; #Unused right now.  
$GLOBALS['doneDir']="/videos/tv/done/"; #Where to place the file after commericals have been ripped out and all post processing is done.  
$GLOBALS['m3u'] = ""; #The m3u download location with a list of all your channels.  
$GLOBALS['fakeShowLength']=10; #How many minutes to record our fake testing show.  
$GLOBALS['fakeShowUrl']=""; #The url of a video to record for the fake show.  
$GLOBALS['dbLocation']="/videos/tv/tv.sqlite"; #The Location of the database  
$GLOBALS['comcut']="/usr/bin/comcut"; #Location of the comcut script.  
$GLOBALS['copyDir']="/mnt/TV/"; #The location to scp to  
$GLOBALS['remoteHost']="user@remote"; #Remote server to copy the finished file to.  
$GLOBALS['rsa']="/home/user/.ssh/id_rsa"; #Your Private key you should have the id_rsa.pub contents on the remote machine.  
```

## TODO:
- Right now you have to edit the tv script in order to add a new recording, that's lame, that will be my next fix.  
- Easier way to add shows to the database to record.  
