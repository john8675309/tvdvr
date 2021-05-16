# TVDVR
This is an application that allows you to use IPTV streams and DVR them.  
It requires tv_grab_na_dd and a subscription to schedules direct (other XMLTV services may be supported, you will need to hack the script)  

## Required Applications:  
Sqlite3  
PHP  
-curl  
-pdo  
-sqlite  
FFmpeg  
comcut (included)  
comcut ini file (included)  
comskip https://github.com/erikkaashoek/Comskip  
tv_grab_na_dd  

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
edit the variables if you followed the above steps probably not much will need to be configured except for where you want to ssh to and the id_rsa key  

After this last thing to do would be to configure schedules direct: https://schedulesdirect.org/  
you probably shoudn't run this a root so when you run this command do it as a user NOT as root (You have been warned)  
```
tv_grab_na_dd  --configure
```
edit the tv file again and set the location of the xmltv config file for example: /home/user/.xmltv/tv_grab_na_dd.conf  
create a cronjob for all the required stuff. Do this as your user no more root!  
```
crontab -e
```  
```
30 01 * * * /videos/tv/tv --loadShows --loadDatabase >> /videos/tv/log 2>> /videos/tv/err.txt
* * * * * /videos/tv/tv --checkTime >> /videos/tv/log 2>> /videos/tv/err.txt
* * * * * /videos/tv/tv --convert >> /videos/tv/log 2>> /videos/tv/err.txt
```  
Lastly change permissions on /videos directory
```
chown -R user:user /videos
```

## TODO:
- Right now you have to edit the tv script in order to add a new recording, that's lame, that will be my next fix.  
- Easier way to add shows to the database to record.  
