https://raw.githubusercontent.com/kenneth558/a/master/Surveillance%20Page%20Main%20Tab.png

# Web Page to play back my Surveillance Recordings, includes client-side alerting and ability to disable system alerts for current weather and such

I had to develop this - a web page and some server-side web server scripting - for personal use.  The cameras in my system are all IP cameras that only output their stream in rtsp, not the newer streaming protocols, so I didn't bother putting any live view elements on this web page.  If your camera live streams come to you as a newer http protocol, just modify this and add some video elements where you want them, and you could thus have live streaming elements in the page. The server runs Ubuntu 20.04 (not included here) and the web server is lightweight bash scripting forked from bashttpd and tricked out (included here). **bashttpd bash script takes the place of Apache or nginx to serve this web page**, so that saves you ("me") the trouble of learning one of those web servers.  Alerts used to be sent by email from the server to as many recipient destinations as desired, which is expected to include SMS/MMS translator[s] for alerting via cell phone messaging.  However, email providers have proven unreliable, so that means has been removed.  Only by turning on the 'Alert checking' while displaying this web page can you now be alerted by camera events.  Removing the email alerting also removed the alerting by email of changes in ip address, unfortunately.

NOTE: My cameras stream files are segmented to 6 minute segments by ffmpeg whose command line is in a startup script (not included).  They are recorded at all times continuously regardless of whether events occur or not.
NOTE: Alerting of events and new ip addresses used to be configured via emails sent out.  For this functionality you'll need to hard code some email account name with password in this scripting, so I removed mine from this public repository.  Please create a dedicated email account for this.  I found the email provider (gmail) not reliable, perhaps due to their rules getting violated when the system alerted continuously for way too many weather events.
NOTE: Most features require server-side customizations to the http server script (included), a functionality similar to PHP or server-side Javascript, so most functionality will be lost if this page is served without using the tricked out bashttpd script for that purpose.  Future work may switch from bashttpd to Tornado web server, however.

Please don't expect this page to radiate impressive beauty, but it does do me a world of good in functionality such as -
  - configure week long predictable "don't wake me up" time windows as files both per camera and system-wide.  Events in the form of .jpg files will continue to be recorded as .jpgs when they happen so you can research later if you missed anything, they just won't produce alerts.
  - play a sound repeatedly when an alert is sent out

There is room for improvement if you want to: the playback synchronizing algorithm only synchronizes the recorded streams to one place - one minute before the beginning of an event you're researching, assuming you've viewed the .jpg of that moment.  As those streams play, they will lose synchronization due to small unpredictable signal acquisition outages and so forth.  Not only that, but you'll need to consider internet security measures.  I use iptables rules for blacklisting and whitelisting (ULTRA-IMPORTANT, I BEST NOT SAY WHY, BUT NOT INCLUDED), rather than login by password.

Instructions: copy all these files to your Ubuntu 20.04 server.  Look at the source code in all files and build the directories referenced, customized as you desire.  Move the index.html into the directory you built for it.  Create the user you want to run this under, considering anyone pulling up the web page will have those same rights.  Assign rights and file permissions.  There is no live stream server for the rtsp live streams, but on my server I use **socat** to buffer the live streams going to off-page rtsp player[s] on clients.  The IP address port that the web page is served for is located in bashttpd as the variable LISTEN_PORT.

Screen shot: https://raw.githubusercontent.com/kenneth558/a/master/Surveillance%20Page%20Main%20Tab.png

