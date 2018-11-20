
The lazy's guide to linux, servers and executing stuff in them. And getting the results back, of course. If not, why would you even bother... anyways.

## Why though?

This terminal is useful because in case you do not have a directory explorer (the thing were you see the boxes with files and directories), you need to be able to do everything you did there but via a terminal.

TODO.

## Introduction and requisites

To open a new command line in Linux you first need to have Linux. 

If you have *mac*: you're doomed. Not really. Actually yes but also: most Linux commands *should* work in your terminal.
If you've *windows* you can actually start an ubuntu (or any other Linux distro) command line but [I'm not explaining this right now](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

In Linux, to open a new terminal window, do `Ctrl + Alt + T`. Or search for the application "Terminal". Or "Konsole" or "Console".

There, you'll see the famous black screen (don't worry you can change the colour, although I have no idea how).

When you open a terminal you start with the view in your home directory which is, usually, something like `/home/USERNAME/` in Linux or `C:/Users/USERNAME/` in Windows.

## Basic commands

So, if you want to create a new directory that is named 'homework' you do:

	mkdir homework

Now if we do `ls` (or the shortcut for the *really lazy* `l`) we'll see the folder:

    ls

Although I usually prefer the following format because it shows everything important:

    ls -lha

To create an empty fil inside the folder (why would you want that you say? to make the examples below I answer):

    touch homework/myhomework.txt

To see the small help on some command you can (usually) do the following:

    mkdir --help

To see the big help of a command you need to do the following:

    man mkdir

Here you enter a new "page" and you can go up and down with arrows. To close it you do: `Q`.

There are other commands that are very easy to understand. Below the table with an example:

Command | what it does | example
------ | ------ | ------
cp | copys a file  | cp homework/myhomework.txt myhomework_copy.txt
cp -r \* | copys a  directory | cp homework homework_copy
rm | deletes a file | rm homework/myhomework.txt
mkdir | creates a directory | mkdir homework
cd | enter a directory | cd homework
cd .. | go up one directory | cd ..
touch | creates an empty file | touch homework/myhomework.txt
scp | like cp but between different machines | scp user@server:PATH_TO_FILE LOCAL_PATH_TO_FILE
cat | show the contents of a file | cat homework/myhomework.txt
tail | like cat but only shows the last lines | tail homework/myhomework.txt
echo | show text in the console | echo $HOME

\* I'm not going to put the `-r` everywhere. Usually when dealing with directories, you kinda need to put it. Don´t worry, Linux will tell you if not.

## Connecting to a server

To connect to a server you need a couple of things:

1. Be able to locate the server in your network (or over the Internet). Or in other words, the server needs to be accessible.
2. Have a username with access to this server (and the password: no passport needed).
3. Have `ssh` installed in your pc (and running in the server but that's usually not your job). It usually comes with Linux.

To connect to a server you do the following:

    ssh USERNAME@SERVER

For example if I want to connect to server I´d do:

    ssh f.peschiera@serv-cluster1

Here you get asked your password, which you can then write. Then press enter and so forth. The first time it asks you some weird thing about public keys. You just need to trust it and say "yes" out loud (and write `y`).

Once you connect things do not change too much. You have the same black screen (Did you change the colour already?). The text just before your commands will slightly change and will make you notice you're connected.

To be sure you can do something like:

    ls -lha

And confirm the files you see are the files on the server.
Almost all (basic) commands should work the same.

If you want to exit the server you just write:

    exit

Of course.

## Executing something in your server

This is no different as executing something in your pc. You just need to take some things into account:

1. You cannot access the files in your pc from the server. Unless you send them via `scp` or some other way (see section *Moving things between machine and server*).
2. The paths in the server may be different from the paths in your pc. So if you use absolute paths, beware.
3. Maybe you had installed things in your pc that you do not have in the server. Or versions of programs can vary. Beware.

Example. If you have a python script you want to execute, you need to do the following:

    python script.py

Some things that can be useful when executing things in a command line.

**If you want to make the script output to a file**. Imagine your script shows things to the terminal screen when you execute it (because you make `print` or an equivalent) and you want to save those messages. you can add a `> output.log` at the end of the command to hide the output of the script and instead write it in the file.

    python script.py > output.log

Of course you can always write some code inside your script to do that. But that's just more work!

Another example. If `echo` writes output to the console and `>` redirects it to a file you can do the following to fill a file with text:

    echo 'this is content' > homework/myhomework.txt

Then you can check you wrote it correctly:

    cat homework/myhomework.txt

**Stopping the execution of your script**. This one is easy: you just to `Ctrl + C`.

**If you want to avoid blocking the terminal window with your script**. Imagine you execute you script in the server but you want to go home. You don't want to leave your machine on. Or you don't want to risk the connection to the server to break. Or you just want to use the terminal to do other things (like execute more scripts). You just need to add `&` at the end of the script.

    python script.py &

This will create a process id (and will show the number to you). This number is important! In case you want to later stop the process, for example. Because the `Ctrl + C` doesn't work any more! To do this see section *Monitoring processes and killing them*.

## Monitoring processes and killing them

Imagine you have things executing and all of a sudden you want to do the equivalent of opening the "Monitoring" (Ubuntu) or "Task manager" (Windows) to see what's going on with your executions and why is your machine so slow. You can do the following:

    top

(remember: you go out with `Q`).

If you want a more fancy version of this tool (in case it's installed) you can do:

    htop

Anyway, you can see the processes that are running and the resources they consume. The first column in the table shows the process id (not a surprise: it's called *PID*).

If you want to kill the process you do:

    kill PID_NUMBER

In the rare cases it resists to death, you can try:

    kill -9 PID_NUMBER

You should always return to see the processes to confirm your process is dead!

    top

In the case your process is writing something into a log file, you can monitor the contents actively.

You just to use the following command:

    tail -f homework/myhomework.txt

The `tail` command just shows the last N lines of a file. When providing the `-f` argument we tell it to update automatically as the file updates, always showing only the last lines.

To close this visualization, one needs to do the famous `Ctrl + C`

## Getting information from your server

**Memory**:

    free -mh

**CPU**:

    lscpu

**distribution**:

    cat /etc/*release

More information on getting the OS information in this [*very complete page*s](http://whatsmyos.com).

## Moving things between machine and server

For simple use cases (few files), `scp` one can move one file or an entire directory from and to the server. This is already useful. But not enough.

An example using this would be:

    scp -r f.peschiera@serv-cluster1:/home/disc/f.peschiera/Documents/projects/ROADEF2018/results/clust1_20180922_venv ./

The first argument is the source (that is located in the server) and the second argument (`./`) is the present location in the command line. So I'm saying: bring that directory over there, "here".

### Moving code

I recommend `git` to move code. With git you can easily push and pull code from a git server. This way, you know you get the correct version of your code. This tutorial is not a git tutorial, you'll have to find some other resources. Git has many more advantages that make it a strong recommendation.

Some very basic examples:

    git push origin master

sends your last committed changes to the git server's main branch.

    git pull origin master

brings the last changes from the main branch to the pc from which you execute it.

### Moving data

I recommend `rsync` to move data files. With this application you can tell to bring or send a given directory and and only the files that have changed will move.

For example I use:
    
    rsync -rav -e ssh --include '*/' --exclude='formulation.lp' f.peschiera@serv-cluster1:/home/disc/f.peschiera/Documents/projects/optima/results/* ./

to bring sync all the results I got from the last executions to the current directory in my pc.

Other options include some third party syncing app such as Dropbox, NextCloud, Google Drive, etc.

## Advices and conventions

* **File and directory names**: never put spaces, commas or weird things in your files and directory's name.
* **Always have a configuration file** where one can parametrize the environment accordingly. This way, you don't need to update all your code if you want to change some configuration * in the server (for example, the number of cores to use or the maximum memory to use, or the time to execute). Another option is using command line arguments.* 
* **Always separate code from data**: data usually doesn't change often. Code does. This way you only get to change a small file and it's easier to track changes.
* **Write your most known commands somewhere**: that way you just need to copy and paste and not remember everything every time.

## Virtual Private Network

You'd use a VPN to watch the World Cup matches that are not televised in France (such as Peru - Australia). Another use is to get inside the university network while you're outside. This can be useful to connect to a server that is only accessible from within the network of the university.

The configuration for a VPN varies from service to service.

TODO: finish this.