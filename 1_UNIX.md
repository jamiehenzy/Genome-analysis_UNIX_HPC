This tutorial is adapted from the following work:

*Unix and Perl Primer for Biologists* 
Keith Bradnam & Ian Korf 

>Unix and Perl Primer for Biologists by Keith Bradnam & Ian Korf is licensed under a Creative Commons Attribution-Noncommercial-No Derivative Works 3.0 License. Please send feedback, questions, money, or abuse to
<keith.bradnam@icr.ac.uk> or <ifkorf@ucdavis.edu>. (c) 2016, all rights reserved.

This course has been greatly extended and reworked into a book that has been published by Cambridge University Press. It is available to order on [Amazon.com][Amazon] and at many other [online stores][online_stores]. It is also available in various ebook formats.

[Amazon]: https://www.amazon.com/gp/product/0521169828?tag=keithbradnamc-20
[online_stores]: http://rescuedbycode.com/about-the-book/#wheretobuy
---

# Part 1: Unix - Learning the essentials

## Introduction to Unix

These exercises will (hopefully) teach you to become comfortable when working in the environment of the Unix terminal. Unix contains many hundred of commands but you will probably use just 10 or so to achieve most of what you want to do.

You are probably used to working with programs like the Apple Finder or the Windows File Explorer to navigate around the hard drive of your computer. Some people are so used to using the mouse to move files, drag files to trash etc. that it can seem strange switching from this behavior to typing commands instead. Be patient, and try as much as possible to stay within world of the Unix terminal. Please make sure you complete and understand each command before moving on to the next one.

--- 

## U1. The Terminal

A terminal is the common name for the program that does two main things. It allows you to type input to the computer (i.e. run programs, move/view files etc.) and it allows you to see output from those programs. If you're working on a Mac, you can open the application known as Terminal. If you're working on a PC, you'll need to use Windows Subsystem for Linus (WSL).

Once you are in Terminal, you should see something that looks like the following (any text that appears inside your terminal window will look different):

![alt text](https://github.com/jamiehenzy/Genome-analysis_Unit_1/blob/assets/terminal.png)

Before we go any further, you should note that you can:

* make the text larger/smaller (hold down command and either + or -) 
* resize the window (this will often be necessary) 
* have multiple terminal windows on screen (see the Shell menu) 
* have multiple tabs open within each window (again see the Shell menu)

There will be many situations where it will be useful to have multiple terminals open and it will be a matter of preference as to whether you want to have multiple windows, or one window with multiple tabs (there are keyboard shortcuts for switching between windows, or moving between tabs). 

--- 

## U2. Your first Unix command

Unix keeps files arranged in a hierarchical structure. From the 'top-level' of the computer, there will be a number of directories, each of which can contain files and subdirectories, and each of those in turn can of course contain more files and directories and so on, ad infinitum. It's important to note that you will always be in a directory when using the terminal. The default behavior is that when you open a new terminal you start in your own home directory (containing files and directories that only you can modify).

To see what files are in our home directory, we need to use the `ls` command. This command *list*s the contents of a directory. So why don't they call the command *list* instead? Well, this is a good thing because typing long commands over and over again is tiring and time-consuming. There are many (frequently used) Unix commands that are just two or three letters. If we run the `ls` command we should see something like:

	olson27-1:~ kbradnam$ ls 
	Application Shortcuts 	Documents	Library 
	Desktop					Downloads 
	olson27-1:~ kbradnam$

There are four things that you should note here:

1. You will probably see different output to what is shown here, it depends on your computer. Don't worry about that for now. 
2. The `olson27-1:~ kbradnam$` text that you see is the Unix command prompt. It contains a user name (kbradnam), the name of the machine that this user is working on (olson27-1) and the name of the current directory. Note that the command prompt might not look the same on different Unix systems. For example, your prompt may be the \% symbol instead of \$.
3. The output of the `ls` command lists five things. In this case, they are all directories, but they could also be files. We'll learn how to tell them apart later on. 
4. After the `ls` command finishes it produces a new command prompt, ready for you to type your next command.

#### Task U2.1
Go to your home directory and use the `ls` command to display the contents.

---

## U3: The Unix tree

Looking at directories from within a Unix terminal can often seem confusing. But bear in mind that these directories are exactly the same type of folders that you can see if you use Apple's graphical file-management program (known as Finder). A tree analogy is often used when describing computer filesystems. From the root level (/) there can be one or more top level directories, though most Macs will have about a dozen. In the example below, we show just three. When you log in to a computer you are working with your files in your home directory, and this will nearly always be inside a Users directory.

All Macs have an applications directory where all the GUI (graphical user interface) programs are kept (e.g. iTunes, Microsoft Word, Terminal). Another directory that will be on all Macs is the Volumes directory. In addition to any attached _external_ drives, the Volumes directory should also contain directories for every _internal_ hard drive (of which there should be at least one, in this case it's simply called Mac). It will help to think of this tree when we come to copying and moving files. E.g. if we had a file in the Code directory and wanted to copy it to the keith directory, we would have to go _up_ four levels to the root level, and then _down_ two levels.

![alt text](https://github.com/jamiehenzy/Genome-analysis_Unit_1/blob/assets/dir_tree.png)

---

## U4: Finding out where you are

There may be many hundreds of directories on any Unix machine, so how do you know which one you are in? The command `pwd` will Print the Working Directory:

	olson27-1:~ kbradnam$ pwd 
	/users/clmuser

When you log in to a Unix computer, you are typically placed into your _home_ directory, which will be \/Users\/yourname (on a Mac). An abbreviation for this path is simply the tilde (\~) sign. No matter where  you are in your tree, by typing `cd ~` you can get back home (\/Users\/yourname).

In this example, after we log in, we are placed in a directory called 'clmuser' which itself is a subdirectory of another directory called 'users'. Conversely, 'users' is the parent directory of 'clmuser'. The first forward slash that appears in a list of directory names always refers to the top level directory of the file system (known as the root directory). The remaining forward slash (between 'users' and 'clmuser') delimits the various parts of the directory hierarchy. If you ever get 'lost' in Unix, remember the `pwd` command.

As you learn Unix you will frequently type commands that don't seem to work. Most of the time this will be because you are in the wrong directory, so it's a really good habit to **get used to running the `pwd` command a lot**!

---

## U5: Getting from 'A' to 'B'

We are in the home directory on the computer but we want to to work on the Desktop. To change directories in Unix, we
use the `cd` command. Because the directory we want to go to is a subdirectory of home, we can simply type its name (case-sensitive):

	olson27-1:~ kbradnam$ cd Desktop 
	olson27-1:USB kbradnam$ ls
	Items	Data    Code   Misc 
	olson27-1:Desktop kbradnam$ pwd 
	/Users/yourname/Desktop

The first command reads as "change directory to the Desktop directory that is inside the current (home) directory, which itself is at the root level of the computer". Did you notice that the command prompt changed after you ran the `cd` command?  The '~' sign should have changed to 'Desktop'. This is a useful feature of the command prompt. By default it reminds you where you are as you move through different directories on the computer.

---

## U6: Root is the root of all evil

In the previous example, we could have achieved the same result in three separate steps:

	$ cd /Users 
	$ cd yourname 
	$ cd Desktop

Note that the second and third commands do not include a forward slash. When you specify a directory that starts with a forward slash, you are referring to a directory that should exist one level below the root level of the computer. What happens if you try the following two commands? The first command should produce an error message.

	$ cd Users
	$ cd /Users

The error is because without including a leading slash, Unix is trying to change to a Users directory below your current level in the file hierarchy, but there is no directory called Volumes at this location.

---

## U7: Up, up, and away

Frequently, you will find that you want to go 'upwards' one level in the directory hierarchy. Two dots `..` are used in Unix to refer to the _parent_ directory of wherever you are. Every directory has a parent except the root level of the computer:

	$ cd ~/Desktop
	$ pwd 
	/Users/yourname/Desktop
	$ cd .. 
	$ pwd 
	/Users

What if you wanted to navigate up _two_ levels in the file system in one go? It's very simple, just use two sets of the `..` operator, separated by a forward slash:

	$ cd ~/Desktop
	$ pwd 
	/Users/yourname/Desktop 
	$ cd ../.. 
	$ pwd 
	/Users 
	
---	
	
## U8: I'm absolutely sure that this is all relative

Using `cd ..` allows us to change directory _relative_ to where we are now. You can also always change to a directory based on its _absolute_ location. E.g. if you are working in the `/Users/yourname/Desktop/Code` directory and you then want to change to the `/Users/yourname/Desktop/Items` directory, then you could do either of the following:

	$ cd ../Items

or...

	$ cd /Users/yourname/Desktop/Items

They both achieve the same thing, but the 2nd example requires that you know about the full _path_ from the root level of the computer to your directory of interest (the 'path' is an important concept in Unix). Sometimes it is quicker to change directories using the relative path, and other times it will be quicker to use the absolute path.

#### Task U8.1
Try moving from a directory to another directory at that same level, using each method (absolute and relative paths).

---

## U9: Time to go home

Remember that the command prompt shows you the name of the directory that you are currently in, and that when you are in your home directory it shows you a tilde character (~) instead? This is because Unix uses the tilde character as a short-hand way of specifying a home directory.

#### Task U9.1
See what happens when you try the following commands (use the `pwd` command after each one to confirm the results):

	$ cd / 
	$ cd ~ 
	$ cd / 
	$ cd

Hopefully, you should find that `cd` and `cd ~` do the same thing, i.e. they take you back to your home directory (from wherever you were). Also notice how you can specify the single forward slash to refer to the root directory of the computer. When working with Unix you will frequently want to jump straight back to your home directory, and typing `cd` is a very quick way to get there.

---

## U10: Making the `ls` command more useful

The `..` operator that we saw earlier can also be used with the `ls` command. Can you see how the following command is listing the contents of the root directory? If you want to test this, try running `ls /` and see if the output is any different.

	$ cd /Users/yourname/Desktop
	$ ls ../../.. 
	Applications	Volumes		net
	CRC				bin			oldlogins 
	Developer		cores		private
	Library			dev			sbin 
	Network			etc			tmp 
	Server			home		usr
	System			mach_kernel	var 
	Users			mach_kernel.ctfsys

The `ls` command (like most Unix commands) has a set of options that can be added to the command to change the results. Command-line options in Unix are specified by using a dash (\-) after the command name followed by various letters, numbers, or words. If you add the letter l to the `ls` command it will give you a longer output compared to the default:

	$ ls -l /Users/yourname/Desktop
	total 192 
	drwxrwxrwx  1 keith  staff  16381 Oct  3 09:03 Items
	drwxrwxrwx  1 keith  staff  16384 Oct  3 11:11 Data  
	drwxrwxrwx  1 keith  staff  26384 Oct  3 11:34 Code
 	drwxrwxrwx  1 keith  staff  13384 Oct  3 11:45 Misc
 

For each file or directory we now see more information (including file ownership and modification times). The 'd' at the start of each line indicates that these are directories

#### Task U10.1
There are many, many different options for the ls command. Try out the following (against any directory of your choice) to see how the output changes.

	ls -l 
	ls -R 
	ls -l -t -r 
	ls -lh

Note that the last example combine multiple options but only use one dash. This is a very common way of specifying multiple command-line options. You may be wondering what some of these options are doing. It's time to learn about Unix documentation....

---

## U11: Man your battle stations!

If every Unix command has so many options, you might be wondering how you find out what they are and what they do. Well,
thankfully every Unix command has an associated "manual" that you can access by using the `man` command. E.g.

	$ man ls 
	$ man cd
	$ man man # yes even the man command has a manual page

When you are using the man command, press `space` to scroll down a page, `b` to go back a page, or `q` to quit. You can also use the up and down arrows to scroll a line at a time. The man command is actually using another Unix program, a text viewer called `less`, which we'll come to later on.

Some Unix commands have very long manual pages, which might seem very confusing. It is typical though to always list the command line options early on in the documentation, so you shouldn't have to read too much in order to find out what a command-line option is doing.

---

## U12: Make directories, not war

If we want to make a new directory (e.g. to store some work related data), we can use the 
`mkdir` command:

	$ cd /Users/yourname/Desktop
	$ mkdir unix
	$ ls
	Items	Data    Code   Misc	unix
 	$ cd /Users/yourname/Desktop/unix
	$ mkdir Temp1 
	$ cd Temp1 
	$ mkdir Temp2 
	$ cd Temp2 
	$ pwd 
	/Users/yourname/Desktop/unix/Temp1/Temp2

In the last example we created the two temp directories in two separate steps. If we had used the `-p` option of the `mkdir` command we could have done this in one step. E.g.

	$ mkdir -p Temp1/Temp2

#### Task U12.1
Practice creating some directories and navigating between them using the `cd` command. Try changing
directories using both the _absolute_ as well as the _relative_ path.

---

## U13: Time to tidy up

We now have a few (empty) directories that we should remove. To do this use the `rmdir` command. This will only remove empty directories so it is quite safe to use. If you want to know more about this command (or any Unix command), then remember that you can just look at its man page.

	$ cd /Users/yourname/Desktop
	$ rmdir unix

Why the error on the `rmdir` command? If a directory contains files or other directories, you need to use `rm -r` instead. Try it!

#### Task U13.1
Use `cd` to navigate to your Downloads directory. Remove an unwanted file.

---

## U14: The art of typing less to do more

Saving keystrokes may not seem important, but the longer that you spend typing in a terminal window, the happier you
will be if you can reduce the time you spend at the keyboard. Especially, as prolonged typing is not good for your body.
So the best Unix tip to learn early on is that you can **tab complete** the names of files and programs on most Unix
systems. Type enough letters that uniquely identify the name of a file, directory or program and press tab...Unix will
do the rest. E.g. if you type 'tou' and then press tab, Unix will autocomplete the word to touch (which we will learn
more about in a minute). In this case, tab completion will occur because there are no other Unix commands that start
with 'tou'. If pressing tab doesn't do anything, then you have not have typed enough unique characters. In this case
pressing tab _twice_ will show you all possible completions. This trick can save you a LOT of typing...if you don't use
tab-completion then you must be a masochist.

#### Task U14.1
Navigate to your home directory, and again make a "unix" directory. Then use the `cd` command to change to the `/Users/yourname/Desktop/unix` directory. Use tab completion for each directory name.

Another great time-saver is that Unix stores a list of all the commands that you have typed in each login session. You can access this list by using the `history` command or more simply by using the up and down arrows to access anything from your history. So if you type a long command but make a mistake, press the up arrow and then you can use the left and right arrows to move the cursor in order to make a change.

---

## U15: U _can_ touch this

The following sections will deal with Unix commands that help us to work with files, i.e. copy files to/from places, move files, rename files, remove files, and most importantly, look at files. Remember, we want to be able to do all of these things without leaving the terminal. First, we need to have some files to play with. The Unix command `touch` will let us create a new, empty file. The touch command does other things too, but for now we just want a couple of files to work with.

	$ cd /Users/yourname/Desktop/unix
	$ touch heaven.txt 
	$ touch earth.txt 
	$ ls 
	earth.txt     heaven.txt

---

## U16: Moving heaven and earth

Now, let's assume that we want to move these files to a new directory (Temp). We will do this using the Unix `mv` (move) command:

	$ mkdir Temp 
	$ mv heaven.txt Temp/ 
	$ mv earth.txt Temp/ 
	$ ls 
	Temp 
	$ ls Temp/
	earth.txt heaven.txt

For the `mv` command, we always have to specify a source file (or directory) that we want to move, and then specify a
target location. If we had wanted to we could have moved both files in one go by typing any of the following commands:

	$ mv *.txt Temp/ 
	$ mv *t Temp/ 
	$ mv *ea* Temp/

The asterisk `*` acts as a wild-card character, essentially meaning 'match anything'. The second example works because there are no other files or directories in the directory that end with the letters 't' (if there was, then they would be copied too).  Likewise, the third example works because only those two files contain the letters 'ea' in their names. Using wild-card characters can save you a lot of typing.

#### Task U16.1
Use `touch` to create three files called 'fat', 'fit', and 'feet'inside the Temp directory. I.e.

	$ cd Temp
	$ touch fat fit feet

Then type either `ls f?t` or `ls f*t` and see what happens. The ? character is also a wild-card but with a slightly different meaning. Try typing `ls f??t` as well.

---

## U17: Renaming files

In the earlier example, the destination for the `mv` command was a directory name (Temp). So we moved a file from its source location to a target location ('source' and 'target' are important concepts for many Unix commands). But note that the target could have also been a (different) file name, rather than a directory. E.g. let's make a new file in Desktop and move it whilst renaming it at the same time:

	$ cd ..	
 	$ touch rags 
	$ ls 
	Temp  rags 
	$ mv rags Temp/riches 
	$ ls Temp/
	earth.txt      heaven.txt     riches

In this example we create a new file ('rags') and move it to a new location and in the process change the name (to 'riches'). So `mv` can rename a file as well as move it. The logical extension of this is using `mv` to rename a file without moving it (you have to use `mv` to do this as Unix does not have a separate 'rename' command):

	$ mv Temp/riches Temp/rags 
	$ ls Temp/ 
	earth.txt      heaven.txt     rags

---

## U18: Stay on target

It is important to understand that as long as you have specified a 'source' and a 'target' location when you are moving a file, then it doesn't matter what your current directory is. You can move or copy things within the same directory or between different directories regardless of whether you are in any of those directories. Moving directories is just like moving files:

	$ mkdir Temp2 
	$ ls 
	Temp  Temp2 
	$ mv Temp2 Temp/ 
	$ ls Temp/
	Temp2		earth.txt		heaven.txt	rags

This step moves the Temp2 directory inside the Temp directory.

#### Task U18.1
Create another Temp directory (Temp3) and then change directory to your home directory (/Users/yourname or simply `~`). **Without** changing directory, move the Temp3 directory to inside the /Users/yourname/Desktop/Temp directory.

---

## U19: Here, there, and everywhere

The philosophy of not having to be in a directory to do something in that directory, extends to just about any operation that you might want to do in Unix. Just because we need to do something with file X, it doesn't necessarily mean that we have to change directory to wherever file X is located. Let's assume that we just want to quickly check what is in the Data directory before continuing work with whatever we were previously doing in /Users/yourname/Desktop. Which of the following looks more convenient:

	$ cd Data 
	$ ls 
	Arabidopsis		C_elegans	GenBank		Misc		Unix_test_files 
	$ cd ..

or...

	$ ls Data/ 
	Arabidopsis		C_elegans	GenBank		Misc		Unix_test_files

In the first example, we change directories just to run the ls command, and then we change directories back to where we were again. The second example shows how we could have just stayed where we were.

---

## U20: To slash or not to slash?

#### Task U20.1
Run the command to return to your home directory.

From your home directory, run the following commands:

	$ ls Desktop

	$ ls Desktop/

The two examples are not quite identical, but they produce identical output. So does the trailing slash character in the second example matter? Well not really. In both cases we have a directory named Desktop and it is optional as to whether you include the trailing slash. When you tab complete any Unix directory name, you will find that a trailing slash character is automatically added for you. This becomes useful when that directory contains subdirectories which you also want to tab complete.

I.e. imagine if you had to type the following (to access a buried directory "ggg") and tab-completion _didn't_ add the trailing slash characters. You'd have to type the seven slashes yourself.

	$ cd aaa/bbb/ccc/ddd/eee/fff/ggg/
	
---	
	
## U21: The most dangerous Unix command you will ever learn!

You've seen how to remove a directory with the `rmdir` command, but `rmdir` won't remove directories if they contain any files. So how can we remove the files we have created (in /Users/yourname/Desktop/Temp)? In order to do this, we will have to use the `rm` (remove) command.

>***Please read the next section VERY carefully. Misuse of the rm command can lead to needless death & destruction*** 

Potentially, `rm` is a very dangerous command; if you delete something with `rm`, you will not get it back! It does **not** go into the trash or recycle can, it is permanently removed. It is possible to delete everything in your home directory (all directories and subdirectories) with `rm`, that is why it is such a dangerous command.

Let me repeat that last part again. **It is possible to delete EVERY file you have ever created with the `rm` command.** Are you scared yet? You should be. Luckily there is a way of making `rm` a little bit safer. We can use it with the `-i` command-line option which will ask for confirmation before deleting anything:

	$ cd /Users/jamiehenzy/Desktop/unix 
	$ touch dog.txt cat.txt  owl.fa
	$ ls 
	dog.txt	    cat.txt  owl.fa
	$ rm -i dog.txt 
	remove dog.txt? y 
	$ rm -i cat.txt 
	remove cat.txt? y

We could have simplified this step by using a wild-card (e.g. `rm -i *.txt`).

#### Task U21.1
Remove the remaining file from the "/Users/yourname/Desktop/unix" directory. Now navigate to either your Desktop or Downloads folder and remove any files you don't need. Use a wild-card when appropriate. For example, I frequently have screenshots pile up on my Desktop, which I can quickly remove with `rm Scree*`.

---

## U22: Go forth and multiply

Copying files with the `cp` (copy) command is very similar to moving them. Remember to always specify a _source_ and a _target_ location. While in your unix folder, create a new file and make a copy of it.

	$ touch file1 
	$ cp file1 file2 
	$ ls 
	file1	file2

What if we wanted to copy files from a different directory to our current directory? Let's put a file in our home
directory and copy it to the unix directory:

	$ touch ~/file3 
	$ ls 
	file1	file2 
	$ cp ~/file3 . 
	$ ls file1 file2 file3

This last step introduces another new concept. In Unix, the current directory can be represented by a '.' (dot) character. You will mostly use this only for copying files to the current directory that you are in. But just to make a quick point, compare the following:

	$ ls 
	$ ls . 
	$ ls ./

In this case, using the dot is somewhat pointless because `ls` will already list the contents of the current directory by default. Also note again how the trailing slash is optional.

Let's try the opposite situation and copy these files back to the home directory (even though one of them is already there). The default behavior of copy is to overwrite (without warning) files that have the same name, so be careful.

	$ cp file* ~/

Based on what we have already covered, do you think the trailing slash in `~/` is necessary? 

---

## U23: Going deeper and deeper

The `cp` command also allows us (with the use of a command-line option) to copy entire directories (also note how the `ls` command in this example is used to specify multiple directories). From within the unix directory:

	$ mkdir Storage 
	$ mv file* Storage/ 
	$ ls 
	Storage 
	$ cp -R Storage Storage2 
	$ ls Storage Storage2 
	Storage:
	file1	file2	file3

	Storage2: 
	file1	file2	file3

#### Task U23.1
The `-R` option means "copy recursively". Many other Unix commands also have a similar option. See what happens if you don't include the `-R` option. We've finished with all of these temporary files now. Make sure you remove the unix directory and its contents (remember to always use `rm -i`).

---

## U24: When things go wrong

At this point in the course, you may have tried typing some of these commands and have found that things did not work as expected. Some people will then assume that the computer doesn't like them and that it is being deliberately mischievous. The more likely explanation is that you made a typing error. Maybe you have seen one the following error messages:

	$ ls Codee 
	ls: Codee: No such file or directory

	$ cp Data/Unix_test_files/* Docmentation 
	usage: cp [-R [-H | -L | -P]] [-fi | -n] [-pvX] source_file target_file 
	       cp [-R [-H | -L | -P]] [-fi | -n] [-pvX] source_file ... target_directory

In both cases, we included a deliberate typo when specifying the name of the directories. With the `ls` command, we get a fairly useful error message. With the `cp` command we get a more cryptic message that reveals the correct usage statement for this command. In general, if a command fails, check your current directory (`pwd`) and check that all the files or directories that you mention actually exist (and are in the right place). Many errors occur because people are not in the right directory! 

---

## U25: Less is more

So far we have covered listing the contents of directories and moving/copying/deleting either files and/or directories. Now we will quickly cover how you can look at files; in Unix the `less` command lets you view (but not edit) text files. Let's take a look at a file of _Arabidopsis thaliana_ protein sequences. First you'll need to download the "At_proteins.fasta" file from Genome-analysis_UNIX_HPC repository on GitHub. Click on the filename, then right-click the `Raw` button to Copy Link Address. In Terminal, type the command `wget` and paste the link address and hit enter.

Now we can look at the contents:

	$ less At_proteins.fasta

When you are using less, you can bring up a page of help commands by pressing `h`, scroll forward a page by pressing `space`, or go forward or backwards one line at a time by pressing `j` or `k`. To exit less, press `q` (for quit). The `less` program also does about a million other useful things (including text searching).

#### Task U25.1
Make a new directory called Data in your Desktop. Move the At_proteins.fasta file to the Data directory. Show that it is there using the `ls` command.

---

## U26: Directory enquiries

When you have a directory containing a mixture of files and directories, it is not often clear which are files and which, directories. One solution is to use `ls -l` which will put a **d** at the start of each line of output for items which are directories. A better solution is to use `ls -p`. This command simply adds a trailing slash character to those items which are directories. Compare the following:

	$ ls 
	Applications	Data	file1 	Code	Documentation	file2

	$ ls -p 
	Applications/	Data/	file1 	Code/	Documentation/	file2

Hopefully, you'll agree that the second example makes things a little clearer. You can also do things like always capitalizing directory names (like I have done) but ideally we would suggest that you always use `ls -p`. If this sounds a bit of a pain, then it is. Ideally you want to be able to make `ls -p` the default behavior for `ls`. Luckily, there is a way of doing this by using Unix aliases. It's very easy to create an alias:

	$ alias ls='ls -p' 
	$ ls 
	Applications/	Data/	file1 	Code/	Documentation/	file2

If you have trouble remembering what some of these very short Unix commands do, then aliases allow you to use human-readable alternatives. I.e. you could make a "copy" alias for the cp command or even make "list_files_sorted_by_date" perform the `ls -lt` command. Note that aliases do not replace the original command. It can be dangerous to use the name of an existing command as an alias for a different command. I.e. you could make an `rm` alias that put files to a "trash" directory by using the `mv` command. This might work for you, but what if you start working on someone else's machine who doesn't have that alias? Or what if someone else starts working on your machine?

#### Task U26.1
Create an alias such that typing `rm` will always invoke `rm -i`. Try running the alias command on its own to see what happens. Now open a new terminal window (or a new tab) and try running your `ls` alias. What happens?

---

## U27: Fire the editor

The problem with aliases is that they only exist in the current terminal session. Once you log out, or use a new terminal window, then you'll have to retype the alias. Fortunately though, there is a way of storing settings like these. To do this, we need to be able to create a configuration file and this requires using a text editor. We could use a program like TextEdit to do this (or even Microsoft Word), but as this is a Unix course,  we will use a simple Unix editor called nano. Let's create a file called profile:

	$ cd ~ 
	$ nano profile

You should see the following appear in your terminal:

![alt text](https://github.com/jamiehenzy/Genome-analysis_Unit_1/blob/assets/nano.png)

The bottom of the nano window shows you a list of simple commands that are all accessible by typing "Control" plus a letter. E.g. Control + X exits the program. "Control" is often represented as \^, or in this case \^X.

#### Task U27.1
Type the following text in the editor and then save it (\^O). Nano will ask if you want to "save the modified buffer" and then ask if you want to keep the same name. Then exit nano (\^X) and use `less` to confirm that the profile file contains the text you added.

 	# some useful command line short-cuts 
	alias ls='ls -p' 
	alias rm='rm -i'

Now you have successfully created a configuration file (called "profile") which contains two aliases. The first line that starts with a hash (#) is a comment, these are just notes that you can add to explain what the other lines are doing. But how do you get Unix to recognize the contents of this file? The `source` command tells Unix to read the contents of a file and treat it as a series of Unix commands (but it will ignore any comments).

#### Task U27.2
Open a new terminal window or tab (to ensure that any aliases will not work) and then type the following (make sure you first change to the correct directory):

	$ source profile

Now try the `ls` command to see if the output looks different. Next, use `touch` to make a new file and then try deleting it with the `rm` command. Are the aliases working?

---

## U28: Hidden treasure

In addition to adding aliases, profile files in Unix are very useful for many other reasons. Profile files are hidden files (or "dot" files). If a filename starts with a dot, Unix will treat it as a hidden file. To see it, you  can use `ls -a` which lists all hidden files (there may be several more files that appear) in addition to the other files.

#### Task U28.1
Navigate to your home directory. Use the appropriate command to list all files including hidden files. 

If any files appear that has "profile" in the name, use the `less` command to look at the contents. Any such file in your _home_ directory will be automatically read every time you open a new terminal. If you want to use the profile you created above, you have to remember to use the command `source profile` whenever you open a new terminal session. If you're in a different directory when you run the command, remember to alter the path accordingly.

#### Task U28.2
Open a new Terminal session and navigate to your Downloads folder (or any other folder besides your home directory). 

Use the `ls` command to see the contents of the folder.

Now use the `source profile` command, but specifying the correct path to "profile" (since you are not in your home directory where "profile" lives).

Again type the `ls` command. The output should now look different.


---

## U29: Sticking to the script

Unix can also be used as a programming language just like Python. Depending on what you want to do, a Unix script might solve all your problems and mean that you don't really need to learn Python at all.

So how do you make a Unix script (which are commonly called "shell scripts")? At the simplest level, we just write one or more  Unix commands to a file and then treat that file as if it was any other Unix command or program.

#### Task U29.1
Make a new directory in Desktop named "Code". Witin the Code directory, create a nano file named "hello.sh" containing the following two lines: 

	# my first Unix shell script 
	echo "Hello World"


#### Task U29.2
Type `bash hello.sh` and see what happens. It likely won't work! Use `ls -l` and look closely at the information in the first column. You'll see some r's and w's, but no x's (explained below). 

You need to run a command that gives the file permission to execute. Do this by typing:

`chmod u+x hello.sh` 

Type `ls -l` and look again at the first column. What changed?

Now run `bash hello.sh` again. What do you see?


#### Task U29.3
Now place a "dot-slash" in front of the name of the file you want to execute, like this:

	./hello.sh
---

The chmod command can also modify read and write permissions for files, and change any of the three sets of permissions (read, write, execute) at the level of "user", "group", and "other". You probably won't need to know any more about the chmod command other than you need to use it to make scripts executable.

---

## U30: The power of shell scripts

Time to make some Unix shell scripts that might actually be useful.

#### Task U30.1
Look around at some of the files in your messiest folder, which is likely either Downloads or Desktop. Identify some file types you'd like to organize into folders. For example, you might want to move all .txt files into a "Text" folder, and all .mp3 files into a "Music folder". Put the appropriate information into a shell script (using `nano`) and save it as cleanup.sh. Might look like this:

	#!/bin/bash
	mv *.txt Text 
	mv *.jpg Pictures 
	mv *.mp3 Music 
	
#### Task U30.2
First use `ls` to show the files before they are organized. Then give permission to `cleanup.sh` and run it. Use `ls` again to show the newly organized files.

Did you notice the `#!/bin/bash` line in this script? There are several different types of shells in Unix, and this line makes it clearer that a) that this is actually a file that can be treated as a program and b) that it will be a bash script (bash is a type of Unix shell). As a general rule, all types of scriptable programming languages should have a similar line as the first line in the program.

---
