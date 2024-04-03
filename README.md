# Tutorial: basics of using WoC

Get updates or ask questions related to World of Code: https://discord.gg/fKPFxzWqZX

In order to provide you with the access to the systems, please fill
1. [WoC registration form](https://docs.google.com/forms/d/e/1FAIpQLSd4vA5Exr-pgySRHX_NWqLz9VTV2DB6XMlR-gue_CQm51qLOQ/viewform?vc=0&c=0&w=1&flr=0&usp=mail_form_link)

2. Please view [WoC Elements and Structure](https://youtu.be/c0uFPwT5SZI)

3. [Recording of the tutorial conducted on 2022-10-27](https://drive.google.com/file/d/1ytzOiOSgMpqOUm2XQJhhOUAxu0AAF_OH/view?usp=sharing) and [an older (possibly obsolete) on 2019-10-15](https://drive.google.com/file/d/14tAx2GQamR4GIxOc3EzUXl7eyPKRx2oU/view?usp=sharing) 

4. [WoC website](https://worldofcode.org)

### On using shell script

[A brief gude on use of bash and related tools](https://github.com/woc-hack/tutorial/blob/master/ShellGuide.md)

Please consider auditing this MOOC if you are not comfortable working with the terminal/shell script

[Unix Tools: Data, Software and Production Engineering](https://courses.edx.org/courses/course-v1:DelftX+UnixTx+1T2020/course/)

### Before the Tutorial: Access to da server(s)

To register for the hackathon/tutorial, please
generate a public key.
For Mac and unix users the instructions below would work, but for windows users 
the best option is to enable [ubuntu shell]](https://winaero.com/blog/how-to-enable-ubuntu-bash-in-windows-10) [other instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10) first, then follow instructions for unix/mac. Alternatively, 
[OpenSSH module for PowerShell](https://www.techrepublic.com/blog/10-things/how-to-generate-ssh-keys-in-openssh-for-windows-10/), or
[Git-Bash](https://docs.joyent.com/public-cloud/getting-started/ssh-keys/generating-an-ssh-key-manually/manually-generating-your-ssh-key-in-windows#Git-Bash) are other options.

To generated ssh key open a terminal window and run `ssh-keygen` command. Once
it completes it produces the `id_rsa.pub` and `id_rsa` files inside your $HOME/.ssh/ folder. 
Type
```
cat ~/.ssh/id_rsa.pub
```
That is your public ssh key you include in the WoC registration [form http://bit.ly/WoCSignup](http://bit.ly/WoCSignup). The form will ask you for the contents of `id_rsa.pub` and the
GitHub and BitBucket handles. You will receive a response to the
email provided in the form once your account is set up.

Set up your `~/.ssh/config` file so that you can login to one of the da servers without having to fully specify the server name each time:
```
Host *
  ForwardAgent yes

Host da0
	Hostname da0.eecs.utk.edu
	Port 443
	User YourUsername
	
```
Please note that access to remaining servers is similarly available. da2 and da3 have ssh port 22 (both are running worldofcode.org web server on the https port 443)

YourUsername is the login name you provided on the signup form. 
Logging in then becomes as simple as typing `ssh da0` in your terminal.

### Set up accounts for GitHub and Bitbucket

If you dont have these already, please setup an account on both
GitHub and Bitbucket (they will be needed to invite you to the
relevant repositories on GitHub&BitBucket).
GitHub: https://github.com/pricing  
BitBucket: https://bitbucket.org/account/signup/  

## Tutorial Objectives

Prepare for the hackathon, make sure connections work, 
get familiar with the basic functionality, and potential of WoC, 
start thinking on how to investigate global relationships
in open source.

### WoC Objectives

Do the hard work to enable research on global properties of FLOSS: 

* Census of all FLOSS
   - What is out there, of what kind, how much
   - Ability to select projects/developers/APIs for natural experiments/other empirical studies 
* Provide FLOSS-wide relationships
   - Technical dependencies (to run applications)
   - Tool dependencies (to build/test applications)
   - Code copying
   - Knowledge (and people) migration
   - API use and spread over time
* Data Cleaned/Augmented/Contextualized
   - Correction: Authors/Forks/Outliers
   - Augmentation: Dependencies/Linking to other data sources
   - Context: project types/expertise
* Big Data Analytics: Map entities to all related entities efficiently
* Timely: Targeting < 1 Quarter old analyzable snapshot of the entire FLOSS
* Community run
   - Hackathon will help determine community needs
   - [Hackathon Schedule](https://github.com/woc-hack/schedule)
* How to participate?
   - Hackathon registration [form](http://bit.ly/WoCSignup)
   - If you can not attend the hackathon but just want to try out WoC, please fill the hackathon form but indicate in the topic section is that you do not plan to attend the hackathon.

### What WoC Contains

![Workflow](https://github.com/woc-hack/tutorial/blob/master/Database-workflow.png)
![Content: Commits., trees, blobs, projects, authors](https://github.com/woc-hack/tutorial/blob/master/Database.png)

### Related background reading

- [Estimate the impact of your code](https://da2.eecs.utk.edu)
- [About WoC](https://bitbucket.org/swsc/overview/raw/master/pubs/WoC.pdf)
- [Overview of the Software Supply Chains](https://bitbucket.org/swsc/overview/src/master/README.md)
- [Details on WoC storage/APIs](https://bitbucket.org/swsc/lookup/src/master/README.md)
- [Fun Facts](https://bitbucket.org/swsc/overview/src/master/fun/README.md)

## Activity 1: Access to da server(s)

Log in: `ssh da0`.

Once you are in a da server, you will have an empty directory under `/home/username` where you can store your programs and files:  
```
-bash-4.2$ pwd
/home/username
-bash-4.2$ 
```

Set up your shell:
```
-bash-4.2$ echo 'exec bash' >> .profile
-bash-4.2$ echo 'export PS1="\u@\h:\w>"' >> ~/.bashrc
-bash-4.2$ . .bashrc
[username@da0]~%
```

You can also login to other da servers, but first need to set up an ssh key on these systems:
```
[username@da0]~% ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/niravajmeri/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/niravajmeri/.ssh/id_rsa.
Your public key has been saved in /home/niravajmeri/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:/UoJkpnx5mn8jx4BhcnQRUFfPq4qmC1MVLRJSjpYnpo niravajmeri@da0.eecs.utk.edu
The key's randomart image is:
+---[RSA 2048]----+
|    . o=o**.  .  |
|   + + o*+ . o   |
|  . = o.+   . o  |
|   o ..* o   . . |
|  E  .= S o   .  |
|      .= o + .   |
|     o += + o    |
|      =.oo =     |
|       . o*..    |
+----[SHA256]-----+
```
Once the key is generated add it to your .ssh/auhorized_keys
```
[username@da0]~% cat .ssh/id_rsa.pub >> .ssh/authorized_keys 
```

Now you can login to da4:
```
[username@da0]~% ssh da4
[username@da4]~% 
```

#### Exercise 1

Log in to da0 and clone two repositories that contain APIs to access WoC data
```
[username@da0]~% git clone https://bitbucket.org/swsc/lookup
[username@da0]~% git clone https://github.com/ssc-oscar/oscar.py
```

Log in to da4 from da0:
```
[username@da0]~% ssh da4
[username@da4]~% ls
...
[username@da4]~% exit
[username@da0]~%
```

**NOTE:** Make sure to access these directories and execute a `git pull` frequently to ensure you are working with latest updates.

## Activity 2: Shell APIs - Basic Operations

Shell APIs might be useful to accsess content of the commit, trees, blobs, 
to calculate the diff produced by a commit, etc. 

For more examples [see full API](https://bitbucket.org/swsc/lookup/src/master/README.md). 

Lets look at the commit 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a:

```
[username@da0]~% echo 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a | ~/lookup/showCnt commit 3
tree 464ac950171f673d1e45e2134ac9a52eca422132
parent dddff9a89ddd7098a1625cafd3c9d1aa87474cc7
author Warner Losh <imp@FreeBSD.org> 1092638038 +0000
committer Warner Losh <imp@FreeBSD.org> 1092638038 +0000

Don't need to declare cbb module.  don't know why I never saw
duplicate messages..
```


This commit has a tree and a parent commit and is created by 'Warner Losh <imp@FreeBSD.org>'. 
(parameter 3 defines that raw output needs to be produced)

Lets inspect the tree (the root folder of the project):
```
[username@da0]~% echo 464ac950171f673d1e45e2134ac9a52eca422132 | ~/lookup/showCnt tree
100644;a8fe822f075fa3d159a203adfa40c3f59d6dd999;COPYRIGHT
...
040000;6618176f9f37fa3e62f2efd953c07096f8ecf6db;usr.sbin
```

We may also want inspect the first element in the tree: blob representing file COPYRIGHT
```
[username@da0]~% echo a8fe822f075fa3d159a203adfa40c3f59d6dd999 | ~/lookup/showCnt blob
blob;40;2999;66321199427;66321199427;2999;a8fe822f075fa3d159a203adfa40c3f59d6dd999
# $FreeBSD$
#  @(#)COPYRIGHT  8.2 (Berkeley) 3/21/94
...
```

#### Important Note
In case you want to get the content of many objects (or look up
values for many keys), please use a single function invocation and
provide multiple keys/sha1s as standard input because each call so showCnt
and getValues may involve ssh to another server where the data
resides.
To separate content of separate blobs, you can ask showCnt to put
output on a single line, for example,
```
[username@da0]~% echo a8fe822f075fa3d159a203adfa40c3f59d6dd999 | ~/lookup/showCnt blob 1
```
produces a single line starting from sha1:
```
a8fe822f075fa3d159a203adfa40c3f59d6dd999;IyAkRnJlZUJTRCQKIwlAKCMpQ09Q....
```
The content of the blob is base64 encoded. (use python's base64.b64decode)
### Exercise 2

Determine the author of the parent commit for commit 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a

Hint 1: parent commit is listed in the content of commit 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a above
```
[username@da0]~% echo dddff9a89ddd7098a1625cafd3c9d1aa87474cc7 | ~/lookup/showCnt commit
```

### Summary 2

Synopsis:
```
~/lookup/showCnt commit|tree|blob
```
reads from the standard input sha1 of the corresponding objects and 
prints the content of these objects. 

## Activity 3 - Investigate the maps

We see the content of the copyright file above. Such files are often copied verbatim. Lets determine the first author who has created it (irrespective of a repository).
WoC has created this relationship and stored in b2fa (Blob to First Author) map:
```
[username@da0]~% echo a8fe822f075fa3d159a203adfa40c3f59d6dd999 |  ~/lookup/getValues b2a
a8fe822f075fa3d159a203adfa40c3f59d6dd999;1072910122;Warner Losh <imp@ccf9f872-aa2e-dd11-9fc8-001c23d0bc1f>;00a8f599c25ded714d2a4da9e1bb30e2a335181c
```
It turns out that it was created by commit 00a8f599c25ded714d2a4da9e1bb30e2a335181c done by what appears to be the same author on unix second 1072910122.

What is b2fa? The letters signify what keys (b - Blob) and values
(fa - first author) mean. As in natural sentence some decontextualization is needed in rare cases as this because f generally stands for file. Literally, that would mean b2fa is blob to file and author. As the number of objects and maps will multiply, single letters will not do and full word parsing will be used. 

**Primary Objects:**

* a = Author (A - aliased author)
* b = Blob  (b2c map will become obsolete as of version U as one can get more info from b2tac)
* c = Commit, cc - child commit and pc - parent commit
* f = File (occasionally its an adjective modifying the following object as in fa or First Author)
* p = Project (P - deforked project)
* t = Time (unix unsigned long in UTC)
* g = gender

**Capital Version** - simply means that the data has been corrected:   
* A = aliased version (see https://arxiv.org/abs/2003.08349); any organizational and group IDs, bot IDs, as well as author IDs, that do not
contain sufficient info to alias are excluded.
* P = deforked project (via Leuwen community
detection on commit / repo bi-graph: https://arxiv.org/abs/2002.02707)

We can inspect the relationships between a and A and also between p
and P:
```
[username@da0]~% echo 'Warner Losh <imp@FreeBSD.org>' | ~/lookup/getValues a2A
Warner Losh <imp@FreeBSD.org>;imp <imp@bsdimp.com>
[username@da0]~% 
[username@da0]~% echo 'imp <imp@bsdimp.com>' | ~/lookup/getValues A2a | tr ";" "\n" | head -n 3
imp <imp@bsdimp.com>
M. Warner Losh <imp@bsdimp.com>
M. Warner Losh <imp@freebsd.org>
```

Going back to blob we may ask if this blob has been widely copied as would be expected for copyright files. We can use b2tac to obtain sha1, blob to time, author, and commit. The following example pipes the output to only see the first entry:

```
[username@da0]~% echo a8fe822f075fa3d159a203adfa40c3f59d6dd999 |  ~/lookup/getValues b2tac | cut -d ";" -f1-4          
a8fe822f075fa3d159a203adfa40c3f59d6dd999;1072910122;Warner Losh <imp@FreeBSD.org>;121f970412fec7f9af0352a9b4ce8dca43bdb59e

```

b2tac (blob to time, author, commit) shows the numerous commits that introduced that blob in all repositories. We can further use commit to project map (c2p)
to identify all associated projects:
```
[lgonzal6@da0]~/tutorial% echo a8fe822f075fa3d159a203adfa40c3f59d6dd999 | ~/lookup/getValues b2tac | awk -F \; '{for(i=4;i<NF;i+=3){print $i}}' | ~/lookup/getValues -f c2p | cut -d ";" -f2 | sort -u |  head -3
0cjs_unix-history-repo
0mp_freebsd
0xbda2d2f8_freebsd
```
In fact there are 1719 distinct repositories where this blob appears.

Finally, we have the author 'Warner Losh <imp@FreeBSD.org>' for the commit we have investigated. 
Can we find what other commits Warner has made?:
```
[username@da0]~% echo 'Warner Losh <imp@FreeBSD.org>' | ~/lookup/getValues a2c
Warner Losh <imp@FreeBSD.org>;0000ce4417bd8d9a2d66a7a61393558d503f2805;000109ae96e7132d90440c8fa12cb7df95a806c6;00014b72bf10ad43ca437daf388d33c4fea73df9;000171c80d0d0ab6ff22b58b922e559e51485936;000282fc6c091e1c0abdadf1f58c088fc3ed9bc9;0002d1cab0d367c074a601e28183c60657254820;000373a8c48347c3e0e30486ccf4d9b043438826;...

```

In addition to variable-length records (key;val1,;val2;...;valn), 
the output can be produced as a flat table (key;val1\nkey;val2\n...\nkey;valn)
using -f option:
```
[username@da0]~% echo 'Warner Losh <imp@FreeBSD.org>' | ~/lookup/getValues -f a2c
Warner Losh echo 'Warner Losh <imp@FreeBSD.org>' | ~/lookup/getValues -f a2c| head
Warner Losh <imp@FreeBSD.org>;0000ce4417bd8d9a2d66a7a61393558d503f2805
Warner Losh <imp@FreeBSD.org>;000109ae96e7132d90440c8fa12cb7df95a806c6
...
```

In addition to the random lookup, the maps are also stored in flat
sorted files and this format is preffered (faster) when
investigating over two hundred thousand items or an entire WoC. 
For example, find commits by any author named Warner (a similar
task would be to find all blobs or commits involving a c-language
file ".c" or a README file "README"): 
```
[username@da0]~% zcat /da0_data/basemaps/gz/a2cFullV0.s | grep 'Warner'
```
As described below, the maps are split into 32 (or 128) parts to enable parallel search.
FullV means that we are looking ata  complete extract at version V. 

As versions keep being updated, and data no longer fits on a single server, 
a more flexible way to run the same command would be
```
[username@da0]~% zcat /da?_data/basemaps/gz/a2cFull?0.s | grep 'Warner'
```
In other words, we look for the file on any of the servers and selecting an arbitrary 
version of the datbase.

### Exercise 3

a) Find all files modified by author id 'Warner Losh <imp@FreeBSD.org>'

Hint 1: What is the map name?

Author ID to File or a2f
```
echo 'Warner Losh <imp@FreeBSD.org>' | ~/lookup/getValues a2f 
```

Find all commits  developers who have your last and your first name:

Hint 1: use wc (word count), e.g., 

```
[username@da0]~% zcat /da0_data/basemaps/gz/a2cFullP*.s | grep -i 'audris' | grep -i 'mockus' | wc -l
```

b) Find all files modified by all author IDs used by a developer 'Warner Losh <imp@FreeBSD.org>'

Hint 1: What is the map name?
A represents all author IDs so we first get the group name:
```
echo 'Warner Losh <imp@FreeBSD.org>' | ~/lookup/getValues a2A
Warner Losh <imp@FreeBSD.org>;imp <imp@bsdimp.com>
```

and then use it to get all files via A2f
```
echo 'imp <imp@FreeBSD.org>' | ~/lookup/getValues A2f 
```



### Summary 3
For any key provided on standard input, a list of values is provided
```
~/lookup/getValues [-f] a2c|c2dat|b2ta|b2fa|c2b|b2f|c2f|p2c|c2p|c2P|P2c
```
option -f replaces one output line per input line into the number of lines corresponding to the number of values. 
(or single-value maps such as c2dat, b2fa) -f makes no sense as it prints distinct fields on separate lines)

Also, only the first column of the input is considered as the key, other fields are passed through, e.g., 
```
echo 'Warner Losh <imp@FreeBSD.org>;zz' | ~/lookup/getValues -f a2c| head
Warner Losh <imp@FreeBSD.org>;zz;0000ce4417bd8d9a2d66a7a61393558d503f2805
Warner Losh <imp@FreeBSD.org>;zz;000109ae96e7132d90440c8fa12cb7df95a806c6
...
```

## Activity 4: Using Python APIs from oscar.py

**oscar.py Tutorial:** oscar.py has their own tutorial for hackathon purposes. We suggest that you go [here](https://github.com/ssc-oscar/oscar.py/blob/master/docs/tutorial.md) and read through it. The tutorial contains information about the current available functions, how to implement applications (simple and complex), and useful imports for applications.

**Important Note:** If you experience any difficulties in retrieving data from oscar.py's function calls (i.e., you receive an empty tuple on function return), please run `git pull` in your cloned repo to stay up-to-date with the latest version of oscar.py.   

These are corresponding functions in oscar.py that open the .tch files listed below for a given entity. "/<function_name>" after a function name denotes the version of that function that returns a Generator object.

1. `Author('...')`  - initialized with a combination of name and email
	* `.blobs`
	* `.commit_shas/commits`
	* `.project_names`
	* `.files`
	* `.torvald` - returns the torvald path of an Author, i.e, who did this Author work
				 with that also worked with Linus Torvald
2. `Blob('...')` -  initialized with SHA of blob
	* `.author` - returns timestamp, author name, and binary SHA of commit
	* `.commit_shas/commits` - commits removing this blob are not included
	* `.data` - content of the blob
	* `.file_sha(filename)` - compute blob sha from a file content
	* `.position` - get offset and length of blob data in storage
	* `.parent`
	* `.string_sha(string)`
	* `.tkns` - result of ctags run on this blob, if there were any
3. `Commit('...')` - initialized with SHA of commit
	* `.blob_shas/blobs`
	* `.child_shas/children`
	* `.changed_file_names/files_changed`
	* `.parent_shas/parents`
	* `.project_names/projects`
	* `.attributes` - time, tz, author, tree, parent(s)
	* `.tdiff`
5. Deprecated, see [#50](https://github.com/ssc-oscar/oscar.py/issues/50): `File('...')` - initialized with a path, starting from a commit root tree
	* `.authors`
	* `.blobs`
	* `.commit_shas/commits`
6. `Project('...')` - initialized with project name/URI
	* `.author_names`
	* `.commit_shas/commits`	
7. `Tdiff('...')` - initialized with SHA, result of diff run on 2 blobs (if there was a diff)
	* `.commit`
	* `.file`
8. `Tree('...')` - representation of a git tree object (dir), initialized with SHA of tree
	* `.files`
	* `.blob_shas/blobs`
	* `.commit_shas/commits`
	* `.traverse`

The non-Generator version of these functions will return a tuple of items which can then be iterated:
```
for commit in Author(author_name).commit_shas:
	print(Commit(commit))
```

### Exercise 4a:  Get a list of commits made by a specific author:  

Install the latest oscar.py
```
[username@da0]~% cd ~/oscar.py
```
If "import oscar" fails
```
[username@da0]~% easy_install --user clickhouse-driver
```

As we learned before, we can do that in shell
```
[username@da0]~% zcat /da?_data/basemaps/gz/a2cFullU0.s | grep '"Albert Krawczyk" <pro-logic@optusnet.com.au>'
"Albert Krawczyk" <pro-logic@optusnet.com.au>;17abdbdc90195016442a6a8dd8e38dea825292ae
"Albert Krawczyk" <pro-logic@optusnet.com.au>;9cdc918bfba1010de15d0c968af8ee37c9c300ff
"Albert Krawczyk" <pro-logic@optusnet.com.au>;d9fc680a69198300d34bc7e31bbafe36e7185c76
```

Now the same thing can be done using oscar.py:  
```
[username@da0]~% cd oscar.py
[username@da0:oscar.py]~% python3
>>> from oscar import Author, Commit
>>> for commit in Author('"Albert Krawczyk" <pro-logic@optusnet.com.au>').commit_shas: print(Commit(commit))
17abdbdc90195016442a6a8dd8e38dea825292ae
9cdc918bfba1010de15d0c968af8ee37c9c300ff
d9fc680a69198300d34bc7e31bbafe36e7185c76
```

### Exercise 4b: Get the URL of a projects repository using the oscar.py `Project(...).url` attribute:  
```
[username@da0:oscar.py]~%  python3
>>> from oscar import Project
>>> Project('notcake_gcad').url
'https://github.com/notcake/gcad'
```

### Exercise 4c

Get list of files modified by commit 17abdbdc90195016442a6a8dd8e38dea825292ae

Hint 1: What class to use?
Commit
```
[username@da0:oscar.py]~%  python3
>>> from oscar import Commit
>>> Commit('17abdbdc90195016442a6a8dd8e38dea825292ae').changed_file_names
```

## Activity 5: Understanding Servers and folders

All home folders are on da2, so it is preferred not to do very large
file operations to/from these folders  when running tasks on servers
other than da2,
since these operations will load NFS and may slow access to home
folders of other users.

Each server has /data/play folder where you can create your
subfolders to store/process large files.

### List of relevant directories

Not all files are stored on all servers due to limited disk sizes
and different speed of disks (fast refers to SSDs).
The location of the file can be identified via a pathname as described below. 

### da0/../da5 Servers
#### <relationship>.{0-31}.tch files can be found in `/da[0-5]_fast/` or `/da[0-5]_data/basemaps`  
(.s) signifies that there are either .s or .gz versions of these files in /da[0-5]_data/basemaps/gz/ folder, which can be opened with Python gzip module or Unix zcat.  
all five da[0-5] server may have these .s/.gz files  
Keys for identifying letters:   

* a = Author
* b = Blob
* c = Commit
* cc = Child Commit
* f = File
* h = Head Commit
* ob = Parent Blob
* p = Project
* pc = Parent Commit
* P = Forked/Root Project (see Note below)
* ta = Time;Author
* fa = First;Author;commit
* r = root commit obtained by traversing commit history
* h = head commit obtained by traversing commit history
* td = Tdiff
* tk = Tokens (ctags)
* trp = Torvalds Path

Version T keys for identifying letters:
* L = LICENSE* files
* Lb - blobs that are shared among fewer than 100 Projects
* fb = firstblob
* tac = time, author, commit
* t = root tree

Recall that the captal version of author A means aliased version (see
https://arxiv.org/abs/2003.08349) and it also means that
organizational and group IDs, bot IDs as well as author IDs that do not
contain sufficient info to alias are excluded.
Similarly, the capital version of project P represents a deforked project (via Leuwen community
detection on commit / repo bi-graph: https://arxiv.org/abs/2002.02707)

List of relationships:
```
* a2b 		* a2c (.s)	* a2f		* a2ft		
* a2p (.s)	* a2trp0 (.s)
* b2a		* b2tac (.s) 	* b2f (.s)	* b2ob		* ob2b
* b2tk
* c2b (.s)	* c2cc		* c2f (.s)	* c2h		* c2pc
* c2p (.s)	* c2P		* c2ta (.s)	* c2td
* p2a (.s)	* p2c (.s)	* P2c
* td2c		* td2f
```

Special relationships (names do not correspond to keys):
```
Versions T or U:
b2f[aA]  - blob to time, author, commit for the first commit creating that blob
b2tac  - blob to time, author, commit for all commits creating that blob
bb2cf  - result of diff on a commit: blob old blob, commit, file
obb2cf - see bb2cf but blobs reversed
c2fbb  - result of diff on a commit: commit file, blob, old blob

P2core - Project to devs who make 80+% of the commits

b2fLICENSE - grep for LICESNSE in b2f
bL2P - license blob to project

c2dat - full commit data in semicolon-separated fields

dl2Pf - API defined; language; project; file
====
```

Note: c2P returns the most central repository for this commit, and does not include repos that forked off of this commit. 
      P2c returns ALL commits associated with this repo, including commits made to forks of this particular repo. 
	  The list of relationships is not exhaustive and more information can be found at https://github.com/woc-hack/tutorial/issues/17#issuecomment-850823408

### Exercise 5

Find all blobs associated with Julia language files (extension .jl) 

Hint 1: What is the name of the map?

```
[username@da0] zcat /da?_data/basemaps/gz/b2fFullU*.s | grep '\.jl;'
```

## Activity 6: Investigating Technical dependencies

The technical dependencies have been extracted by parsing the content of all blobs related to 
several different languages: and, for version V, are located in
`/da7_data/basemaps/gz/c2PtAbflPkgFullVX.s` with X ranging from 0
to 127 based on the 7 bits in the first byte of the commit sha1. 


The format of each file is encoded in its name: 
```
commit;deforked repo;timestamp;Aliased author;blob;filename;language (as used in WoC);module1;module2;...`  

```
for example
```
000000000fcd56c8536abd09cac5f2a54ba600c2;not-an-aardvark_lucky-commit;1510643197;Teddy Katz <teddykatz@fb.com>;d9730ab3fca05f4d61e7225de5731868cfb99fb6;lucky-commit.c;C;errno.h;string.h;math.h;zlib.h;stdio.h;sha.h;stdbool.h;stdlib.h;stat.h
```

Unlike in version R where each language had a separate thruMaps
directory, info on all languages is kept in a single place. 

To identify the implementation of various packages one can use
`/da?_data/basemaps/gz/c2PtAbflDefFullUX.s` with X ranging from 0
to 127 based on the 7 bits in the first byte of the commit sha1. 
for example
```
zcat /da?_data/basemaps/gz/c2PtAbflDefFullU0.s|head
0000000000abc668c5388237320e97d0dadae7b1;not-an-aardvark_lucky-commit;1613716402;Teddy Katz <teddykatz@fb.com>;050e87971a0a069043821c8d5f0c55d1f4761edc;Cargo.toml;Rust;lucky_commit
0000000000abc668c5388237320e97d0dadae7b1;not-an-aardvark_lucky-commit;1613716402;Teddy Katz <teddykatz@fb.com>;61aebdecc47b2b7521a353b1cc180b2af1080977;Cargo.lock;Rust;addr2line
```
Instead of the list of dependencies the last field identifies the
package implemented within the blob, specifically, lucky_commit and
addr2line in the above two blobs.

The Def relationship in WoC tracks blobs that define a package based 
on the content of the source code. There is no guarantee that only one project will have it due to copying and other reasons. Identifying the which repository is the true upstream one may not be that difficult, however. 

Def relationship points only to blobs that define the package (e.g., blobs for file setup.py in Python, packages.json in JavaScript, etc.). This can be used to identify 
repositories (or parts of the repositories) where these package metafiles reside.   	
	
*TODO*:put it into clickhouse to speed up access. 

Lets get a list of commits and repositories that imported Tensorflow for .py files:  
```
[username@da0]~%zcat c2PtAbflPkgFullU76.s |grep tensorflow|head -2
000005efe300482514d70d44c5fa922b34ff79a5;Rayhane-mamah_Tacotron-2;1557284915;qq443452099 <47710489+qq443452099@users.noreply.github.com>;05604b3f0632e98cc0eee3afef589dc5031f3a43;tacotron/synthesizer.py;PY;tacotron.utils.text.text_to_sequence;tacotron.utils.plot;tacotron.models.create_model;wave;datasets.audio;os;librosa.effects;tensorflow;infolog.log;datetime.datetime;io;numpy
000005efe300482514d70d44c5fa922b34ff79a5;Rayhane-mamah_Tacotron-2;1557284915;qq443452099 <47710489+qq443452099@users.noreply.github.com>;49bc3b8b6533b93941223ccbeb401e47e5a573d7;hparams.py;PY;tensorflow;numpy
```

### Exercise 6

Find all repositories using Julia language that import package 'StaticArrays'


Hint 1: What file to look for?
```
[username@da0]~% zcat /da?_data/basemaps/gz/c2PtabllfPkgFullS*.s | grep ';jl;' | grep StaticArrays
```

Hint 2: What field contains the repository name?
```
[username@da0]~% zcat /da?_data/basemaps/gz/c2PtabllfPkgFullS*.s | grep ';jl;'| grep StaticArrays | cut -d\; -f2 | sort -u
```

## Activity 7: Investigating copy-based reuse

WoC's operationalization of copy-based supply chains is based on mapping blobs 
(versions of the source code) to all commits and projects where they have been created. 
For each blob, all commits are sorted based on their timestamp and the project in which
the first commit exists is identified as the originator and all other projects as the reuser 
of that blob. These files are located in `/da?_data/basemaps/gz/Ptb2PtFullVX.s` with X ranging from 0
to 127 based on the 7 bits in the first byte of the blob sha1. 

The format of each file is encoded in its name: 
```
originating repo;timestamp;blob;destination repo;timestamp  

```
for example
```
zhunengfei_ExtJS6.2-samples;1466402956;00000056a59bde3926f65c334caef688ccad0a08;bitbucket.org_mastercad_sencha_demo;1551632725
```
This means that blob 00000056a59bde3926f65c334caef688ccad0a08 was first seen in zhunengfei_ExtJS6.2-samples at 1466402956
and was reused by bitbucket.org_mastercad_sencha_demo at 1551632725.

## Activity 8: Suggested by the audience

Find all projects that have commits mentioning "sql injection"

List of commits is on /da4_data/All.blobs/
Lets login to da4, create a data folder to store temporary data on the same server
"/data/play/username", and uce pcommit to project map to get the list of projects.

```
[username@da0]~% ssh da4
[username@da4]~% mkdir /data/play/audris
[username@da4]~% cd /data/play/audris
[username@da4:/data/play/audris]~% cut -d\; -f4 commit_*.idx | ~/lookup/showCmt.perl 2 | grep -i 'sql injection' > 
[username@da4:/data/play/audris]~% cut -d\; -f1 sql_inject | ~/lookup/getValues.perl /da0_data/basemaps/c2pFullP > sql_inject.c2p
```

## Activity 9: Summary of the activities undertaken

* Shell API (faster) and Python API (also Perl API not illustrated) for random access

* Sorted compressed tables for sweeps (grep)

* Key-Value maps to link authors, commits, files, projects, and blobs

* Overview of naming conventions server/data/databases

* Mongodb tables with the summary information about authors and projects to enable selection of subsets for later analysis: (e.g, I want authors with at least 100 commits who worked no less than three years and participated in at least five java projects.)

### Summary exercise

* What type of usability improvements are needed?

* What types of tasks would you like to work during the hackathon?

* What would make you a long-time user of WoC?


## Self paced part of the tutorial

The remainig activities are provided to illustrate various realistic 
tasks. 

## Activity S0: Finding 1st-time imports for AI modules (Simple)

Given the data available, this is a fairly simple task. Making an application to detect the first time that a repo adopted an AI module would give you a better idea as to when it was first used, and also when it started to gain popularity.  

A good example of this is in
[popmods.py](https://github.com/ssc-oscar/aiframeworks/blob/master/popmods.py). In
this application, we can read all 128 c2PtabllfPkgFullS*.s
files and look for a given module with the earliest import times. The program then creates a <module_name>.first file, with each line formatted as `repo_name;UNIX_timestamp`.  

TODO: update popmods.py to work with c2PtabllfPkgFullS*.s
Usage: `[username@da0]~%  python popmods.py language_file_extension module_name`  

Before anything else (and this can be applied to many other
programs), you want to know what your input looks like ahead of time
and know how you are going to parse it.
Since each line of the file has this format:  
```
commit;deforked repo;timestamp;author;blob;language (as used in WoC);language (as determined by ctags);filename;module1;module2;...
```

We can use the `string.split()` method to turn this string into a list of words, split by a semicolon (;).  
By turning this line into a list, and giving it a variable name, `entry = ['commit', 'repo_name', 'timestamp', ...]`, we can then grab the pieces of information we need with `repo, time = entry[1], entry[2]`. 

An important idea to keep in mind is that we only want to count unique timestamps once. This is because we want to account for repositories that forked off of another repository with the exact timestamp of imports. An easy way to do this would be to keep a running list of the times we have come across, and if we have already seen that timestamp before, we will simply skip that line in the file:  
```
...
if time in times:
	continue
else:
	times.append(time)
...
```
We also want to find the earliest timestamp for a repository importing a given module. Again, this is fairly simple:  
```
...
if repo not in dict.keys() or time < dict[repo]:
	for word in entry[5:]:
		if module in word:
			dict[repo] = time
			break
...
```
#### Implementing the application

Now that we have the .first files put together, we can take this one step further and graph a modules first-time usage over time on a line graph, or even compare multiple modules to see how they stack up against each other. [modtrends.py](https://github.com/ssc-oscar/aiframeworks/blob/master/modtrends.py) accomplishes this by:  

* reading 1 or more .first files 
* converting each timestamp for each repository into a datetime date
* "rounding" those dates by year and month
* putting those dates in a dictionary with `dict["year-month"] += 1`
* graphing the dates and frequencies using matplotlib.

If you want to compare first-time usage over time for Tensorflow and
Keras for the .ipynb language .first files you created, run:
```
UNIX> python3.6 modtrends.py tensorflow.first keras.first
```
The final graph looks something like this:  
[![Tensorflow vs Keras](../ipynb_first/Tensorflow-vs-Keras.png "Tensorflow vs Keras")](https://github.com/ssc-oscar/aiframeworks/blob/master/charts/ipynb_charts/Tensorflow-vs-Keras.png)



## Activity S1: Detecting percentage language use and changes over time (Complex) 

An application to calculate this would be useful for seeing how different authors changed languages over a range of years, based on the commits they have made to different files.  
In order to accomplish this task, we will modify an existing program from the swsc/lookup repo ([a2fBinSorted.perl](https://bitbucket.org/swsc/lookup/src/master/a2fBinSorted.perl)) and create a new program ([a2L.py](https://bitbucket.org/swsc/lookup/src/master/a2L.py)) that will get language counts per year per author.  

#### Part 1 -- Modifying a2fBinSorted.perl
For the first part, we look at what a2fBinSorted.perl currently does: it takes one of the 32 a2cFullP{0-31}.s files thru STDIN, opens the 32 c2fFullO.{0-31}.tch files for reading, and writes a corresponding a2fFullP.{0-31}.tch file based on the a2c file number. The lines of the file being `author_id;file1;file2;file3...`  

Example usage: `UNIX> zcat /da0_data/basemaps/gz/a2cFullP0.s | ./a2fBinSorted.perl 0`

We can modify this program so that it will write the earliest commit dates made by that author for those files, which will become useful for a2L.py later on. To accomplish this, we will have the program additionally read from the c2taFullP.{0-31}.tch files so we can get the time of each commit made by a given author:  
```
my %c2ta;
for my $s (0..($sections-1)){
	tie %{$c2ta{$s}}, "TokyoCabinet::HDB", "/fast/c2taFullP.$s.tch", TokyoCabinet::HDB::OREADER |
	TokyoCabinet::HDB::ONOLCK,
	   16777213, -1, -1, TokyoCabinet::TDB::TLARGE, 100000
	or die "cant open fast/c2taFullP.$s.tch\n";
}
```
We will also ensure the files to be written will have the relationship a2ft as oppposed to a2f:  
```
my %a2ft;
tie %a2ft, "TokyoCabinet::HDB", "/data/play/dkennard/a2ftFullP.$part.tch", TokyoCabinet::HDB::OWRITER | 
     TokyoCabinet::HDB::OCREAT,
	16777213, -1, -1, TokyoCabinet::TDB::TLARGE, 100000
	or die "cant open /data/play/dkennard/a2ftFullP.$part.tch\n";
```
Another important part of the file we want to change is inside the `output` function:  
```
sub output {
	my $a = $_[0];
	my %fs;
	for my $c (@cs){
		my $sec =  segB ($c, $sections);
		if (defined $c2f{$sec}{$c} and defined $c2ta{$sec}{$c}){
			my @fs = split(/\;/, safeDecomp ($c2f{$sec}{$c}, $a), -1);
			my ($time, $au) = split(/\;/, $c2ta{$sec}{$c}, -1);  #add this for grabbing the time
			for my $f (@fs){
				if (defined $time and (!defined $fs{$f} or $time < $fs{$f})){ #modify condition to grab earliest time
					$fs{$f} = $time;
				}
			}
		}
	}
	$a2ft{$a} = safeComp (join ';', %fs); #changed
}
```
Now when we run the new program, it should write individual a2ftFullP.{0-31}.tch files with the format:  
`author_id;file1;file1_timestamp;file2;file2_timestamp;...`  

We can then create a new PATHS dictionary entry in oscar.py, as well as making another function under the Author class to read our newly-created .tch files:  
```
In PATHS dictionary:
...
'author_file_times': ('/data/play/dkennard/a2ftFullP.{key}.tch', 5)
...

In class Author(_Base):
...
@cached_property
def file_times(self):
	data = decomp(self.read_tch('author_file_times'))
	return tuple(file for file in (data and data.split(";")))
...
```

#### Part 2 -- Creating a2L.py
Our next task involves creating a2LFullP{0-31}.s files utilizing the new .tch files we have just created. We want these files to have each line filled with the author name, each year, and the language counts for each year. A possible format could look something like this:  
`"tim.bentley@gmail.com" <>;year2015;2;py;31;js;30;year2016;1;py;29;year2017;8;c;2;doc;1;py;386;html;6;sh;1;js;3;other;3;build;1`  
where the number after each year represents the number of languages used for that year, followed by pairs of languages and the number of files written in that language for that year. As an example, in the year 2015, Tim Bentley made initial commits to files in 2 languages, 31 of which were in Python, and 30 of which were in JavaScript.  

There is a number of things that have to happen to get to this point, so lets break it down:  

* Iterating Author().file_times and grouping timestamps into year 

We will start by reading in a a2cFullP{0-31}.s file to get a list of authors, which we then hold as a tuple in memory and start building our dictionary:  
```
a2L[author] = {}
file_times = Author(author).file_times
for j in range(0,len(file_times),2):
	try:
		year = str(datetime.fromtimestamp(float(file_times[j+1]))).split(" ")[0].split("-")[0]
	#have to skip years either in the 20th century or somewhere far in the future
	except ValueError:
		continue
	#in case the last file listed doesnt have a time
	except IndexError:
		break
	year = specifier + year #specifier is the string 'year'
	if year not in a2L[author]:
		a2L[author][year] = []
	a2L[author][year].append(file_times[j])
```
The datetime.fromtimestamp() function will turn this into a datetime format: `year-month-day hour-min-sec` which we split by a space to get the first half `year-month-day` of the string, and then split again to get `year`.  

* Detecting the language of a file based on file extension
```
for year, files in a2L[author].items():
	build_list = []
	for file in files:
		la = "other"
		if re.search("\.(js|iced|liticed|iced.md|coffee|litcoffee|coffee.md|ts|cs|ls|es6|es|jsx|sjs|co|eg|json|json.ls|json5)$",file):
			la = "js"
		elif re.search("\.(py|py3|pyx|pyo|pyw|pyc|whl|ipynb)$",file):
			la = "py"
		elif re.search("(\.[Cch]|\.cpp|\.hh|\.cc|\.hpp|\.cxx)$",file):
			la = "c"
	.......
```
The simplest way to check for a language based on a file extension is to use the re module for regular expressions. If a given file matches a certain expression, like `.py`, then that file was written in Python. `la = other` if no matches were found in any of those searches. We then keep track of these languages and put each language in a list `build_list.append(la)`, and count how many of those languages occurred when we looped through the files `build_list.count(lang)`. The final format for an author in the a2L dictionary will be `a2L[author][year][lang] = lang_count`.  

* Writing each authors information into the file

See [a2L.py](https://bitbucket.org/swsc/lookup/src/master/a2L.py) for how information is written into each file.  

Usage: `UNIX> python a2L.py 2` for writing `a2LFullP2.s`  

#### Implementing the application
Now that we have our a2L files, we can run some interesting statistics as to how significant language usage changes over time for different authors. The program [langtrend.py](https://bitbucket.org/swsc/lookup/src/master/langtrend.py) runs the chi-squared contingency test (via the stats.chi2_contingency() function from scipy module) for authors from an a2LFullP{0-31}.s file and calculates a p-value for each pair of years for each language for each author.   
This p-value means the percentage chance that you would find another person (say out of 1000 people) that has this same extreme of change in language use, whether that be an increase or a decrease. For example, if a given author editied 300 different Python files in 2006, but then editied 500 different Java files in 2007, the percentage chance that you would see this extreme of a change in another author is very low. In fact, if this p-value is less than 0.001, then the change in language use between a pair of years is considered "significant".  

In order for this p-value to be a more accurate approximation, we need a larger sample size of language counts. When reading the a2LFullP{0-31}.s files, you may want to rule out people who dont meet certain criteria:  

* the author has at least 5 consecutive years of commits for files
* the author has edited at least 100 different files for all of their years of commits

If an author does not meet this criteria, we would not want to consider them for the chi-squared test simply because their results would be "uninteresting" and not worth investigating any further.  

Here is one of the authors from the programs output:  
```
----------------------------------
Ben Niemann <pink@odahoda.de>
{ '2015': {'doc': 3, 'markup': 2, 'obj': 1, 'other': 67, 'py': 127, 'sh': 1},
	'2016': {'doc': 1, 'other': 23, 'py': 163},
	'2017': {'build': 36, 'c': 116, 'lsp': 1, 'other': 81, 'py': 160},
	'2018': { 'build': 12,
		'c': 134,
		'lsp': 2,
		'markup': 2,
		'other': 133,
		'py': 182},
	'2019': { 'build': 13,
		'c': 30,
		'doc': 8,
		'html': 10,
		'js': 1,
		'lsp': 2,
		'markup': 16,
		'other': 67,
		'py': 134}}
	pfactors for obj language
		2015--2016 pfactor == 0.9711606775110577  no change
	pfactors for doc language
		2015--2016 pfactor == 0.6669499228133753  no change
		2016--2017 pfactor == 0.7027338745275937  no change
		2018--2019 pfactor == 0.0009971248193242038  rise/drop
	pfactors for markup language
		2015--2016 pfactor == 0.5104066960256399  no change
		2017--2018 pfactor == 0.5532258789014389  no change
		2018--2019 pfactor == 1.756929555308731e-05  rise/drop
	pfactors for py language
		2015--2016 pfactor == 1.0629725495084215e-07  rise/drop
		2016--2017 pfactor == 1.2847558344252341e-25  rise/drop
		2017--2018 pfactor == 0.7125543569718793  no change
		2018--2019 pfactor == 0.026914075872778477  no change
	pfactors for sh language
		2015--2016 pfactor == 0.9711606775110577  no change
	pfactors for other language
		2015--2016 pfactor == 1.7143130378377696e-06  rise/drop
		2016--2017 pfactor == 0.020874234589765908  no change
		2017--2018 pfactor == 0.008365948846657284  no change
		2018--2019 pfactor == 0.1813919210757513  no change
	pfactors for c language
		2016--2017 pfactor == 2.770649054044977e-16  rise/drop
		2017--2018 pfactor == 0.9002187643203734  no change
		2018--2019 pfactor == 1.1559110387953382e-08  rise/drop
	pfactors for lsp language
		2016--2017 pfactor == 0.7027338745275937  no change
		2017--2018 pfactor == 0.8855759560371912  no change
		2018--2019 pfactor == 0.9944669523033288  no change
	pfactors for build language
		2016--2017 pfactor == 4.431916568235125e-05  rise/drop
		2017--2018 pfactor == 5.8273175348446296e-05  rise/drop
		2018--2019 pfactor == 0.1955154860787908  no change
	pfactors for html language
		2018--2019 pfactor == 0.0001652525618661536  rise/drop
	pfactors for js language
		2018--2019 pfactor == 0.7989681687355706  no change
----------------------------------
```  

Although it is currently not implemented, one could take this one step further and visually represent an authors language changes on a graph, which would be simpler to interpret as opposed to viewing a long list of p values such as the one shown above.  

## Activity S2: Useful Python imports for applications
### subprocess
Simlar to the C version, system(), this module allows you to run UNIX processes, and also allows you to gather any input, output, or error from those processes, all from within a Python script. This module becomes especially useful when you are looking for specific lines out of a .s/.gz file, as opposed to reading the entire file which takes more time.  
A good example usage for subprocess is when we read the c2bPtaPkgO$LANG.{0-31}.gz files for first-time AI module imports in popmods.py. Rather than reading one of these files in its entirety, we look for lines of the file that have a specific module we are looking for.  
```
for i in range(32):
	print("Reading gz file number " + str(i))
	command = "zcat /data/play/" + dir_lang + "thruMaps/c2bPtaPkgO" + dir_lang + "." + str(i) + ".gz"
	p1 = subprocess.Popen(command, shell=True, stdout=subprocess.PIPE)
	p2 = subprocess.Popen("egrep -w " + module, shell=True, stdin=p1.stdout, stdout=subprocess.PIPE) 
	output = p2.communicate()[0]
```
We can then iterate the lines of this output accordingly, and gather the pieces of information we need:  
```
for entry in str(output).rstrip('\n').split("\n"):
	entry = str(entry).split(";")
	repo, time = entry[1], entry[2]
```

Additional documentation on subprocess can be found [here](https://docs.python.org/2/library/subprocess.html).  

----------
### re
The re (Regular Expression) module is another useful import for pattern-matching in strings.  
Addtional re documentation can be found [here](https://docs.python.org/2/library/re.html).  

----------
### matplotlib
Useful graphing module for creating visual representations.  
Extensive documentation can be found [here](https://matplotlib.org/).  

## Activity S3: a comparison of oscar.py vs. Perl scripts

When it comes to creating new relationship files (.tch/.s files), using Perl over Python for large data-reading is more time-saving overall. This situation occurred in the complex application we covered where we modified an existing Perl file to get the initial commit times of each file for each author, rather than using Python to accomplish this task.  
Before making this decision, one of our team members decided to run a test between 2 programs, [a2ft.py](https://bitbucket.org/swsc/lookup/src/master/a2ft.py) and [a2ft.perl](https://bitbucket.org/swsc/lookup/src/master/a2ft.perl). These programs were run at the same time for a period of 10 minutes. Both programs had the same task of retrieving the earliest commit times for each file under each author from a2cFullP{0-31}.s files. The Python version calls the `Commit_info().time_author` and `Commit().changed_file_names` functions from oscar.py. The Perl version ties each of the 32 c2fFullO.{0-31}.tch (Commit().changed_file_names) and c2taFullP.{0-31}.tch (Commit_info().time_author) files into 2 different Perl hashes (Python dictionary equivalent), %c2f and %c2ta. The speed difference between Perl and Python was quite surprising:  

```
[username@da3]/data/play/dkennard% ll a2ftFullP0TEST1.s
-rw-rw-r--. 1 dkennard dkennard 980606 Jul 22 11:56 a2ftFullP0TEST1.s
[username@da3]/data/play/dkennard% ll a2ftFullPTEST2.0.tch
-rw-r--r--. 1 dkennard dkennard 663563424 Jul 22 11:56 a2ftFullPTEST2.0.tch
```  

Within this 10 minute period, the Python version only wrote 980,606 bytes of data into the TEST1 file shown above, whereas the Perl version wrote 663,563,424 bytes into the TEST2 file.  
The main reason oscar.py is slower, in theory, is because oscar.py has more private function calls that it has to perform in order to calculate the key (0-31) and locate where the requested information is stored. Upon further inspection of the [oscar.py](https://github.com/ssc-oscar/oscar.py/blob/master/oscar.py) functions that are called, we can see that there are between 6-7 function calls for each lookup. All of these function calls cause function overhead and thus increase the amount of time to retrieve data for multiple entities.  
In the Perl version of a2ft, the program simply calls `segB()`, which calculates the key of where the information is stored. The function takes a string and the number 32 as arguments (ex. segB(commit_sha, 32)):   

```
sub segB {
	my ($s, $n) = @_;
	return (unpack "C", substr ($s, 0, 1))%$n;
}
```  

Because the %c2f and %c2ta Perl hashes are tied to their respective .tch files, we can then check if a specific commit in a specific number section is defined:  

```
for my $c (@cs){	#where cs is a list of commits for an author and c is one of those commits
	my $sec =  segB ($c, $sections);
	if (defined $c2f{$sec}{$c} and defined $c2ta{$sec}{$c}){
		...
	}
	...
}
```

This is not to say that oscar.py is inefficient and should not be utilized, but it is not the optimal solution for creating new .tch or .s relationship files. oscar.py solely provides a Python interface for gathering requested data out of the respective .tch files and not for mass-reading all 32 files. It also provides simple function calls that were mentioned earlier in the tutorial for retrieving bits of information at a time in a more convenient way.


## Activity S4: Plumbing of WoC

We can obtain a diff for any commit. It requires comparing trees of it and its parent:

Lets find the diff for 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a: 
```
[username@da0]~% echo 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a | ~/lookup/cmputeDiff 
009d7b6da9c4419fe96ffd1fffb2ee61fa61532a;/sys/dev/pccbb/pccbb_isa.c;9d5818e25865797b96e4783b00b45f800423e527;594dc8cb2ce725658377bf09aa0f127183b89f77
009d7b6da9c4419fe96ffd1fffb2ee61fa61532a;/sys/dev/pccbb/pccbb_pci.c;b3c1363c90de7823ec87004fe084f41d0f224c9b;4155935a98ba3b5d3786fa1b6d3d5aa52c6de90a
```
We can see it modifying two files.

### Exercise

Calculate the change made to /sys/dev/pccbb/pccbb_isa.c by commit 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a.

Hint1: Get the old and new blob for  /sys/dev/pccbb/pccbb_isa.c

Hint2: Use shell redirect output '>' to save the content of each blob

Hint3: Use unix diff to calculate the difference
```
[username@da0]~% diff old new
```

## Iterating over a dataset

Sometimes, iterating over the entire dataset using the already created basemaps is the only way to retrieve the desired information. The basemappings from one datatype to another are key-value pairings of data. As such, the retrieval of the entire dataset can usually be done in one pass over one of the already created basemaps.

For example, if the goal was to determine information pertaining to each author in WoC, simply iterating over one of the many basemaps from author to some other dataset e.g. (a2b, a2c, etc) will serve. Since these datasets are a key-value mapping from author to another dataset, this guarantees that each of the keys will be one of the unique authors in WoC. From there, the desired information about that specific author can be determined. 

Below is a Perl script template that allows for retrieval of all the authors from a2c. 

-----------------------
```perl
#!/usr/bin/perl -I /home/audris/lookup -I /home/audris/lib64/perl5 -I /home/audris/lib/x86_64-linux-gnu/perl -I /usr/local/lib64/perl5 -I /usr/local/share/perl5
use strict;
use warnings;
use Error qw(:try);

use TokyoCabinet;

my $split = 32;
my %a2cF = (); 
for my $sec (0..($split-1)){   
  my $fname = "/da1_data/basemaps/a2cFullS.$sec.tch";   
  tie %{$a2cF{$sec}}, "TokyoCabinet::HDB", "$fname", TokyoCabinet::HDB::OREADER | TokyoCabinet::HDB::ONOLCK,  16777213, -1, -1, TokyoCabinet::TDB::TLARGE, 100000 
  or die "cant open $fname\n"; 
}
while (my ($i, $author) = each %a2cF) {
  my $v1 = join ";", sort keys %{$author};
  my @apl = split(';', $v1);
  for my $a (@apl) {
    print "$a\n";
  }
}

```
---------------
This script simply prints each WoC authors name. This helps illustrate how to go about retrieving the keys in a key-value basemap using Perl, but lacks any practical use on it's own.

Notice in this script the $split is defined to be 32 and the for loop iterates from 0 to 31. The reason for this is because of how the data is stored in the basemaps. Each basemap from one data type to another is split into 32 roughly equal parts based on their hashes. As such, in order to iterate over the entire data set, it is neccesary to look at each of these files separately.

From there, Perl allows for direct tying to each of these files in the format of a hash. Because the basemappings are saved using TokyoCabinet, it requires them to be opened using TokyoCabinet to retrieve the data.

Once the hash is tied to the mapping, iterating over the hash can be done, and retrieval of the information simply becomes accessing the elements.

## Mongo Database

On the da1 server, there is a MongoDB server holding some relevant
data. This data includes some information that was used for data
analysis in the past. Mongo provides an excellent place to store
relatively small data without requiring relational information. 

Two collections the WoC database cand be helpful for sampling
projects and authors A_metadata.V and P.metadata.V where V
represents the version (e.g., T) , A stands for aliased author id
and P for deforked repository name. 

### MongoDB Access

When on the da1 server, you can gain access to the MongoDB server simply by running the command 'mongo', or, when on any other da server, you can gain access by running 'mongo --host "da1.eecs.utk.edu"'.

Once on the server, you can see all the available databases using the "show dbs" command. However, the database that pertains primarily to the WoC is the WoC database. 

Most databases are used for teaching and other tasks, spo please use
WoC database using the 'use "database name"' command, E.G. (use WoC), and, after switching, you can view the available collections in the database by using the 'show collections' command. 
	
Currently, there is an author metadata collection (A_metadata.U)
that contains basic stats: the total number of projects, 
	the total number of blobs created by them (before
anyone else), the total number
of commits made, the total number of files they have modified, the
distribution of language files modified by that author, 
and the first and last time the author committed in Unix Timestamp based on the
data contained on version S of WoC. Author names have ben aliased
and the number of aliases and the list are also included in the record.
Furthermore, up to 100 most commonly used API (packages) in author modified files are 
also included. 
	
Alongside this, there is a similar collection for projects on WoC
(P_metadata.U) that contains the total number of authors on the
project, the total number of commits, the total number of files, the
distribution of languages used, the first and last time there was a
commit to the project in Unix Timestamp based on the version U of
WoC. Since the project is deforked, the community size (the number
of other projects that share commits with the deforeked project) is
also provided. WoC relation P2p can be used to list other projects
linked to it.  We also provide additional info based on linking to
attributes that exist only in GitHub. That data is not as recent,
however and more work is needed to make it complete. These
attributes include the number of stars GitHub has given that project
(if any), if the project is a GitHub fork, and where it was forked
from (if anywhere).

Finally the collection of APIs or packages contains summary of the first and last time the package was used in a modified file as well as the number of commits, authors and repositories associated with the use of that package.
	
To see data in one of the collections, you can run the 'db."collection name".findOne()' command. This will show the first element in the collection and should help clarify what is in the collection.


When the above findOne() command is run on the A_metadata.U
collection, the output is as follows: (in this case we look only for items with more than 200 commits:

-----------
```
mongosh --host=da1
mongosh>use WoC;
WoC> db.A_metadata.U.findOne({NumCommits:{$gt:200}})
{ 
  _id: ObjectId("62229b132bc6e5f0dbd0307f"),
  FileInfo: {
    Ruby: 2,
    TypeScript: 1,
    Python: 2,
    Rust: 92,
    PHP: 6895,
    other: 2406,
    Sql: 1,
    JavaScript: 1384,
    'C/C++': 6,
    Java: 1
  },
  NumActiveMon: 19,
  EarliestCommitDate: 1512136069,
  ApiInfo: {
    'Rust:the': 1,
    'PY:sys': 1,
    'PY:datetime': 1,
    'Rust:on': 1,
    'PY:os': 1
  },
  LatestCommitDate: 1550574037,
  MonNprj: {
    '2019-02': 1,
    '2017-11': 5,
    '2018-04': 2,
    '2018-12': 2,
    '2018-03': 2,
    '2019-05': 1,
    '2019-04': 1,
    '2019-03': 1,
    '2018-05': 2,
    '2018-02': 3,
    '2018-08': 1,
    '2018-01': 1,
    '2019-06': 1,
    '2017-12': 4,
    '2018-10': 1,
    '2018-06': 1,
    '2018-11': 1,
    '2018-07': 1,
    '2019-01': 4
  },
  NumOriginatingBlobs: 2187,
  AuthorID: 'Jennifer Calipel <jennifer.calipel@gmail.com>',
  MonNcmt: {
    '2019-02': 9,
    '2017-11': 21,
    '2018-04': 13,
    '2018-12': 2,
    '2018-03': 29,
    '2019-05': 1,
    '2019-04': 2,
    '2019-03': 5,
    '2018-05': 9,
    '2018-02': 32,
    '2018-08': 42,
    '2018-01': 6,
    '2019-06': 2,
    '2017-12': 23,
    '2018-10': 1,
    '2018-06': 5,
    '2018-11': 1,
    '2018-07': 12,
    '2019-01': 17
  },
  NumCommits: 232,
  NumProjects: 18,
  NumFiles: 10790
}
```
Similarly for projects:
```
 WoC> db.P_metadata.U.findOne({NumCommits:{$gt:200}})
{ 
  _id: ObjectId("62228cb7e65a0aefc2ca086f"),
  FileInfo: { other: 442, JavaScript: 17 },
  NumAuthors: 11,
  MonNauth: {
    '2020-04': 9,
    '2021-07': 1,
    '2020-11': 1,
    '2020-07': 1,
    '2021-05': 1,
    '2021-08': 1,
    '2020-08': 1,
    '2020-03': 5,
    '2020-06': 5,
    '2020-12': 1,
    '2020-05': 5
  },
  EarliestCommitDate: 1584055325,
  NumStars: 17,
  NumBlobs: 709,
  LatestCommitDate: 1628739252,
  ProjectID: 'foss-responders_fossresponders.com',
  MonNcmt: {
    '2020-04': 60,
    '2021-07': 2,
    '2020-11': 1,
    '2020-07': 1,
    '2021-05': 2,
    '2021-08': 2,
    '2020-08': 4,
    '2020-03': 48,
    '2020-06': 16,
    '2020-12': 3,
    '2020-05': 91
  },
  NumCore: 3,
  NumCommits: 230,
  CommunitySize: 1,
  NumFiles: 459,
  Core: {
    'SaptakS <saptak013@gmail.com>': '23',
    'Awele <awele.osuka@gmail.com>': '11',
    'Richard Littauer <richard@maintainer.io>': '157'
  },
  NumForks: 11
}
```
And for APIs
```
mongosh> WoC> db.API_metadata.U.findOne({$and: [ { NumCommits:{$gt:200} }, { NumProjects: {$gt:200} }, {NumAuthors:{$gt:200}} ]})
{
  _id: ObjectId("62257192758fdfbec79e9125"),
  NumAuthors: 456,
  Lang: 'C',
  NumProjects: 236,
  NumCommits: 4366,
  API: 'C:BasicUsageEnvironment.hh'
}
```
---------------

This metadata can then be parsed for the desired information.

Python, like most other programming languages, has an interface with
Mongo that makes for data storage/retrieval much simpler. When
retrieving or inputting large amounts of data onto the servers, it
is almost always faster and easier to do so through one of the
interfaces provided. 


### PyMongo

PyMongo is an import for Python that simplifies access to the database and elements inside of it. When accessing the server you must first provide which Mongo Client you wish to connect to. For our server, the host will be "mongodb://da1.eecs.utk.edu/". 
This will allow access to the data already saved and will allow for creation of new data if desired. 

From there, accessing databases inside of the client becomes as simple as treating the desired database as an element inside the client. The same is true for accessing collections inside of a database. 
The below code illustrates this process.

--------
```python3
import pymongo
client = pymongo.MongoClient("mongodb://da1.eecs.utk.edu/")

db = client["WoC"]                                                    
coll = db["A_metadata.U"]
```
-------

#### Data Retrieval using PyMongo

When attempting to retrieve data, iterating over the entire collection for specific info is often neccesary. This is done most often through a mongo specific data structure called cursors. However, cursors have a limited life span. After roughly 10 minutes of continuous connection to the server, the cursor is forcibly disconnected. This is to limit the possible number of idle cursors connected to the server at any time. 

Taking this into consideration, if the process may take longer than that, it is neccesary to define the cursor as undying. If this is neccesary, manual disconnection of the cursor after it's served it's purpose is required as well.
The below code illustrates creation and iteration over the collection with a cursor.

--------
```python
client = pymongo.MongoClient("mongodb://da1.eecs.utk.edu/")

db = client["WoC"]                                                    
coll = db["A_metadata.U"]

dataset = col.find({}, cursor_no_timeout=True)
for data in dataset:
   ...

dataset.close()
```
-------

Once data retrieval has begun, accessing the specific information desired is simple. 
For example, provided above is the information saved in one element
of auth_metadata. If access to the AuthorID of each cursor is
desired, the "AuthorID" can be treated as the key in a key
value-mapping. However, it is often neccesary to consider how the
data is stored. 

Most often, when storing data in Mongo, it will be stored in Mongo
specific format called BSON. BSON objects are saved in
unicode. Working with unicode can be an issue if printing needs to
be done. As such, decoding from unicode must to be done. Below
illustrates a small program that prints each AuthorID from the
auth_metadata collection. 

----------
```python
import pymongo
import bson

client = pymongo.MongoClient("mongodb://da1.eecs.utk.edu/")
db = client ['WoC']
coll = db['A_metadata.U']

dataset = coll.find({}, no_cursor_timeout=True)
for data in dataset:
    a = data["AuthorID"].encode('utf-8').strip()
    print(a)

dataset.close()

```
----------

When retrieving data, it is often neccesary to narrow the
results. This is possible directly through Mongo when querying for
information. For instance, if all the data is not needed in the
auth_metadata, just the NumCommits and the AuthorID, the query can
be restricted adding parameters to the find call. An example query
is provided below. 

----------
```python
dataset = coll.find({}, {"AuthorID": 1, "NumCommits": 1, "_id": 0}, no_cursor_timeout=True)

for data in dataset:
    print(data)
    
dataset.close()
```
---------

This specific call allows for direct printing of the data, however, as noted above, the names are saved in BSON and as such will be printed in unicode. The first 10 results are shown below.

-------------
```
{u'NumCommits': 1, u'AuthorID': u'  <mvivekananda@virtusa.com>'}
{u'NumCommits': 0, u'AuthorID': u' <1151643598@163.com>'}
...
```
--------------

Sometimes, restricting the data even further is neccesary. Notice above that many of the users have 0 commits. Exclusion of these entries may be desired. The below example illustrates a way to restrict the results to only users with greater than 0 commits.

----------
```python
dataset = coll.find({"NumCommits : { "$gt" : 0 } }, 
				     {"AuthorID": 1, "NumCommits": 1, "_id": 0}, 
				     no_cursor_timeout=True)

for data in dataset:
    print(data)
```
---------

## Accesing by time slices

To access collection indexed by time we use clickhouse databse:
https://clickhouse.yandex/docs/en/getting_started/tutorial/


It has interfaces to various languages but the key is super fast indexing and the ability to distribute data over a cluster of da servers.

Since only commits have time associated with them, we start from storing all commits in the database. We store only a subset of commits on each server, first we create a table local to each server (commits) and a table that represents all servers (commits_a):
```
for h in da0 da1 da2 da3 da4; 
do echo "CREATE TABLE api_$v (api String, from Int32, to Int32, ncmt Int32, nauth Int32, nproj Int32) ENGINE = MergeTree() ORDER BY from" |clickhouse-client --host=$h
   echo "CREATE TABLE api_all AS api_$v ENGINE = Distributed(da, default, api_$v, rand())" | clickhouse-client --host=$h

  echo "CREATE TABLE commit_$v (sha1 FixedString(20), time Int32, tc Int32, tree FixedString(20), parent String, taz String, tcz String, author String, commiter String, project String, comment String) ENGINE = MergeTree() ORDER BY time" |clickhouse-client --host=$h
  echo "CREATE TABLE commit_all AS commit_$v ENGINE = Distributed(da, default, commit_$v, rand())" | clickhouse-client --host=$h
done
```

Then we import data into each of these five tables:
```bash
v=u
for j in {0..4}
do da=da$j
  for i in $(eval echo "{$j..31..5}")
  do echo "start inserting $da file $i"
    time /da?_data/basemaps/gz/Pkg2stat$i.gz | ~/lookup/chImportPkg.perl | clickhouse-client --max_partitions_per_insert_block=1000 --host=$da --query 'INSERT INTO api_u FORMAT RowBinary'
  done
  for i in $(eval echo "{$j..127..5}")
  do echo "start inserting $da file $i"
    time zcat /da?_data/basemaps/gz/c2chFullU$i.s | ~/lookup/chImportCmt.perl | clickhouse-client --max_partitions_per_insert_block=1000 --host=$da --query 'INSERT INTO commit_u FORMAT RowBinary'
  done
done 
```

Once the data is in there we can query commits
```bash
clickhouse-client --host=da3 --query 'select count (*) from commits_all'
2061780191
```
or APIs:
```bash
echo "select api,ncmt, nauth, nproj from api_all where match(api, 'stdio') and nauth > 100 limit 3 FORMAT CSV" |clickhouse-client --host=da3 --format_csv_delimiter=";"
"C:stdio_ext.h";153898;4797;1671
"C:ustdio.h";9995;1107;230
"C:vcl_cstdio.h";5868;163;9
```

	
It works fast if we specify specific time or an interval:
```bash
clickhouse-client --host=da3 --query 'select author,comment from commits_all where time=1568656268'
Matt Davis <mw.davis@hotmail.co.uk>     Made some SEO improvements and also added comments outlining what is contained in each section.\n
Jessie 1307 <295101171@qq.com>  First Commit\n
�
 �� <910063@gmail.com>  0917\n
nodemcu-custom-build <vladurash@yahoo.com>      Prepare my build.config for custom build
zzzz1313 <zaki56@rambler.ru>    Initial commit
Erik Faye-Lund <erik.faye-lund@collabora.com>   .mailmap: add an alias for Sergii Romantsov\n
Paulus Pärssinen <paulus.parssinen01@edupori.fi>       Initial commit
AnnaLub <yaskrava@gmail.com>    get all tickets command impl\n
```

We may want to match on commit comment
```bash
echo "select lower(hex(sha1)),author, project, comment from commit_all where match(comment, 'CVE-2021') limit 3 FORMAT CSV" |clickhouse-client --host=da3 --format_csv_delimiter=";"
"Florian Westphal <fw@strlen.de>";"Jackeagle_kernel_msm-3.18";"netfilter: x_tables: make xt_replace_table wait until old rules are not used anymore\nxt_replace_table relies on table replacement counter retrieval (which__NEWLINE__uses xt_recseq to synchronize pcpu counters).\nThis is fine, however with large rule 
set get_counters() can take__NEWLINE__a very long time -- it needs to synchronize all counters because__NEWLINE__it has to assume concurrent modifications can occur.\nMake xt_replace_table synchronize by itself by waiting until all cpus__NEWLINE__had an even seqcount.\nThis allows a followup patch to copy the cou
nters of the old ruleset__NEWLINE__without any synchonization after xt_replace_table has completed.\nCc: Dan Williams <dcbw@redhat.com>__NEWLINE__Reviewed-by: Eric Dumazet <edumazet@google.com>__NEWLINE__Signed-off-by: Florian Westphal <fw@strlen.de>__NEWLINE__Signed-off-by: Pablo Neira Ayuso <pablo@netfilter.org
>\n(cherry picked from commit 80055dab5de0c8677bc148c4717ddfc753a9148e)__NEWLINE__Orabug: 32709122__NEWLINE__CVE: CVE-2021-29650__NEWLINE__Signed-off-by: Sherry Yang <sherry.yang@oracle.com>__NEWLINE__Reviewed-by: John Donnelly <john.p.donnelly@oracle.com>__NEWLINE__Signed-off-by: Somasundaram Krishnasamy <somasu
ndaram.krishnasamy@oracle.com>"
"Joe Yu <joe.yu@unisoc.com>";"daedroza_aosp_development_sony8960_n";"Fix storaged memory leak\nCVE-2021-0330 : (AOSP) EoP Vulnerability in Framework / storaged__NEWLINE__A-170732441__NEWLINE__Mot-CRs-fixed: (CR)\nstoraged try to load user's proto even if it has been loaded before\nhttps://partnerissuetracker.corp
.google.com/u/2/issues/118719575\nChange-Id: Ia7575cdc60e82b028c6db9a29ae80e31e02268b1__NEWLINE__(cherry picked from commit 857a63eb6604baa1ed6b0e31839ccce8da18c716)__NEWLINE__Signed-off-by: Mark Salyzyn <salyzyn@google.com>__NEWLINE__Bug: 170732441__NEWLINE__Test: compile__NEWLINE__(cherry picked from commit 8ec
2afb91400818b0a8843b8917c05aba75b00db)__NEWLINE__Reviewed-on: https://gerrit.mot.com/1843719__NEWLINE__SLTApproved: Slta Waiver__NEWLINE__SME-Granted: SME Approvals Granted__NEWLINE__Tested-by: Jira Key__NEWLINE__Reviewed-by: Konstantin Makariev <kmakariev@motorola.com>__NEWLINE__Submit-Approved: Jira Key"
"Joe Yu <joe.yu@unisoc.com>";"daedroza_aosp_development_sony8960_n";"Fix storaged memory leak\nCVE-2021-0330 : (AOSP) EoP Vulnerability in Framework / storaged__NEWLINE__A-170732441__NEWLINE__Mot-CRs-fixed: (CR)\nstoraged try to load user's proto even if it has been loaded before\nhttps://partnerissuetracker.corp
.google.com/u/2/issues/118719575\nChange-Id: Ia7575cdc60e82b028c6db9a29ae80e31e02268b1__NEWLINE__(cherry picked from commit 857a63eb6604baa1ed6b0e31839ccce8da18c716)__NEWLINE__Signed-off-by: Mark Salyzyn <salyzyn@google.com>__NEWLINE__Bug: 170732441__NEWLINE__Test: compile__NEWLINE__(cherry picked from commit 8ec
2afb91400818b0a8843b8917c05aba75b00db)__NEWLINE__Reviewed-on: https://gerrit.mot.com/1844255__NEWLINE__SLTApproved: Slta Waiver__NEWLINE__SME-Granted: SME Approvals Granted__NEWLINE__Tested-by: Jira Key__NEWLINE__Reviewed-by: Konstantin Makariev <kmakariev@motorola.com>__NEWLINE__Submit-Approved: Jira Key"
```
commit sha1's are binary, so to print them we need to process, e.g., 
```bash
clickhouse-client --host=da1 --query 'select sha1, author,comment from commits_all where time=1568656268 limit 1 format RowBinary' | perl -ane '$sha1=substr($_, 0, 20); $o=unpack "H*", $sha1; $rest=substr($_,21,length($_)-21); print "$o;$rest\n";' 
fbb7add2a58b733a797d97a1e63cb8661702d0a3;zzzz1313 <zaki56@rambler.ru>Initial commit
```
Alternatively, we can hex them in the select statement:
```bash
clickhouse-client --host=da1 --query "select lower(hex(sha1)),author,comment from commits_all where match(comment, '^(CVE-(1999|2\d{3})-(0\d{2}[0-9]|[1-9]\d{3,}))$') limit 2 format CSV" 
"024fbd8de50c1269d178c3ee6b8664f5eee7f57b","nickmx1896 <nickmx1896@Hotmail.com>","CVE-2016-2355"
"209446bab86e996d58c233abee0376cb26dcd4c4","jonathanliem94 <jonathanliem94@gmail.com>","CVE-2017-4963"
```

We can create additional tables, so that the time filtering could be fast, for example for projects:

```
clickhouse-client --max_partitions_per_insert_block=1000 --host=da3 --query "CREATE TABLE c2p_$i (date Date, sha1 FixedString(20), np UInt32, p String) ENGINE = MergeTree(date, sha1, 8192)"
for j in {3..31..4}; do time ./importc2p.perl $j | clickhouse-client --max_partitions_per_insert_block=1000 --host=da3 --query "INSERT INTO c2p_$j (date, sha1, np, p) FORMAT RowBinary"; done 
```

	
	
## Python Clickhouse API

CH API is disabled in the current version of oscar:   make a
separate module - see draft in lookup/oscarch.py)


There are classes in oscar.py that allow for querying the clickhouse database:  
1. `Time_commit_info(tb_name='commits_all', db_host='localhost')` - commits
	* `.commit_counts(start, end=None)` - get the count of the commits given a time interval
	* `.commits_iter(start, end=None)` - get the commits as 'Commit' objects in a generator
	* `.commits_shas(start, end=None)` - get the sha1 of the commits in a list
	* `.commits_shas_iter(start, end=None)` - get the sha1 of the commits in a generator
2. `Time_projects_info(tb_name='b2cPtaPkgR_all', db_host='localhsot')` - \*projects
	* `.get_values_iter(cols, start, end)` - query columns given a time interval (generator)
	* `.project_timeline(cols, repo)` - query columns of a given project name, sorted by time (generator)
	* `.author_timeline(cols, author)` - query columns of a given author, sorted by time (generator)
	
\*note that the *b2cPtaPkgR_all* table currently does not contain projects that uses the following programming languages: php, Lisp, Sql, Fml, Swift, Lua, Cob, Erlang, Clojure, Markdown, CSS

The structures of the databases are listed below:  
**commits_all:**  
        |__name___|______type_______|
        | sha1    | FixedString(20) |
        | time    | Int32           |
        | timeCmt | Int32           |
        | tree    | FixedString(20) |
        | parent  | String          |
        | TZAuth  | String          |
        | TZCmt   | String          |
        | author  | String          |
        | commiter| String          |
        | project | String          |
        | comment | String          |

**b2cPtaPkgR_all:**  
| name     | type            |
|----------|-----------------|
| blob     | FixedString(20) |
| commit   | FixedString(20) |
| project  | String          |
| time     | UInt32          |
| author   | String          |
| language | String          |
| deps     | String          |

We can use the `commit_counts` to query the count of the commits given a time interval:
```python
>>> from oscarch import Time_commit_info
>>>
>>> t = Time_commit_info()
>>> t.commit_counts(1568656268)
9
```
The `commits` method can be used to iterate through commit objects given a time interval:
```python
>>> t = Time_commit_info()
>>> commits = t.commits(1568656268)
>>> c = commits.next()
>>> type(c)
<class 'oscar.Commit'>
>>> c.parent_shas
('9c4cc4f6f8040ed98388c7dedeb683469f7210f5',)
```
The `commits_shas` method can be used to iterate through commit hashes given a time interval:
```python
>>> t = Time_commit_info()
>>> for sha1 in t.commits_shas(1568656268):
...     print(sha1)
0a8b6216a42e84d7d1e56661f63e5205d4680854
874d92e732d79d0d8bafb1d1bcc76a3b6d81302f
ccf1a5847661de2df791a5a962e3499a478f48ab
39927c70a99f0949c1de3d65a2693c8768bc4e0f
fbb7add2a58b733a797d97a1e63cb8661702d0a3
...
```

The `b2cPtaPkgR_all` table stores the information associated to each **commit**.  
For `b2cPtaPkgR_all` table, use `get_values_iter` of the `Time_projects_info` class to queries for columns in a given time interval:
```python
>>> from oscar import Time_project_info as Proj
>>> p = Proj()
>>> rows = p.get_values_iter(['time','project'], 1568571909, 1568571910)
>>> for row in rows:
...     print(row)
...
(1568571909, 'mrtrevanderson_CECS_424')
(1568571909, 'gitlab.com_surajpatel_tic_toc_toe')
(1568571909, 'gitlab.com_surajpatel_tic_toc_toe')
...
```
`project_timeline` can be used to query for a specific repository. The result shows the time of the commit and the name of the commit repo sorted by time:
```python
>>> rows = p.project_timeline(['time','project'], 'mrtrevanderson_CECS_424')
>>> for row in rows:
...     print(row)
...
(1568571909, 'mrtrevanderson_CECS_424')
(1568571909, 'mrtrevanderson_CECS_424')
(1568571909, 'mrtrevanderson_CECS_424')
...
```

It might be useful to examine the dependencies (i.e. includes in C or imports in Python) for each commit.  
The snippet below shows the time, repo name, language, and dependencies for each commit. Note that the commits are sorted by time and the dependencies are separated by semicolon.
```python
>>> rows = p.get_values_iter(['time', 'project', 'language', 'deps'], 1568571915, 1568571916)
>>> for row in rows:
...     print(row)
...
(1568571916, 'Nakwendaa_neural-network', 'PY', 'numpy\n')
(1568571916, 'Nakwendaa_neural-network', 'PY', 'os;pickle;numpy;time;matplotlib.pyplot;gzip;Mlp.Mlp\n')
```  
Similarily, `author_timeline` queries for a specific author:
```python
>>> rows = p.author_timeline(['time', 'project'], 'Andrew Gacek <andrew.gacek@gmail.com>')
>>> for row in rows:
...     print(row)
...
(49, 'smaccm_camera_demo')
(677, 'smaccm_vm_hack')
(1180017188, 'teyjus_teyjus')
...
```

# Considerations on performance

1. getValues and showCnt are not supposed to be called every second,
   the keys are passed through from standard input and one line is
   generated for each key (get values also passes attributes,
```
echo "k;a" | getValues k2v
```
produces one line
```
k;a;v0;v1;..vn
```
For blobs, it is possible to export the entire content as a single
bas64 encoded line

2. Operations that require iteration over all keys or values (e.g., match a pattern) over all
is faster via flat files
```
for i in {0..127}; do zcat /da?_data/basemaps/gz/k2vFullUi.s; done | grep PATTERN
```
If the iteration is over commit content, use 
```
cd /da5_data/All.blobs/
for i in {0..127}; do perl ~/lookup/lstCmt.perl 9 $i; done
```
If iteration is over blob content 

```
cd /da5_data/All.blobs/
for i in {0..127}; do perl ~/lookup/lstBlob.perl $i; done
```

3. For a very large number of exact keys (over 500K) it is faster to use unix join 
(simply split (via splitSec.perl for hashes and splitSecCh.perl for strings), sort each piece
and use unix join:
```
for i in {0..127}; do zcat /da?_data/basemaps/gz/k2vFullUi.s |join -t\; <(zcat piece$i) -; done
```  
