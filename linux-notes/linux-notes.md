**‘The Complete Linux Course: Beginner to Power User\!’**  
**Notes**  
Youtube  
14/10/23 										       Saturday

Linux is open source, it has many distributions, most use “Debian” as their base. Ubuntu also uses Debian as its base. 

**Downloading Ubuntu:**   
On Ubuntu's website, we can download it by going to the downloads tab. It has two versions:   
1\. Ubuntu LTS (Long Term Support) \- Good for businesses that need to use Linux servers since, LTS only needs to be updated once in 5 years, although a new version of Ubuntu LTS is released once every 2 years.   
2\. Ubuntu \- Gets security and maintenance updates every 9 months, so you’ll have to update every 9 months. Good for individual/home/personal use.

**Customizing Ubuntu/Linux Desktop:**  
Desktop themes, Icon themes, Cursors, etc. on gnome-look.org.

**Install “Unity Tweak Tool”:**   
*$ sudo apt-get install unity-tweak-tool*  
\[$ sudo apt-get install \*software\_name\*\] is the basic command to install any software through the terminal in ubuntu.

**Ubuntu Command Line/ Terminal Essentials:**  
Terminal Shortcut: Ctrl \+ Alt \+ T

1. *pwd* : (print working directory \[wd\]) Prints the current directory that we’re working in. 

2. *cd* : (change directory) changes the current directory/ modifies ‘wd’.

Using a “/” before typing the name of the directory triggers the terminal to use an absolute path, and then you’ll have to provide the complete absolute path of the directory you want to travel to.  
Removing the “/” before typing the name triggers the terminal to use a relative path, and the typed directory will only be accessed if it's present in the current working directory as a subfolder.   
Eg: *cd /home/paarth\_pandey* sets the wd to “paarth\_pandey”  
*cd paarth\_pandey* sets the wd to “paarth\_pandey” using relative path (current wd needs to be /home)  
Using a “./” means the current directory, and using a “\~” takes you back to your home directory which is the default directory your terminal starts with.  *cd \~* \= *cd /home/paarth\_pandey*  
“../” means taking the current directory up a level.

3. *clear* : Clears the terminal.

4. *ls* : Lists the content of the current wd in alphabetical order. 

*ls \-l* : prints the content of the current wd in alphabetical order and in “long form”, i.e. it also lists out the permissions of the file, who is able to make changes to what file, the first column after s.no. is the owner of the file, the second column is groups  
*ls \-r* : prints the content of the currency wd in reverse alphabetical order, in normal form.  
*ls \-s* : sorts the current wd with respect to size, and prints it  
*ls /Documents/’General Work’/Wallpapers* : lists the content of the specified directory using the given path without leaving the current wd.  
*ls –help* : prints all the options for running ‘ls’ in terminal

5. *\!\!* : These mean “run the previous command”/”Tell the previous command”

   
**Administrative Privileges in Terminal:** 

1. *sudo* : Stands for “Super User Do”, where “super user” just checks your permissions to be able to perform an action.

2. *nano* : Opens specified file in a special editor where you can view its content in text format, and edit it, provided you have the necessary permission to do so. 

Eg: *nano ./file* : Runs file named “file” in nano, but if you don’t have the permission to edit it, you’ll have to open it using “sudo”, to enable your administrative privileges. *sudo nano ./file*

3. *su* : Stands for “Switch User”, which allows you to change users.

*$ sudo su* : Allows you to take control as “root” user, which has 100% administrative power over anything belonging to the computer.  
\==================================================================================  
15/10/23										           Sunday

**Using the package manager of Ubuntu:**  
The package manager allows you to modify the current packages that you have installed, and install new ones.  
*apt-get* is the package manager command  
*apt-get* has the following access methods:  
    
*update* : Retrieve new lists of packages  
  *upgrade* : Perform an upgrade  
  *install* : Install new packages (pkg is libc6 not libc6.deb)  
  *reinstall* : Reinstall packages (pkg is libc6 not libc6.deb)  
  *remove* : Remove packages  
  *purge* : Remove packages and config files  
  *autoremove* : Remove automatically all unused packages  
  *dist*\-*upgrade* : Distribution upgrade, see apt-get(8)  
  *dselect*\-*upgrade* : Follow dselect selections  
  *build*\-*dep* : Configure build-dependencies for source packages  
  *satisfy* : Satisfy dependency strings  
  *clean* : Erase downloaded archive files  
  *autoclean* : Erase old downloaded archive files  
  *check* : Verify that there are no broken dependencies  
  *source* : Download source archives  
  *download* : Download the binary package into the current directory  
  *changelog* : Download and display the changelog for the given packages  
Repositories of Ubuntu: These packages are available on ubuntu’s servers as repositories, and they each have an index, by which we can access and install them.

**Searching through repositories to find new apps in Ubuntu:**  
*$ apt-cache search bluefish\** : Searches the repository for anything that has the word “bluefish” in it.

*$ apt-cache search gimp\** : Searches the repository for anything that has the word “gimp” in it.

*$ apt-cache policy gimp* : Checks whether the program specified after “policy” is installed in the system, and tells its version.

**Installing programs that are not in the repository:**  
First you’ll need to get the .deb file of the program, then go over to that directory in your terminal where the program’s .deb file is located.   
Then run the following command:

*$ sudo dpkg \-i ./google-chrome-stable\_current-amd64.deb*

The “dpkg” refers to the download package, the ‘-i’ refers to ‘install’, the “./” refers to “look in current directory”, which is finally followed by the package name.

**Updating programs, whose updated packages have been pushed into the repository:**

*$ sudo apt-get upgrade* : This is automatically gonna find and update the programs.

**File Permissions & Ownerships:**

Ownership of file:  
As group:

*$ sudo nano file.txt* : Creates a text file called “file” in the current wd.  
*chown* : Stands for ‘change ownership’, in which a ‘group’ is also given access to the file, along with the owner.  
*$ sudo chown* ‘user:group’ ‘name of file’ \= iis the syntax to change permission of file from user to group  
Example:   
*$ sudo chown root:paarth\_pandey file.txt*

As owner:  
*$sudo chown paarth\_pandey:paarth\_pandey* *file.txt* : Transfers the ownership of the file to paarth\_pandey, of the file ‘file.txt’, which originally was owned by ‘root’

Ownership of all files in a directory:  
To access all files in a directory to make any sort of change to them, be it virus scanning, renaming, or transferring ownership, we use the ‘*\-R’* command, which means recursive, which means it’s gonna repeat for each file in that directory, until all our acted upon, pretty much like a ‘for loop’ in python.

*$ sudo chown \-R root:paarth\_pandey ./mydir* : Changes all the files, in a directory called ‘my\_dir’, as ownership of the user ‘paarth\_pandey’.

Permission of file:  
There are three columns in the start (LHS) when you print the list of directory in long form. It looks like this for eg:

\-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-  
\-rw-rw-r-- 1 paarth\_pandey paarth\_pandey    0 Oct 11 19:37 cap\_%d  
drwxr-xr-x 2 paarth\_pandey paarth\_pandey 4096 Oct 10 16:48 Desktop  
drwxr-xr-x 5 paarth\_pandey paarth\_pandey 4096 Oct 10 17:31 Documents  
drwxr-xr-x 2 paarth\_pandey paarth\_pandey 4096 Oct 15 09:17 Downloads  
**\-rw-r--r--** 1 root          paarth\_pandey    6 Oct 15 15:24 **file.txt**  
drwxr-xr-x 2 paarth\_pandey paarth\_pandey 4096 Oct 10 16:48 Music  
drwxr-xr-x 2 paarth\_pandey paarth\_pandey 4096 Oct 10 16:48 Pictures  
drwxr-xr-x 2 paarth\_pandey paarth\_pandey 4096 Oct 10 16:48 Public  
drwx------ 7 paarth\_pandey paarth\_pandey 4096 Oct 10 22:19 snap  
drwxr-xr-x 2 paarth\_pandey paarth\_pandey 4096 Oct 10 16:48 Templates  
drwxr-xr-x 2 paarth\_pandey paarth\_pandey 4096 Oct 10 16:48 Videos  
\-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-  
The first highlighted column is the permissions of the owner, which in this case are ‘read & write’.  
The second highlighted column is the permissions of the group, which in this case are ‘read only’  
The third highlighted column is the permissions of the public, which in this case are ‘read only’

d \= tells its a directory  
r \= readable (Opening the directory/file)  
w \= writable (Making changes to the directory/file)  
x \= executable (Opening the file/Changing as the directory using ‘cd’)

The code for **‘read & write’** is **‘6’**, and the code for **‘read only’** is **4**

To change file permissions of the file, ‘file.txt’, for the group ‘paarth\_pandey’, we need to use the following command.

*chmod* : Changes the file modification permissions for users and groups.

*$ sudo chmod 646 file.txt* : Changes the \-rw \-r– r– to \-rw \-r– rw-, which allows the public to read as well as write to the file, ‘file.txt’.

**Removing a file or directory:**  
*rm* : Stands for “remove”, which removes the specified file from the directory.  
*$ rm file.txt* : Will remove file.txt, if you have permission to do so, otherwise, you’ll need to run it with administrative privileges, as ‘sudo’.  
*$ sudo rm file.txt* : Removing file with administrative privileges.

*$ rm ./\** : Removes everything in the directory, leaving the directory intact.

\-*rf* : Adding this after rm, removes the directory.   
*$ rm \-rf mydir* : Removes mydir.

Removing all files of the same type in a directory:   
*$ rm mydir/\*.txt* : Removes all text files from the directory called “mydir”

   
**Making a directory:**   
*mkdir* : Stands for “make directory”, allows you to create a new directory. You can create it as admin or user.   
*$ sudo mkdir my\_directory* : Makes a new directory called “my\_directory”, using administrative privileges.

**Creating new files:**  
*touch* : Used for creating new files, followed by file names and file extensions.  
*$ touch file1.txt file2.c file3.py file4.jpg*

**Copying and moving files:**  
*cp* : Stands for copy.   
*$ cp file1 ./mydir/file2.txt* : Copies file, in my dir, as “file2.txt”

*mv* : Stands for move.  
*$ mv file1 ./mydir/file2.txt* : Moves file, in my dir, as “file2.txt”

**Renaming a file:**  
Renaming a file in linux is basically just moving it with a new name.  
Eg:  
*$ mv file.txt filenew.txt* : Moves the file called “file.txt” into the same directory, but with a new name, “filenew.txt”

**FIND Command:**  
*find* : Used for searching between files and directories, just like Ctrl \+ F.   
Syntax: *$ find . \-type f \-name “file.txt”* **OR** *$ find ./mydir \-type f \-name “\*.py”*  
The first command, searches within the current working directory (“.”), specifies the type of thing to search as file (“-type f”), and searches for the thing with name file.txt (“-name “file.txt””)  
The second command, searches within the ‘mydir’ directory, specifies the type of thing to search as file (“-type f”), and searches for every file using the wildcard “\*” that have “.py” in them. (“-name “\*.py””)

Ignoring case sensitivity during file search: *\-iname* : Replace ‘-name’ with this, to ignore case   
sensitivity during file search.

Searching all files with common name, but uncommon extensions : *\-iname “file\*”*

Searching with permissions criteria: *$ find \-type f/d* (f stands for files, d stands for directories, you have to choose either one of them) *\-perm 0664* : Scans for files with permissions as 0664 i.e. read write, read write, and read permission.

Searching with file size as criteria: *$ find . \-type f \-size \+100k* : Searches for all files which are over a 100 kb. 

Searching for files that are not a name criteria: *$ find . \-type f \-not \-name “\*.php”* : Searches for all files that are not php. 

The find command is recursive, meaning it goes on for every file until the end. If you want to limit your search to max 1 subfolder, or 2 subfolder, or only in the specified directory, and not inside any subdirectory, we can do that by using *maxdepth*.  
*$ find /etc \-maxdepth 1 \-type f \-iname “\*.conf”* : Searches for all files inside the directory called “etc”, that end with extension " .conf”, but does not go inside any of the sub-directories of the main directory, etc.   
\==================================================================================  
20/10/23 											Friday

**Uncomplicated Firewall (3rd party app) usage:**  
ufw can be accessed by typing *ufw* as *sudo*   
*$ sudo ufw*

Usage: ufw COMMAND

Commands:  
 enable                          enables the firewall  
 disable                         disables the firewall  
 default ARG                     set default policy  
 logging LEVEL                   set logging to LEVEL  
 allow ARGS                      add allow rule  
 deny ARGS                       add deny rule  
 reject ARGS                     add reject rule  
 limit ARGS                      add limit rule  
 delete RULE|NUM                 delete RULE  
 insert NUM RULE                 insert RULE at NUM  
 prepend RULE                    prepend RULE  
 route RULE                      add route RULE  
 route delete RULE|NUM           delete route RULE  
 route insert NUM RULE           insert route RULE at NUM  
 reload                          reload firewall  
 reset                           reset firewall  
 status                          show firewall status  
 status numbered                 show firewall status as numbered list of RULES  
 status verbose                  show verbose firewall status  
 show ARG                        show firewall report  
 version                         display version information

Application profile commands:  
 app list                        list application profiles  
 app info PROFILE                show information on PROFILE  
 app update PROFILE              update PROFILE  
 app default ARG                 set default application policy

**GREP Command:**  
*grep* : is a command used to search for items inside the files. So *find* is used to search the files within the system, and *grep* is used for searching within the files.

*$ grep “string\_to\_search\_for” file\_name1 file\_name2 file\_name3*  
Example:   
*$ grep “bread” file1.txt file2.py file3.php recipe.docx*  
The above statement uses the grep command to search for the word / string “bread” in the specified files, i.e., file1.txt, file2.py, file3.php and recipe.docx.

Searching for the specified string/item in all files of a directory:   
*$ grep “bread” ./\** : The last element of the command i.e, “./\*” searches for “bread” in all the files in the working directory.

Ignoring the case sensitivity:  
*$ grep \-i “bread” ./\** : Each command in linux has its own flags, which have the syntax, “-i” or “-a” etc., ‘-i’ here refers to ‘ignore case sensitivity’, and returns all the values of “bread” from all the files in the directory.

Searching with the line number of the string:  
Let’s say you also want the line number of the string you’re searching for in the files. For that you can use another flag called *‘-n’* (these flags can all be combined as well), and this will return the line number and the rest of the line along with the specified string you searched for.

*$ grep \-n \-i “bread” ./\** 

**Combining GREP & FIND using \-exec flag:**  
A very useful flag is \-exec, which allows another command to be executed along with a “main” command, and the secondary command which is being executed is executed as a flag of the “main” command.

Let’s say you want to search within all text files, the existence of the string “bread”. You’d do that by the following command : 

*$ find . \-type f \-iname “\*.txt” \-exec grep \-n \-i “bread” {} \+*  : Notice how I didn't specify the path to grep since it's a flag of the find command, and it’s gonna use the same path as that of find command. 

**Redirecting output of a command into a file:**   
You can print the output of a terminal command into a file, say a text file or something using the “\>” command.

*$ ls \> output.txt* : What this does is that the output, which is the list of contents of the directory, will be printed in the text file called output.txt (if it does not exist, then it gets created automatically.), and the text file will be stored in the same directory, since another path is not specified.

**Redirecting output of a command into a file, and printing it on the terminal at the same time:**  
This action can be performed by the “*tee*” command. 

*$ find . \-type f \-iname “\*.txt” \-size \+10k \-exec grep \-i \-n “bread” {} \+ | tee output.txt* : What this command will do is, it will find all text files with size greater than 10 kb, it’ll search for the string “bread” in all of them, and it will return the grep output, both in terminal and also inside the created text file called “output.txt”. 

**Checking the currently running processes in our machine (similar to task manager of windows:**   
The “*top”* command can be used to display the list of the currently running processes in the terminal. It’s real time, meaning if we start a new application while *top* is running, it’ll show the newly opened application as well. 

The “PID” column refers to the “Process ID” of the application.   
The “User” column tells under whose name the application is running.   
The “Time” column tells how long the application has been running for.   
The “Command” column tells the command of the application. (Command of the application is basically the name which you type in terminal to open that specific application)

To exit such real-time processes in the terminal, hit Ctrl \+ C. 

**Viewing the entire list of processes currently running in the machine:**   
The *ps aux* command is used to view the entire list of all the processes and applications running in the machine. This is not real time.  
   
**Searching for PID of one specific application:**   
Either you can combine *ps aux* with *grep* by the following method:  
*$ ps aux | grep liri-browser* : This command however is really messy, and does a lot more than just returning the PID of the application.  
Instead you can use *pgrep* command for just getting the PID of the process (in this case, liri-browser)  
*$ pgrep liri-browser* : Prints the PIDs of the specified process in chronological order. 

**Killing processes using PID:**  
*$ kill \-9* PID : This command kills the process specified using its PID. 

**Killing all processes without PID:**  
*$ killall* process\_command : This command kills all the processes that have been specified.

**Starting & Stopping services:**   
We can start and stop services by the following two commands:   
*$ sudo service \*service\_name\* start* : For starting  
*$ sudo service \*service\_name\* stop* : For stopping  
*$ sudo service \*service\_name\* restart* : For restarting

We can connect to the services locally by typing in the browser, *localhost:\*port\_number\**. We can also edit the port by editing the configuration of the service which can be done using nano.

**Using system control (systemctl) to start and stop services:**  
*$ sudo systemctl start \*service\_name\**   
*$ sudo systemctl stop \*service\_name\**  
*$ sudo systemctl restart \*service\_name\**

**Scheduling tasks with CRONTAB command:**  
Crontab’s interface can be accessed by typing “*crontab \-e”.*  
For Ubuntu 23.04 and onwards, select editor 1\.   
There are 6 total columns in the crontab interface. 

m h dom mon dow command 

m \= Minute, i.e. 0 to 59  
h \= Hour, i.e. 0 to 23  
dom \= Day of month, i.e. 0 to 29 (for 30 days) or 30 (for 31 days)  
mon \= Month number, i.e. 0 to 11   
dow \= Day of week, i.e 0 to 6   
command \= Whatever command you want to execute.

Command to make a file called outfile.txt every day at 7 PM which contains a list of all items stored in my home directory. 

*$ crontab \-e*  
*00 19 \* \* \* ls \> outfile.txt* : This will make outfile.txt for every day of month, every month of year and every day of week.

\=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=  
21/10/23										Saturday        

\*\*The part of the video course from 3:05:00 to 6:11:00 is not taken notes of, due to irrelevance of the content to the author at the time of note taking. Only the linux essentials were taken note of above, and below are networking essentials. The missing part from the video is more of programming commands and usage of repositories, and github and IDEs, databases, web-servers etc.\*\*

**Getting started with Networking in Ubuntu:**  
We studied a lot about networking in Grade 10 Computer Applications, some of the important definitions are: 

1. LAN : Local Area Network  
2. WAN : Wide Area Network  
3. MAN : Metropolitan Area Network  
4. PAN : Personal Area Network

Another thing I learnt from the course is how the NAT system works. Let's say you have 4 computers in your home and 1 wifi router.   
Instead of your computers all having completely different ip-addresses like 24.244.91.219, 56.71.45.12, etc., your router will have an address which starts with 192.168\*  
So, for example, router \= 192.168.0.1

Computer\_1 \= 192.168.0.11  
Computer\_2 \= 192.168.0.12  
Computer\_3 \= 192.168.0.13  
Computer\_4 \= 192.168.0.14 and so on, and all these ip-addresses are local.

On every computer, there’s an ip-address i.e. 127.0.0.1 which means “this-computer”, which basically means that this is a self-identifying ip-address.  
This save is one of the solutions to the problem of us running out of ipv4 addresses. 

**ping command:**  
The *ping* command is used to connect to a URL, and send and receive data packets to and from it. It sends and receives 64-bytes by default. It tells us the exact IP, the server, the time delay, and when you close it, it tells us the statistics of the ping connection. This is a useful command to check for packet loss in a connection.   
*$ ping google.com* : Connects to google.com and exchanges packets.

**ifconfig command:**  
The *ifconfig* command lets you access all the networking details of your machine in the terminal. It needs to be installed and is not there by default. It can be installed by *$ sudo apt-get install net-tools.*   
Usage : *$ ifconfig* : This will output something like this :-   
—--------------------------------------------------------------------------------------------------  
lo: flags=73\<UP,LOOPBACK,RUNNING\>  mtu 65536  
        inet 127.0.0.1  netmask 255.0.0.0  
        inet6 ::1  prefixlen 128  scopeid 0x10\<host\>  
        loop  txqueuelen 1000  (Local Loopback)  
        RX packets 9911  bytes 868145 (868.1 KB)  
        RX errors 0  dropped 0  overruns 0  frame 0  
        TX packets 9911  bytes 868145 (868.1 KB)  
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

The part highlighted in yellow denotes the local connection with the machine. As you can see it has a self-ip which is always 127.0.0.1. It has a row called RX which means “Received packets and bytes”

wlp2s0: flags=4163\<UP,BROADCAST,RUNNING,MULTICAST\>  mtu 1500  
        inet 192.168.70.10  netmask 255.255.255.0  broadcast 192.168.70.255  
        inet6 fe80::5125:175b:788e:dc47  prefixlen 64  scopeid 0x20\<link\>  
        ether e4:aa:ea:9b:23:d1  txqueuelen 1000  (Ethernet)  
        RX packets 880194  bytes 619513587 (619.5 MB)  
        RX errors 0  dropped 0  overruns 0  frame 0  
        TX packets 616154  bytes 330126550 (330.1 MB)  
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

The part highlighted in green denotes the wireless connection with the router. As you can see it has an ipv4 and an ipv6 address. It also has the RX row, for received packets, but it also has TX for transferred packets. 

—-----------------------------------------------------------------------------------

**tcpdump command:**  
The tcpdump is a pretty big and useful command. It captures all the packets going in and out of your system in real time. It needs to be used as ‘sudo’.   
*$ sudo tcpdump*: Captures packets going in and out of your system in real-time. 

\#Limiting the number of packets you capture using the *\-c* flag:   
*$ sudo tcpdump \-c 10*: Literally, captures only 10 packets. 

\#Capturing the packets in ASCII form:   
*$ sudo tcpdump \-c 5 \-A*: Literally, prints out the packets coming in and out of the machine. 

\#Defining a specific interface for tcpdump using the *\-i* flag:  
*$ sudo tcpdump \-c 5 \-i wlo1*: Listens to the packet transfers on the interface ‘wlo1’ (which is the wireless interface in the *ifconfig* command).

\#Getting the packets in the hex form using the *\-XX* flag:  
*$ sudo tcpdump \-c 5 \-XX \-i wlo1* : Just like how with \-A, the packets were printed in ASCII form, here it’s printed in hex form, and in ASCII format in the side column. 

\#Capturing packets from a specific port:  
*$ sudo tcpdump \-i wlo1 port 22:* Captures packets only from the port no. 22, you can specify port numbers for your servers to check server activity.   
\=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=  
22/10/23      										Sunday

**netstat command:**  
Netstat is an abbreviated term for network-statistics.   
*$ netstat \-nr* : The *\-r* flag tells the terminal that we’re looking for the Kernel IP Routing Table. The *\-n* flag in addition to the *\-r* flag tells the terminal to print out the IP in the dotted-ip form, instead of foreign-address form.

\#Displaying the net-stats of the interface devices:  
*$ netstat \-i* : The interface devices are the ones we displayed using *ifconfig*. 

\#Displaying the net-stats of the sockets:   
*$ netstat \-ta* : This will show the network statistics of the sockets of our machine. If we add *\-n* to *\-ta*, then the foreign addresses form of the ip will be removed, and it will be shown in dotted form. 

The netstat command can be used in multiple ways. One use is to check if your computer is connecting to a malicious server. This can be done by the *\-tan* flag. The foreign-address column will have the ip-address of the servers/computers your machine is connecting to. You can look up those ip-address in IP-WHOIS websites and confirm whether your machine is safe or not. 

**Linux Hosts File:**   
The linux hosts file is basically like an internal DNS server-router. You can add ip-addresses to the websites you wanna go to, and whenever you’ll type the alias for that ip, you’ll be taken there. This can be used to make shortcuts to web pages. 

It can be accessed by the command, *$ sudo nano /etc/hosts*  
It can have 3 columns, the first one is for the ip-address, the second one is for the URL, and the third one is for an alias you wanna give the URL so that you can access it more easily. 

Example:   
      Col.1  	        Col.2  	             Col.3  
192.168.0.1 https://www.google.com/ google

**Hostname and how to change it in Ubuntu:**   
Hostname is the name of your computer/machine which it uses to communicate with other machines on the network. It can be changed by the following steps: 

1. *$ sudo hostnamectl set-hostname new\_host\_name*  
2. *$ sudo nano /etc/hosts* : Here you’ll have to look for the old hostname and change it to the new hostname  
3. *$ sudo service hostname restart* 

**Traceroute command in Ubuntu:**  
First, you’ll have to install the traceroute command and to do that just type:   
*$ sudo apt-get install traceroute*

The traceroute command is used to literally trace the route your machine takes to communicate with a specified server.   
Example: *$ traceroute google.com*: The machine directly won’t just contact google.com, it’ll first go through your wifi router, then your ISP, then DNS servers of the ISP, then DNS servers of google.com then finally to google.com etc. 

The result will show the IP address you’re communicating to, and also the hostname if it's available. Many times it’s gonna show three asterisks \*\*\*, which just means request timed-out.

**Network-Mapping using the nmap command:**   
nmap is the command used for networking-mapping or network-scanning. It’s a very essential and important command for networking in linux. 

%Scanning nmap internally : 

\#Scanning a specific ip address:   
*$ nmap 127.0.0.1*: What this does is, that nmap is gonna scan the ip address provided and show the status of all the ports. If you want more information about the ip-address and its ports, you can use the *\-v* flag, which just provides more information. 

In a few cases, nmap is gonna show that the ports are in ignored states. This is usually because a firewall is blocking connection to them.

\#Scanning multiple ip addresses using the same command: 

You can separate the last digit of your ip-addresses with commas, (if first 3 are the same), so that nmap scans all the ones specified for you, in just one command line.   
*$ nmap 192.168.0.1,100,10:*  This scans the following ip-addresses:   
192.168.0.1  
192.168.0.100  
192.168.0.10  
And prints the nmap report. 

If your ip-addresses are completely different, you can just mention them side-by-side example:   
*$ nmap 192.168.0.1 127.0.0.1* : Scans both the ip-addresses, i.e. 192.168.0.1 and 127.0.0.1

\#Scanning a range of ip-addresses:   
*$ nmap 192.168.0.1-100*: As you can see, I've provided a range for the last column of the ip-address, and it's gonna scan all ip-addresses from 192.168.0.1 all the way to 192.168.0.100.   
   
 %Scanning nmap externally :   
You can also use nmap to scan external devices such as servers, using the hostname of the device.   
*$ nmap \*hostname\**   
Example : *$ nmap pointybracket.net* : Network-maps the server, pointybracket.net

\#Scanning hosts from a file :   
Let’s say you have a file, called networks.txt, with the following text in it:   
192.168.0.1  
192.168.0.100  
127.0.0.1   
If you run *$ nmap \-iL \~/networks.txt* : nmap is going to scan the file networks.txt, for any ip-addresses and hostnames it can find, and scan the found hostnames and ips. 

\#Scanning hosts and displaying their OS details :   
*$ nmap \-A 192.168.0.0-100* : Scans all devices within the 192.168.0 from range 0 to 100 and displays their operating system details. 

\#Scanning a network and see which devices and servers are running :   
*$ nmap \-sP \*network\_ip\_address\**

\#Scanning a network and displaying the reason why a particular port is up and running : Use the *\-reason* flag along with *nmap.*

**Using SSH to access command line of a remote host:**   
To do so you’ll need to use the *ssh* command:   
*$ ssh user\_name\_in\_remote\_host@hostname\_of\_remote\_host* : This will prompt you for the password of that user in the remote host and then give you command line access to that host. 

**Using SFTP to transfer files between local and remote host:**   
You can transfer files between local and remote machines using the *sftp* command:   
*$ sftp user\_name\_in\_remote\_host@hostname\_of\_remote\_host* : This will again ask for the password and then you can access the remote host server.   
If you type *lls* in the remote host’s command line, you will get the local-list, which is the list of items in the current working directory of your local machine. 

*sftp\> put file\_name.extension* : Puts file from local to remote host.  
*sftp\> get file\_name.extension* : Pulls file from remote to local host.

**Getting the ‘manual’ or usage directions for any command:**   
You can get the usage manual for any command using the *man* command and then typing the name of the command you want information about.   
Example: *$ man ssh* or *$ man sftp* or *$ man nmap etc.* 

**‘Google: Introduction to Git and Github’**  
**Notes**  
Coursera

05/06/25												Thursday

**Differentiating b/w files:**  
diff tells difference between files  
example:  
5c5 on top means 5th line changed to something in 5th line  
11a13 indicates “added”  
\< means line removed  
\> means line added

\-u : provides better interface   
\++ means line added  
\-- means lined removed

more tools:  
wdiff, meld, KDiff3, vimdiff

**Creating .diff files:**  
diff \-u *\<old file\> \<new file\>* \> change.diff 

**Patching files with .diff files:**  
patch *\<old file\>* \< *\<diff file\>*

