Linux Assignment

1. Install SMTP in localhost
Ans. SMTP stands for simple mail transfer protocol. There are several software server. I chose sendmail.
Command: 
$ apt install sendmail
Do you want to continue? [Y/n] y
 $ systemctl status sendmail
Active: active (running)
$ sudo systemctlconfig
Configure sendmail with the existing /etc/mail/sendmail.conf? [Y] y
Configure sendmail with the existing /etc/mail/sendmail.mc? [Y] y
To enable sendmail to use STARTTLS, you need to:
1) Add this line to /etc/mail/sendmail.mc and optionally
   to /etc/mail/submit.mc:
  include(`/etc/mail/tls/starttls.m4')dnl
2) Run sendmailconfig
3) Restart sendmail
Reload the running sendmail now with the new configuration? [Y] y

$ sudo vi /etc/mail/sendmail.mc
$ sudo m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf
bash: /etc/mail/sendmail.cf: Permission denied
$ sudo vi /etc/mail/sendmail.mc
$ sudo chmod a+w /etc/mail/sendmail.cf
$  sudo sh -c 'm4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf'
$ sudo systemctl restart sendmail
$ systemctl status sendmail
Active: activating (start) since Fri 2024-02-09 10:41:15 IST; 56s ago
$ systemctl status sendmail
Active: active (running) since Fri 2024-02-09 10:42:18 IST; 33min ago
$ sendmail -v pooja.yadav@sigmoidanalytics.com
From:Pooja
Subject: This is a test mail
This message is sent to WARNING: local host name (sigmoid) is not qualified; see cf/README: WHO AM I?
$ sudo vi /etc/hosts

[1]+  Stopped                 sudo vi /etc/hosts
$ sendmail -v pooja.yadav@sigmoidanalytics.com
From:Pooja
Subject: This is test mail
This message is to configure sendmail
.


2. Create a user in your localhost, which should not be able to execute the sudo command.
Useradd -m burger
ls -ltr
$ sudo su - burger 
$ pwd /home/burger 
$ 
User got switched but username is not visible in prompt

$ sudo adduser pizza
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for pizza
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] y
$ sudo su – pizza
User got switched with username in prompt.
Both the user don’t have access to execute sudo command.

3.Configure your system in such a way that when a user type and executes a describe command from anywhere of the system it must list all the files and folders of the user's current directory. 
Ex:- $ describe
$ content1 content2
Content3 content 4

Ans : 
$ cd /home /hii 
$ ls -la
$ vi .bashrc
i
Add alias describe=”ls-la”
esc
:wq!
Then when we use command describe from user it will able to list all the content.

4.Users can put a compressed file at any path of the linux file system. The name of the file will be research and the extension will be of compression type, example for gzip type extension will be .gz. You have to find the file and check the compression type and uncompress it. 
 Ans:
$ sudo find / -type f -name "research.gz" -exec gzip -g {} +


5.Configure your system in such a way that any user of your system creates a file then there should not be permission to do any activity in that file. 
Note:- Don’t use the chmod command
Ans. 
$ vi /etc /profile
add umask 077 at the end.
$ touch file_name
$ ls -l | grep file_name
Now the the file created in /etc folder will have – rw------- i.e; only user have permission to read and write whereas, group and others have no permission at all. 
We can also just open a directory and excute umask 077
$ umask 077
then file created in any directory after using this command has restricted access.


6.Create a service with the name showtime , after starting the service, every minute it should print the current time in a file in the user home directory. 
Ex:-
sudo service showtime start -> It should start writing in file.
sudo service showtime stop -> It should stop writing in file.
sudo service showtime status -> It should show status.

$ sudo nano /etc / systemd /system /showtime.sh
#!/bin/bash

FILE="$HOME/showtime.log"

start_showtime() {
    echo "showtime service started." > "$FILE"
    while true; do
        echo "$(date)" >> "$FILE"
        sleep 60
    done
}

stop_showtime() {
    echo "showtime service stopped." >> "$FILE"
    exit 0
}
status_showtime() {
      if [-e "$FILE" ];then
        echo "show time service is running"
      else
        echo "show time service is stopped"
      fi
}
case "$1" in
    start)
        start_showtime &
        ;;
    stop)
        stop_showtime
        ;;
    status)
        status_showtime
        ;;
    *)
        echo "Usage: $0 {start|stop|status}"
        exit 1
        ;;
esac

$ sudo nano /etc/systemd/system/showtime.service

[Unit]
Description=Showtime Service
After=network.target

[Service]
Type=simple
ExecStart=/bin/bash /etc/systemd / system /showtime.sh start
ExecStop=/bin/bash /etc/systemd / system/showtime.sh stop
Restart=always
User=sigmoid
Group=sigmoid

[Install]
WantedBy=default.target

$ sudo systemctl daemon-reload
$ sudo systemctl enable showtime
$ sudo systemctl start showtime
$ cat ~/system.log
showtime service started.
Monday 12 February 2024 02:27:35 PM IST
Monday 12 February 2024 02:30:23 PM IST
$ sudo systemctl stop showtime
showtime service started.
Monday 12 February 2024 02:27:35 PM IST
Monday 12 February 2024 02:30:23 PM IST
Monday 12 February 2024 02:31:23 PM IST
Monday 12 February 2024 02:32:23 PM IST
Monday 12 February 2024 02:33:23 PM IST
Monday 12 February 2024 02:34:23 PM IST
Monday 12 February 2024 02:35:23 PM IST
Monday 12 February 2024 02:36:23 PM IST
showtime service stopped.




           
