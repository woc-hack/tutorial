# Tutorial: basics for the Hackathon


[Recording of the tutorial conducted on 2019-10-15](https://drive.google.com/file/d/14tAx2GQamR4GIxOc3EzUXl7eyPKRx2oU/view?usp=sharing) 


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
That is your public ssh key you include in the woc hackathon registration [form](http://bit.ly/WoC-Signup). The form will ask you for the contents of `id_rsa.pub` and the
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
   - Ability to select projects/developers for natural experiments/other empirical studies 
* Provide FLOSS-wide relationships
   - Technical dependencies (to run applications)
   - Tool dependencies (to build/test applications)
   - Code migration
   - Knowledge (and people) migration
* Data Cleaned/Augmented/Contextualized
   - Correction: Authors/Forks/Outliers
   - Augmentation: Dependencies/Linking to other data sources
   - Context: project types/expertise
* Big Data Analytics: Map entities to all related entities efficiently
* Timely: Targeting < 1 week old analyzable snapshot of the entire FLOSS
* Community run
   - Hackathon will help determine community needs
   - [Hackathon Schedule](https://github.com/woc-hack/schedule)
* How to participate?
   - Hackathon registration [form](http://bit.ly/WoC-Signup)
   - If you can not attend the hackathon but just want to try out WoC, please fill the hackathon form but indicate in the topic section is that you do not plan to attend the hackathon.

### What WoC Prototype contains

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
git clone https://bitbucket.org/swsc/lookup
git clone https://github.com/ssc-oscar/oscar.py
```

Log in to da4 from da0:
```
[username@da0]~% ssh da4
[username@da4]~% ls
...
[username@da4]~% exit
[username@da0]~%
```


## Activity 2: Shell APIs - Basic Operations


Shell APIs might be useful to accsess content of the commit, trees, blobs, 
to calculate the diff produced by a commit, etc. 

For more examples [see full API](https://bitbucket.org/swsc/lookup/src/master/README.md). 

Lets look at the commit 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a:

```
[username@da0]~% echo 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a | ~/lookup/showCnt commit
tree 464ac950171f673d1e45e2134ac9a52eca422132
parent dddff9a89ddd7098a1625cafd3c9d1aa87474cc7
author Warner Losh <imp@FreeBSD.org> 1092638038 +0000
committer Warner Losh <imp@FreeBSD.org> 1092638038 +0000

Don't need to declare cbb module.  don't know why I never saw
duplicate messages..
```

This commit has a tree and a parent commit and is created by 'Warner Losh <imp@FreeBSD.org>'. 
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

### Exercise 2

Determine the author of the parent commit for commit 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a

Hint 1: parent commit is listed in the content of commit 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a above
```
echo dddff9a89ddd7098a1625cafd3c9d1aa87474cc7 | ~/lookup/showCnt commit
```

## Activity 3 - Investigate the maps

We see the content of the copyright file above. Such files are often copied verbatim. Lets determine the first author who have created it (irrespective of a repository).
WoC has created this relationship and stored in b2a (Blob to Author) map:
```
[username@da0]~% echo a8fe822f075fa3d159a203adfa40c3f59d6dd999 |  ~/lookup/getValues b2a
a8fe822f075fa3d159a203adfa40c3f59d6dd999;1072910122;Warner Losh <imp@ccf9f872-aa2e-dd11-9fc8-001c23d0bc1f>;00a8f599c25ded714d2a4da9e1bb30e2a335181c
```
It turns out that it was created by commit 00a8f599c25ded714d2a4da9e1bb30e2a335181c done by what appears to be the same author on unix second 1072910122.

What is b2a? The letters signify what keys (b - Blob) and values (a - author) means. These are key objects: 
* a = Author
* b = Blob
* c = Commit
* f = File
* p = Project


We may also ask if this blob has been widely copied as would be expected for copyright files:
```
[username@da0]~% echo a8fe822f075fa3d159a203adfa40c3f59d6dd999 |  ~/lookup/getValues b2c
a8fe822f075fa3d159a203adfa40c3f59d6dd999;00729b406d8d3cfeeeda61c4586dcfd9f9399f4a;00a8f599c25ded714d2a4da9e1bb30e2a335181c;...
```
b2c (blob to commit) shows the numerous commits that introduced that blob in all repositories. We can further use commit to project map (c2p)
to identify all associated projects:
```
[username@da0]~% echo a8fe822f075fa3d159a203adfa40c3f59d6dd999 |  \
~/lookup/getValues b2c | \
perl -ane 's/^[^;];//;s/;/\n/g;print' | \
~/lookup/getValues c2p | \
perl -ane 's/^[^;];//;s/;/\n/g;print' 
ajburton_freebsd
bu7cher_freebsd
denghuancong_freebsd
...
```
In fact there are 1072 distinct repositories where this blob appears.

Finally, we have the author 'Warner Losh <imp@FreeBSD.org>' for the commit we have investigated. 
Can we find what other commits Warner has made?:
```
[username@da0]~% echo 'Warner Losh <imp@FreeBSD.org>' | ~/lookup/getValues a2c
Warner Losh <imp@FreeBSD.org>;0000ce4417bd8d9a2d66a7a61393558d503f2805;000109ae96e7132d90440c8fa12cb7df95a806c6;00014b72bf10ad43ca437daf388d33c4fea73df9;000171c80d0d0ab6ff22b58b922e559e51485936;000282fc6c091e1c0abdadf1f58c088fc3ed9bc9;0002d1cab0d367c074a601e28183c60657254820;000373a8c48347c3e0e30486ccf4d9b043438826;...

```

In addition to the random lookup, the maps are also stored in flat sorted files and this format is preffered (faster) when investigating over one million items. 
For example, find commits by any author named Warner: 
```
[username@da0]~% zcat /da0_data/basemaps/gz/a2cFullP0.s | grep 'Warner'
```
As described below, the maps are split into 32 parts to enable parallel search.
FullP means that we are looking ata  complete extract at version P. 


### Exercise 3

a) Find all files modified by 'Warner Losh <imp@FreeBSD.org>'

Hint 1: What is the map name?

Author to File or a2f
```
echo 'Warner Losh <imp@FreeBSD.org>' | ~/lookup/getValues a2f 
```

Find all commits  developers who have your last and your first name:

Hint 1: use wc (word count), e.g., 

```
[username@da0]~% zcat /da0_data/basemaps/gz/a2cFullP*.s | grep -i 'audris' | grep -i 'mockus' | wc -l
```


## Activity 4: Using Python APIs from oscar.py

Note: "/<function_name>" after a function name denotes the version of that function that returns a Generator object  

These are corresponding functions in oscar.py that open the .tch files listed below for a given entity:

1. `Author('...')`  - initialized with a combination of name and email
	* `.commit_shas/commits`
	* `.project_names`
	* `.torvald` - returns the torvald path of an Author, i.e, who did this Author work
				 with that also worked with Linus Torvald
2. `Blob('...')` -  initialized with SHA of blob
	* `.commit_shas/commits` - commits removing this blob are not included
3. `Commit('...')` - initialized with SHA of commit
	* `.blob_shas/blobs`
	* `.child_shas/children`
	* `.changed_file_names/files_changed`
	* `.parent_shas/parents`
	* `.project_names/projects`
4. `Commit_info('...')` - initialized like Commit()
	* `.head`
	* `.time_author`
5. `File('...')` - initialized with a path, starting from a commit root tree
	* `.commit_shas/commits`
6. `Project('...')` - initialized with project name/URI
	* `.author_names`
	* `.commit_shas/commits`

The non-Generator version of these functions will return a tuple of items which can then be iterated:
```
for commit in Author(author_name).commit_shas:
	print(commit)
```

### Exercise 4a:  Get a list of commits made by a specific author:  

Install the latest oscar.py
```
[username@da0]~% cd ~/oscar.py
[username@da0]~% easy_install --user --upgrade oscar
```

As we learned before, we can do that in shell
```
[username@da0]~% zcat /da0_data/basemaps/gz/a2cFullP0.s | grep "Albert Krawczyk" <pro-logic@optusnet.com.au>
"Albert Krawczyk" <pro-logic@optusnet.com.au>;17abdbdc90195016442a6a8dd8e38dea825292ae
"Albert Krawczyk" <pro-logic@optusnet.com.au>;9cdc918bfba1010de15d0c968af8ee37c9c300ff
"Albert Krawczyk" <pro-logic@optusnet.com.au>;d9fc680a69198300d34bc7e31bbafe36e7185c76
```

Now the same thing can be done using oscar.py:  
```
[username@da0]~% cd oscar.py
[username@da0:oscar.py]~% python
>>> from oscar import Author
>>> Author('"Albert Krawczyk" <pro-logic@optusnet.com.au>').commit_shas
('17abdbdc90195016442a6a8dd8e38dea825292ae', '9cdc918bfba1010de15d0c968af8ee37c9c300ff', 'd9fc680a69198300d34bc7e31bbafe36e7185c76')
```

### Exercise 4b: Get the URL of a projects repository using the oscar.py `Project(...).toURL()` function:  
```
[username@da0:oscar.py]~%  python
>>> from oscar import Project
>>> Project('notcake_gcad').toURL()
'https://github.com/notcake/gcad'
```

### Exercise 4c

Get list of files modified by commit 17abdbdc90195016442a6a8dd8e38dea825292ae

Hint 1: What class to use?
Commit
```
[username@da0:oscar.py]~%  python
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
and different speed of disks. For example, blobs are stored only on
da4 and da5. 
The description below goes over what is stored on each server. 

### da0/da5 Servers
#### <relationship>.{0-31}.tch files in `/data/basemaps/` on da0 and /fast on da5:  
(.s) signifies that there are either .s or .gz versions of these files in gz/ subfolder, which can be opened with Python gzip module or Unix zcat.  
da0 is the only server with these .s/.gz files  
Keys for identifying letters:   

* a = Author
* b = Blob
* c = Commit
* cc = Child Commit
* f = File
* h = Head Commit
* p = Project
* pc = Parent Commit
* ta = Time Author
* trp = Torvalds Path

List of relationships:
```
* a2c (.s)	* a2f		* a2ft		* a2p (.s)	* a2trp0 (.s)
* b2c (.s)	* b2f (.s)
* c2b (.s)	* c2cc		* c2f (.s)		
* c2h		* c2pc		* c2p (.s)	* c2ta (.s)
* f2b (.s)	* f2c (.s)		
* p2a (.s)	* p2c (.s)
```

### Exercise 5

Find all blobs associated with Julia language files (extension .jl) 

Hint 1: What is the name of the map?

```
[username@da0] zcat /da0_data/basemaps/gz/f2bFullP*.s | grep '\.jl;'
```

## Activity 6: Investigating Technical dependencies

The technical dependenciew have been extracted by parsing the content of all blobs related to 
several different languages: and are located in `/da0_data/play/${LANG}thruMaps/`.

These thruMaps directories contain mappings of repositories with modules that were utilized at a given UNIX timestamp under a specific commit. The mappings are in c2bPtaPkgP{$LANG}.{0-31}.gz files.   

The format of each file is: 
```
commit;repo_name;timestamp;author;blob;module1;module2;...`  
```


Each thruMaps directory has a different language ($LANG) that contains modules relevant to that language.

Lets get a list of commits and repositories that imported Tensorflow for .py files:  
```
[username@da0]~% zcat /da0_data/play/PYthruMaps/c2bPtaPkgPPY.0.gz | grep tensorflow`

0000331084e1a567dbbaae4cc12935b610cd341a;abdella-mohamed_BreastCancer;1553266304;abdella <abdella.mohamed-idris-mohamed@de.sii.group>;0dd695391117e784d968c111f010cff802c0e6d1;sns;keras.models;np;random;tensorflow;os;pd;sklearn.metrics;plt;keras.layers;yaml
00034db68f89d3d2061b763deb7f9e5f81fef27;lucaskjaero_chinese-character-recognizer;1497547797;Lucas Kjaero <lucas@lucaskjaero.com>;0629a6caa45ded5f4a2774ff7a72738460b399d4;tensorflow;preprocessing;sklearn
000045f6a3601be885b0b028011440dd5a5b89f2;yjernite_DeepCRF;1451682395;yacine <yacine.jernite@nyu.edu>;4aac89ae85b261dba185d5ee35d12f6939fc2e44;nn_defs;utils;tensorflow
000069240776f2b94acb9420e042f5043ec869d0;tickleliu_tf_learn;1530460653;tickleliu <tickleliu@163.com>;493f0fc310765d62b03390ddd4a7a8be96c7d48c;np;tf;tensorflow
.....
```

### Exercise 6

Find all repositories using Julia language that import package 'StaticArrays'


Hint 1: What file to look for?
```
[username@da0]~% zcat /da0_data/play/jlthruMaps/c2bptaPkgOjl.*.gz | grep StaticArrays
```

Hint 2: What field contains the repository name?
```
[username@da0]~% zcat /da0_data/play/jlthruMaps/c2bptaPkgOjl.*.gz | grep StaticArrays | cut -d\; -f2 | sort -u
```

## Activity 7: Suggested by the audiance

Find all projects that have commits mentioning "sql injection"

List of commits is on da4:/data/All.blobs/
Lets login to da4, create a data folder to store temporary data on the same server
"/data/play/username", and uce pcommit to project map to get the list of projects.

```
[username@da0]~% ssh da4
[username@da4]~% mkdir /data/play/audris
[username@da4]~% cd /data/play/audris
[username@da4:/data/play/audris]~% cut -d\; -f4 commit_*.idx | ~/lookup/showCmt.perl 2 | grep -i 'sql injection' > 
[username@da4:/data/play/audris]~% cut -d\; -f1 sql_inject | ~/lookup/getValues.perl /da0_data/basemaps/c2pFullP > sql_inject.c2p
```

## Activity 8: Summary of the activities undertaken

* Shell API (faster) and Python API (also Perl API not illustrated) for random access

* Sorted compressed tables for sweeps (grep)

* Key-Value maps to link authors, commits, files, projects, and blobs

* Overview of naming conventions server/data/databases

* Soon to debut:  mongodb tables with the summary information about authors and projects to enable selection of subsets for later analysis: (e.g, I want authors with at least 100 commits who worked no less than three years and participated in at least five java projects.)

### Summary exercise

* What type of usability improvements are needed?

* What types of tasks would you like to work during the hackathon?

* What would make you a long-time user of WoC?


## Self paced part of the tutorial

The remainig activities are provided to illustrate various realistic 
tasks. 

## Activity S0: Finding 1st-time imports for AI modules (Simple)

Given the data available, this is a fairly simple task. Making an application to detect the first time that a repo adopted an AI module would give you a better idea as to when it was first used, and also when it started to gain popularity.  

A good example of this lies in [popmods.py](https://github.com/ssc-oscar/aiframeworks/blob/master/popmods.py). In this application, we can read all 32 c2bPtaPkgO$LANG.{0-31}.gz files of a given language and look for a given module with the earliest import times. The program then creates a <module_name>.first file, with each line formatted as `repo_name;UNIX_timestamp`.  

Usage: `[username@da0]~%  python popmods.py language_file_extension module_name`  

Before anything else (and this can be applied to many other programs), you want to know what your input looks like ahead of time and know how you are going to parse it. Since each line of the file has this format:  
`commit;repo_name;timestamp;author;blob;module1;module2;...`  
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

We can obtain a diff for any commit. It requires comparing trees fo it and its parent:

Lets find the diff for 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a: 
```
[username@da0]~% echo 009d7b6da9c4419fe96ffd1fffb2ee61fa61532a | ssh da4 ~/lookup/cmputeDiff2.perl 
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

For example, if the goal was to determine information pertaining to each author in WoC, simply iterating over one of the many basemaps from author to some other dataset e.g. (a2b, a2c, etc) will serve. Since these datasets are a key-value mapping from author to another dataset, this gurantees that each of the keys will be one of the unique authors in WoC, from there the desired information about that specific author can be determined. 

Below is a Perl script template that allows for retrieval of all the keys from a2c. 

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
  my $fname = "/fast/a2cFullP.$sec.tch";   
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
This script simply prints each WoC authors name. This script helps illustrate how to go about retrieving the keys in a key-value basemap using Perl, but lacks any practical use on it's own.

## Mongo Database

On the da1 server, there is a MongoDB server holding some relevant data. This data includes some information that was used for data analysis in the past. Mongo provides an excellent place to store relatively small data without requiring relational information.

On the Mongo server within the WoC database, there are some collections provided that store previously useful data. These collections store relevant metadata on the mass datatypes e.g. (authors, projects, etc). 

### MongoDB Interface

When on the da1 server, you can gain access to the MongoDB server simply by running the command 'mongo', or, when on any other da server, you can gain access by running 'mongo "da1.eecs.utk.edu"'.

Once on the server, you can see all the available Databases using the "show dbs" command. However, the database that pertains primarily to the WoC is the WoC database. 
You can switch to the WoC database, or any other, using the 'use "database name"' command. 
After switching, you can view the available collections in the database by using the 'show collections' command. 

Currently, there is an author metadata collection (auth_metadata) that contains the total number of projects an author has participated in, the total number of blobs created, the total number of commits made, and the total number of files they have created.
Alongside this, we are in the process of creating a project metadata collection that will show the language usage in projects and other relevant metadata specific to projects.

To see data in one of the collections, you can run the 'db."collection name".findOne()' command. This will show the first element in the collection and should help clarify what is in the collection.

When the above findOne() command is run on the auth_metadata collection, the output is as follows:

-----------
```
mongos> db.auth_metadata.findOne() 
{
	"_id" : ObjectId("5d9e3de9c304de5cf415ea6e"), 
	"TorvaldsIndex" : -1, 
	"TotalProjects" : 2, 
	"TotalBlobs" : -1,
	"TotalCommits" : 1,
	"AuthorID" : "  <mvivekananda@virtusa.com>",
	"hidden" : false,
	"creation_time" : ISODate("2019-10-09T20:07:05.921Z"),
	"TotalFiles" : 2
}                          
```
---------------

This metadata can then be parsed for the desired information.

Python, and most other programming languages, has an interface with Mongo that makes for data storage/retrieval much simpler. When retrieving or inputting large amounts of data onto the servers, it is almost always faster and easier to do so through one of the interfaces provided.


### PyMongo

PyMongo is an import for Python that simplifies access to the database and elements inside of it. When accessing the server you must first provide which Mongo Client you wish to connect to. For our server, the host will be "mongodb://da1.eecs.utk.edu/". 
This will allow access to the data already saved and will allow for creation of new data if desired. 

From there, accessing databases inside of the client becomes as simple as treating the desired database as an element inside the client. The same is true for accessing collections inside of a database. 
The below code illustrates this process.

--------
```python
client = pymongo.MongoClient("mongodb://da1.eecs.utk.edu/")

db = client["WoC"]                                                    
coll = db["auth_metadata"]
```
-------

#### Data Retrieval using PyMongo
When attempting to retrieve data, iterating over the entire collection for specific info is often neccesary. This is done most often through a mongo specific data structure called cursors. However, cursors have a limited life span. After roughly 10 minutes of continuous connection to the server, the cursor is forcibly disconnected. This is to limit the possible number of idle cursors connected to the server at any time. Taking this into consideration, if the process may take longer than that, it is neccesary to define the cursor as undying. If this is neccesary, manual disconnection of the cursor after it's served it's purpose is required as well.

In PyMongo, cursors can be iterated over. The below code illustrates creation and iteration with a cursor.

--------
```python
client = pymongo.MongoClient("mongodb://da1.eecs.utk.edu/")

db = client["WoC"]                                                    
coll = db["auth_metadata"]

dataset = col.find({}, cursor_no_timeout=True)
for data in dataset:
   ...

dataset.close()
```
-------

Once data retrieval has begun, accessing the specific information desired is simple. 
For example, provided above is the information saved in one element of auth_metadata. If access to the AuthorID of each cursor is desired, treat the "AuthorID" as the key in the key value mapping. However, one thing to note is that these AuthorIDs are strings. This means the way the data is stored in Mongo must be considered.

Most often, when storing data in Mongo, it will be stored in Mongo specific format called BSON. BSON objects are saved in unicode. Working with unicode can be an issue if printing needs to be done. As such, decoding from unicode must to be done. Below illustrates a small program that prints each AuthorID from the auth_metadata collection.

----------
```python
import pymongo
import bson

client = pymongo.MongoClient("mongodb://da1.eecs.utk.edu/")
db = client ['WoC']
coll = db['auth_metadata']

dataset = coll.find({}, no_cursor_timeout=True)
for data in dataset:
    a = data["AuthorID"].encode('utf-8').strip()
    print(a)

dataset.close()

```
----------

When retrieving the data, it is often neccesary to narrow the results. This possible directly through Mongo when querying for information. For instance, if all the data is not needed in the auth_metadata, simply the TotalCommits and the AuthorID you can restrict the find call by adding parameters. An example query is provided below.

----------
```python
dataset = coll.find({}, {"AuthorID": 1, "TotalCommits": 1, "_id": 0}, no_cursor_timeout=True)

for data in dataset:
    print(data)
```
---------

This specific call would allow for direct printing of the data, however, as noted above, the names are saved in BSON and as such will be printed in unicode. The first 10 results are shown below.

-------------
```
{u'TotalCommits': 1, u'AuthorID': u'  <mvivekananda@virtusa.com>'}
{u'TotalCommits': 0, u'AuthorID': u' <1151643598@163.com>'}
{u'TotalCommits': 0, u'AuthorID': u' <1615638456@qq.com>'}
{u'TotalCommits': 0, u'AuthorID': u' <182036137@qq.com>'}
{u'TotalCommits': 0, u'AuthorID': u' <1974193036@qq.com>'}
{u'TotalCommits': 0, u'AuthorID': u' <200sc@SomethingStupid.localdomain>'}
{u'TotalCommits': 0, u'AuthorID': u' <213>'}
{u'TotalCommits': 1, u'AuthorID': u' <3sodn@yuki.localdomain>'}
{u'TotalCommits': 21, u'AuthorID': u' <625605841@qq.com>'}
{u'TotalCommits': 0, u'AuthorID': u' <712641411@qq.com>'}

```
--------------

Sometimes, restricting the data even further is neccesary. Notice above that many of the users have 0 commits. If analysis of this was desired, there may be reason to exclude these entries. The below example illustrates a way to restrict the results to only users with greater than 100 commits.

----------
```python
dataset = coll.find({"TotalCommits : { "$gt" : 100 } }, 
				     {"AuthorID": 1, "TotalCommits": 1, "_id": 0}, 
				     no_cursor_timeout=True)

for data in dataset:
    print(data)
```
---------

The first 10 results are shown below.

--------------
```
{u'TotalCommits': 228, u'AuthorID': u' <bent.mozilla@gmail.com>'}
{u'TotalCommits': 137, u'AuthorID': u' <carlosborca@gmail.com>'}
{u'TotalCommits': 222, u'AuthorID': u' <carlosga_fb@users.sourceforge.net>'}
{u'TotalCommits': 466, u'AuthorID': u' <edward.lee@engineering.uiuc.edu>'}
{u'TotalCommits': 127, u'AuthorID': u' <lorenha@DESKTOP-BDBLRUP.localdomain>'}
{u'TotalCommits': 599, u'AuthorID': u' <manabu@jsk.t.u-tokyo.ac.jp>'}
{u'TotalCommits': 2633, u'AuthorID': u' <mickeyl@openembedded.org>'}
{u'TotalCommits': 4174, u'AuthorID': u' <paulb@eiffel.com>'}
{u'TotalCommits': 605, u'AuthorID': u' <romans@eiffel.com>'}
{u'TotalCommits': 147, u'AuthorID': u' <viperus@ubuntu.(none)>'}

```
-------------

