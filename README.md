# TVDVR
This is an application that allows you to use IPTV streams and DVR them.

It requires tv_grab_na_dd and a subscription to schedules direct (other XMLTV services may be supported, you will need to hack the script)

Sqlite3

PHP
-curl

-pdo

-sqlite

FFmpeg

comcut (included)

comcut ini file (included)

comskip https://github.com/erikkaashoek/Comskip


Install: 
git clone https://github.com/john8675309/tvdvr

mkdir /videos/tv
cp -pr tvdvr/* /videos/tv/

cd /videos/tv

cp comcut /usr/bin/

chmod +x /usr/bin/comcut

cp comskip.ini /etc/


apt-get install -y autoconf libtool git build-essential libargtable2-dev libavformat-dev libsdl1.2-dev php7.3-curl php7.3-pdo php7.3-sqlite sqlite3 ffmpeg

git clone https://github.com/erikkaashoek/Comskip

cd Comskip

./autogen.sh

./configure

make

make install

nano tv

edit the variables if you followed the above steps probably not much will need to be configured except for where you want to ssh to and the id_rsa key

