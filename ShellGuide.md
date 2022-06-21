Hello and welcome to World of Code!

World of Code can be navigated using the Linux shell.
Let’s go over some of the most commonly used Linux shell commands together!

As a quick note this guide assumes you have used secure shell to connect to da0 by typing “ssh da0” on the appropriate command line as previously
stated in this World of Code tutorial and have done nothing else.

Once you are here you will be greeted by a prompt similar to this: “[wparham1@da0]~%” except instead of wparham1 you will see the username
registered to you during World of Code signup!

Let’s start learning how to navigate World of Code by using the shell.  If you are new to using a Linux terminal, I encourage you to follow along or follow
this link "https://www.hostinger.com/tutorials/linux-commands" to further familiarize yourself with shell commands.

*Note: In this tutorial I will type all commands in quotes but you should not include these quotes when operating with the shell unless specified otherwise.

Now that we are on the same page, let’s begin by typing the command “ls” without the quotes and pressing enter.
  •	The “ls” command stands for “list” and it will list all contents of a directory.
    o	The command can be further specified by using flags after the command.  For instance, let’s type the long flag denoted by “ls -l” next.
      This will give us a more verbose output telling us the privileges on the file in the first column, then the number of links to the file in 
      the second column, the owner of the file in the third column, then the group of people to who own the file in the fourth column, 
      the size of the file in bytes in the fifth column, the date the file was last changed in that directory in the sixth, seventh, and eight column, and 
      finally the file name in the ninth column.
      
    o	Now, let’s try typing the “all” flag denoted by “ls -a”. This will show all contents of a directory including files Linux would 
      normally hide from the user such as “.” And “..”.
      
    o	Now, let’s combine both the “ls -l” flag and the “ls -a” flag. This will result in the verbose, tabular listing of every file inside a directory. 
      This is meant to demonstrate that you can use any valid combination of flags together with any given Linux command! 
      
Now that you have closely examined a most likely blank directory lets go ahead and talk about creating files and directories from the command line.
  •	The first and arguably most useful command when creating files will be “mkdir [directory name]”. 
    Use this command for yourself by replacing the [directory name] with whatever you would like to name this directory. 
    For this example, I recommend naming it “shell_tutorial”. That means the command would look like this: “mkdir shell_tutorial”. 
    To confirm that you created the directory use the “ls” command we talked about above. Type “ls” into the terminal and press enter to 
    see your new directory, good job!
    
  •	Another common way to create files is by using the Vim text editor. The Vim text editor, while extremely handy, can be a little tricky to get used to.
    First, type “vim [file name]” into the terminal and press enter.  Again, feel free to replace [file name] with whatever you want 
    but I recommend replacing it with “vim_tutorial.txt”.  In that case, the command would look like this: “vim vim_tutorial.txt”.  
    This will open up a new window which will be the file you are editing! To edit the file first press the “i” key. 
    This tells vim to swap to text editor mode instead of edit mode.  Now type “This is a blank vim file.” and press the escape key on your keyboard.  
    The escape key tells Vim to take you out of text editor mode and puts you in edit mode.  Next, type “:wq”.  In vim, the “w” stands for save work and 
    the q stands for quit.  If you ever desire to quit without saving then type the command “:q!”.
    
  •	Another extremely useful and extremely common way to create a file is to copy it from an existing file!  
    This can be done by using the “cp” or “copy command”.  For instance, if we wanted to make a copy of “vim_tutorial.txt” named “vim_tutorial_copy.txt” all 
    we would have to do is type “cp vim_tutorial.txt vim_tutorial_copy.txt” and make sure that the file we want to copy is in the same directory we are in.
    You can create copies of a file from a different directory by specifying the path to the directory you wish to copy the file from. For instance, if 
    I am inside the directory “shell_tutorial” and I want to copy a file named “cool_file.txt” but it is one directory higher than “shell_tutorial” all 
    I would have to do is type: “cp ../cool_file.txt cool_file_copy.txt”
    
    o	*Note: You should be careful when creating a copy of a file as it copies the entire contents of a file so if you have very big files, you will be 
      making very large copies.
  •	The final command I would like to talk about here is the “mv” command which stands for “move command”.  
    This command lets you move a file from one location to another location or even rename a file!  For instance, if I wanted to move “vim_tutorial.txt” to 
    the directory above “shell_tutorial” I could navigate inside of “shell_tutorial” using the “cd” command then type the following command:       
    “mv vim_tutorial.txt ..”.  In this command the first parameter specifies the file in question and the second parameter specifies the location in question.
    If I wanted to move “vim_tutorial.txt” back inside of “shell_tutorial” all I would have to do is type:  “mv ../vim_tutorial.txt .”.  
    In this case, the first argument is the path to the file I wish to move and the second argument is the “.” (dot) specifying I want the file moved to 
    this directory.
    
    o	The final note I want to make on the “mv” command is its ability to rename files.  In order to do this all you need to do is type: 
      “mv [name of file to be changed] [new name]”.  If I wanted to change the name of “vim_tutorial.txt” to “awesome_vim.txt” I would type:
      mv “vim_tutorial.txt” “awesome_vim.txt”.
      
Now that we have created a file a few different ways, we can admire our hard work!  To do this let’s use the “cat”, “head”, and “tail” commands!
  •	First, let’s use “cat”.  This command stands for concatenate and will print the contents of a file to “standard out” (file descriptor 1) which is a fancy
    way to say it will print to terminal.  To use cat on the file we created type the following command: “cat vim_tutorial.txt”.  
    This will display the contents of the file “vim_tutorial.txt”.
    
    o	*Note: You should be careful when using “cat” as it can quickly flood your terminal with output if you cat a file that is too large.
    
  •	Second, let’s use “head” to display the content of our “vim_tutorial.txt” file.  “Head” stands for the “head” of a file.  
    It will print the first 10 lines of a file to standard out (stdout).  In order to use it on the file we just created type: “head vim_tutorial.txt”.
    
    o	Head also has flags associated with it the same way “ls” has the “-l” and “-a” flags.  Head lets the user specify how many lines from the top of the 
      file the user would like to see.  For instance, if we type “head -5 vim_tutorial.txt” only the first five lines of “vim_tutorial.txt” will be shown!  
      This is especially useful if you have a file so large that opening the file and looking at the first N arbitrary number of lines will take a long time
      as can be the case in World of Code.  The best part is that you can also specify a number of lines greater than 10 for head to print.  
      For instance, “head -1000 vim_tutorial.txt” would print the first one thousand lines of “vim_tutorial.txt” if it had that many lines!
      
  •	Finally, let’s use “tail” to display the content of our “vim_tutorial.txt” file.  “Tail” works like the opposite of head and stands for tail of file.
    It will try to print the final 10 lines of any given file.  In order to print “vim_tutorial.txt” go ahead and enter “tail vim_tutorial.txt”.
    
    o	Tail also has the ability to specify how many lines of output you desire from thee end of your file.  To do this use the -x flag where x is a 
      given number.  For instance, typing “tail -5 vim_tutorial.txt” would print the final five lines of “vim_tutorial.txt”.

Now that we know how to create files and directories let’s learn how to traverse these directories!  
The simplest way to traverse directories is by moving through them one directory at a time.  
To do this we can make use of the “cd” command commonly referred to as the “Change Directory” command.

  •	First, let’s navigate into the directory we created earlier!  In order to do this, we need to type “cd [name of existing directory]”.  
    If you named the directory “shell_tutorial” then the command would look like this: “cd shell_tutorial”.  
    Once you are in this directory you can once again make use to “ls” to see what is in it.  In this case I recommend using
    the “-a” flag (“ls -a”) in order to see the “.” And “..” folder.  These folders hold valuable information for traversing directories.  
    The singular dot “.” Means “this directory” while the double dots mean “previous directory”.
    
  •	Next, let’s navigate back to the directory we started in.  We can do this by typing   “cd ..”.  
    This command will move us back up one directory.
  
  •	Next let’s understand how the tilde (~) character works.  In Linux the tilde character will try to fill in relevant path information if it has not 
    been specified.  This can be used in a variety of ways including fast directory traversal.  I will cover it more as it becomes relevant.
    
  •	*Note: You can use the “cd ..” command from your home directory to move up a level and see everyone on that da server.  
    For instance, I am usually on da0.  If you wanted to move to my directory you could simply ssh into da0 and type “cd ../wparham1/” to see my home 
    directory and its contents.  If you wanted to go back to your directory from this location all you would need to do is type “cd ../[your username]/”

Now we can talk about deleting files and directories.  To do this we will use two commands: rmdir and rm.
  •	Starting with the rmdir command first, it can be used to remove empty directories.  To test this let’s go ahead and create a directory named 
    “short_lived_dir”.  To do this type “mkdir short_lived_dir”.  To make sure that the directory is there you can type “ls”.  
     Next, in order to delete the empty directory, we will type “rmdir_short_lived_dir”.  Upon checking using “ls” we can see that the directory 
     “short_lived_dir” no longer exists!
     
  •	Next, lets look at using the rm command.  This command can be used to both delete directories and files but let’s look at how to delete files first.
    o	To delete a file simply type “rm [filename]”.  However, you need to be careful when you do this because there is no undo.  
      Deleting anything in the shell is usually a permanent action.  For demonstration purposes let’s go ahead and create a copy of “vim_tutorial.txt” 
      named “short_lived_tutorial.txt”.  To do this go to the directory with “vim_tutorial.txt” and type: “cp vim_tutorial.txt short_lived_tutorial.txt”.  
      Next, we can use “ls” to check if our copy exists.  After confirming its existence, lets delete it.  
      To do this all you will need to enter is “rm short_lived_tutorial.txt”.  This will permanently delete the copy.
      
    o	To delete a directory with contents we will need to use the rm command with the -r flag.  This specifies a recursive directory traversal to 
    delete all content inside the directory.  To illustrate this let’s make a copy of the “shell_tutorial” directory.  
    We can do this by typing “cp -r shell_tutorial short_lived_tutorial”.  The -r flag is specifying a recursive copy allowing us to copy the entire 
    directory and its contents!  Now, to delete this directory we need to type: “rm -r short_lived_tutorial”.  
    This will immediately delete the directory specified and no longer grant access to any files it contained.
    
	  *Note:  You should ALWAYS double check that you are deleting the correct directory when you specify the -r flag.  If you specify the wrong directory you could delete your entire home directory losing all work that isn’t saved elsewhere on a different machine, GitHub repo, etc.  For this reason, you should never, under any circumstance, use the “.” or “..” folders when specifying a directory to delete.  It is much safer to navigate to the directory you want to delete and specify the directory name.

The next command I would like to talk about is the “grep” command.  The grep command allows a user to search through a file for a given string.
  •	For instance, if you wanted to search for the word “blank” inside of “vim_tutorial.txt” you could type 
    “grep blank vim_tutorial.txt”.  This will print all lines that contain the string “blank” to standard out.  
    This command is extremely useful for searching and filtering when using World of Code particularly in x2y type mappings.
    
  •	You can also combine grep with regex expressions to expand the range of what you could possibly grep.  
    A great example of this is: grep -iE ';code\W?(of)?\W?conduct'.  In this the -i flag specifies to ignore the case of the grepped expression and 
    the -E flag allows you to grep with a regex expression.
    
The next useful general purpose command is “wc” which is short for “word count”.  
Word count allows you to see how many lines, words, and bytes are in a file.  This, much like head and tail, is particularly useful for the
potentially huge files you can encounter when using World of Code (8 gb of project names anyone? I digress).
  •	Thankfully, “wc” is an extremely intuitive command to use.  If we call “wc” without any flags it will return to us the number of lines, words, and bytes 
    in a file in that order.  For instance, “wc vim_tutorial.txt” will result in the following output: “1  5  26  vim_tutorial.txt”.

•	We can also specify whether we want the number of lines using “-l”, the number of words using “-w”, and the number of characters using “-c”.  
  For instance, “wc -l vim_tutorial.txt” will result in the following output: “1  vim_tutorial.txt”.

Also worth mentioning is the “clear” command.  Many times, you will accidentally flood your terminal with output by catting a file that was too large or 
running a command to stdout that should have been redirected to a file.  When you do this it can be helpful to clear that output and get a clean terminal 
screen so you can better keep track of where you are in a directory, the last command you executed, etc.  
To do this all you need to do is type “clear” into the terminal and press enter.  This will give you a completely clean terminal to work in.

The next thing we will look at is simple output redirection.  With this anything that is printed to the terminal or stdout can be redirected to a file.  
This can be extremely useful if you don’t want to do formatted write to file calls in a programming language and instead would rather just write to standard
out and redirect it to a file!

  •	To understand this let’s start with an example.  Since we know how to cat a file to print its contents to standard out we can start there.  
    First, navigate to the file holding “vim_tutorial.txt” and then cat it by entering “cat vim_tutorial.txt”.  
    Once you have done that take it a step further and enter “cat vim_tutorial.txt > my_new_file.txt”.  
    Upon executing this command, you may notice that there is no output to the terminal.  This is because we have redirected 
    the output to the file specified after the “>”.  The file after the “>” will always be created or overwritten depending on the file’s 
    previous existence.  This can be important because it means you can very easily overwrite a file you have already created if you redirect into
    a file with the same name.  Watch out for this.
    
Now we will cover a few complicated but equally useful examples that pertain to World of Code specifically.
  •	The first thing we will cover is how to retrieve a list of all projects and deforked projects from version U of World of Code.
    o	This can be done by entering the following command on the command line: “zcat /da?_data/basemaps/gz/p2PU.s ”.  
      This will create a comprehensive list of all projects and map to their deforked counter parts separated by a “;”.  
      This makes it convenient to tokenize each line using your preferred programming language to look at each individual piece.  
      This also makes it convenient to use the “cut” command in linux to only grab the portion of the line you are interested in.  
      As a quick note this does not guarantee that each project is unique.  If you need a list of unique project you should pipe either your cut version 
      or the full version into the appropriate “uniq” Linux filter.
      
  *Note: An example of this will be added at a later date 

  •	The next thing we will cover is how to query and export a list from the MongoDB database contained in World of Code.  
  
    o	First, enter MongoDB.  If you are on da1 you simply type “mongo”.  If you are on a different server you can type “mongo –host “da1.eecs.utk.edu”.  
      Next, specify that we want to use World of Code by entering “use WoC”.  Now we can either interpret from A_metadata or P_metadata.  
      A_metadata stands for author meta data and P stands for project meta data.  In this example we will be using Project meta data or “P_metadata”.  
      To understand how to create this mongo export we can look at the following command: 
    	"mongoexport -h da5 -d WoC -c P_metadata.U -f ProjectID,NumActiveMon,NumAuthors,NumCommits,Gender -o dump_with_gender.csv --type=csv"
      
    o	While this command is very long it is thankfully quite easy to understand once it is broken down.  
      First, mongoexport tells the MongoDB database that we want to export a list from it.  The “-h” specifies that we want the host to be da5.  
      The “-d” specifies the WoC database.  The “-c” specifies the collection as P_metadata.U, and the -f specifies the fields we desire.  
      After the -f you should enter the fields you would like to be exported in that order without spaces.  
      The “-o” specifies a file to write to, in this case it is “dump_with_gender.csv”.  
      Finally, the “--type=csv” specifies that we want this file in a csv or comma separated value format.  
      This is the same way Excel files are formatted for those with Excel experience!

    o	A few important things to note about how mongo generates these files.  If mongo does not have all the information necessary to populate each field it 
      will fill the field in with ‘’.  This is important if you want to tokenize on commas and look at each value.  
      Also, when you specify gender sometimes Mongo will give you the number of females, males, or both.  It tries to give you all the information it
      has but the amount of information can be inconsistent.  It is good to be prepared for this issue.  
      Lastly, when you query Mongo in this way it will give you a result for every project it has meaning it will return a very large file.  
      Be prepared to wait a few minutes while it is generating this file and be prepared to interpret this file.  
      In many scenarios you cannot view it as just another Excel file because it will be too large for the Excel grid.  
      I have also had it crash my Visual Studios when trying to view it even after assigning an appropriate amount of memory.

  •	Finally, I would like to look at how we can fetch files off of World of Code without using a third party like a GitHub repoitory especially if the
    file is too big for GitHub and GitHub’s Large File Storage (LFS).
    
    o	To do this we can use the “scp” command from the terminal on our local pc.  This means that we do not run the following command on the WoC servers,
      we run it on our local machine. This is an example of me using scp on my command line: "scp wparham1@da0:~wparham1/dump_with_gender.csv ~/Downloads/"
      
    o	The “scp” command stands for “secure copy”.  In order to use it you will need ssh permission to the location you wish to copy from, in this case World
    of Code.  To use this command first specify your username then @ the server you wish to connect to.  
    The second parameter is the path of the file you wish to copy.  In my case this would be “~wparham1/dump_with_gender.csv”.  
    The second parameter is the location on your local computer you wish to copy this file to.  In my case this would be “~/Downloads/”.  The scp command in a
    generalized form would be as follows: scp [WoC username]@[WoC server you ssh into] [path to file you want to download] [path to location you want to download to].
