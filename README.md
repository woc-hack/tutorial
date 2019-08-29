# Tutorial basics for Hackathon
-------
## Getting started
### Set up accounts for GitHub and Bitbucket
If you dont have these already, go ahead and setup an account on both GitHub and Bitbucket and give your GH and BB ids and email to Dr. Mockus, who will then invite you to the oscar.py and swsc repositories via email.  
GitHub: https://github.com/pricing  
BitBucket: https://bitbucket.org/account/signup/  
--------

### Gain permissions and access to da server(s)
In order to gain access to one of the da servers, you will need to generate a public key via the `ssh-keygen` command, and put the `id_rsa.pub` and `id_rsa` files inside your .ssh/ folder on your home terminal. Once finished, send the contents of `id_rsa.pub` (your public key) over to Dr. Mockus who will then grant you access to one or more of the da servers.  
Optionally, you can also set up your `.ssh/config` file so that you can login to one of the da servers without having to fully specify the server name each time:  
```
Host da0
	Hostname da0.eecs.utk.edu
	Port 443
	User {username}
```

Logging in then becomes as simple as `ssh da0`.  
Once you are in a da server, you will have an empty directory under `/home/username` where you can store your programs and files:  
```
[username@da0]~% pwd
/home/username
[username@da0]~% 
```
---------
### Clone the oscar.py and swsc/lookup repos
oscar.py link: https://github.com/ssc-oscar/oscar.py  
swsc/lookup link: https://bitbucket.org/swsc/lookup/src/master/

Run `git clone <link>` (no brackets) on a da server to get a copy of the given repository link.  

-------
## List of relevant directories

The folder structure on any server follows the following convention:
    - Raw blobs are located in files that are appended as new
      objects are discovered and extracted
      /data/All.blobs/{commit,tree,tag,blob}_Num.{idx,bin}  where
      Num is in {0..127}
   - Folder /fast is preferably mounted on an array of SSDs that
     so that the data can be read in parallel, but for servers that
     do not have SSDs, a regular disk is used. The maps (e.g.,
     c2fFull$Ver.Num.tch are typically stored there and, as a
     backup, on /da0_data/basemaps
	 /fast has subfolders: All.sha1, All.sha1c, and
     All.sha1o. All.sha1/sha1.{commit,tree,tag,blob}_Num.tch holds
     object content offsets in the raw files stored /data/All.blobs
     and are used to check if the object is in the database and, if
     so, which record. All.sha1c/{commit,tree}_Num.tch maps commit
     and tree sha1s to the object content. All.sha1o/blob_Num.tch
     maps blob sha1 to the file ofset (and object size) that can be
     read directly from /data/All.blobs/blob_Num.bin


Not all files are stored on all servers due to limited disk sizes.
The description below goes over what is stored on each server. 

In order for SSDs to be fast they need to be mounted in parallel,
for example id 7ssds is a volume group that has seven SSDs as
physical volumes, LV 7ssds can be created via
```
lvcreate --extents 100%FREE --name 7ssds --stripes 7 --stripesize 256 7ssds
```

### da0 Server
#### <relationship>.{0-31}.tch files in `/data/basemaps/`:  
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
* trp = Torvald Path

List of relationships:
```
* a2c (.s)		* a2f			* a2ft				* a2L (.s only)		* a2p (.s)			* a2trp0 (.s)
* b2c (.s)		* b2f (.s)
* c2b (.s)		* c2cc			* c2f (.s)		
* c2h			* c2pc			* c2p (.s)			* c2ta (.s)
* f2b (.s)		* f2c (.s)		
* p2a (.s)		* p2c (.s)
```	
------
#### `/data/play/$LANGthruMaps/` on da0:  
These thruMaps directories contain mappings of repositories with modules that were utilized at a given UNIX timestamp under a specific commit. The mappings are in c2bPtaPkgO{$LANG}.{0-31}.gz files.   
Format: `commit;repo_name;timestamp;author;blob;module1;module2;...`  
Each thruMaps directory has a different language ($LANG) that contains modules relevant to that language.
------
### da3 Server
#### .tch files in `/fast/`:  
da3 contains the same files located on da0, except for b2f, c2cc, f2b, and f2c.
This folder can be used for faster reading, hence the directory name.  
In the context of oscar.py, the dictionary values listed in the PATHS dictionary can be changed from `/da0_data/basemaps/...` to `/fast/...` when referencing oscar.py in another program.  
------
## OSCAR functions from oscar.py
Note: "/<function_name>" after a function name denotes the version of that function that returns a Generator object  

These are corresponding functions in oscar.py that open the .tch files listed above for a given entity:

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
------
## Examples of doing certain tasks
* Get a list of commits and repositories that imported Tensorflow for .py files:  
	On da0: `UNIX> zcat /data/play/PYthruMaps/c2bPtaPkgOPY.0.gz | grep tensorflow`  
	Output: 
```
0000331084e1a567dbbaae4cc12935b610cd341a;abdella-mohamed_BreastCancer;1553266304;abdella <abdella.mohamed-idris-mohamed@de.sii.group>;0dd695391117e784d968c111f010cff802c0e6d1;sns;keras.models;np;random;tensorflow;os;pd;sklearn.metrics;plt;keras.layers;yaml
00034db68f89d3d2061b763deb7f9e5f81fef27;lucaskjaero_chinese-character-recognizer;1497547797;Lucas Kjaero <lucas@lucaskjaero.com>;0629a6caa45ded5f4a2774ff7a72738460b399d4;tensorflow;preprocessing;sklearn
000045f6a3601be885b0b028011440dd5a5b89f2;yjernite_DeepCRF;1451682395;yacine <yacine.jernite@nyu.edu>;4aac89ae85b261dba185d5ee35d12f6939fc2e44;nn_defs;utils;tensorflow
000069240776f2b94acb9420e042f5043ec869d0;tickleliu_tf_learn;1530460653;tickleliu <tickleliu@163.com>;493f0fc310765d62b03390ddd4a7a8be96c7d48c;np;tf;tensorflow
.....
```
* Get a list of commits made by a specific author:  
	On da0: `UNIX> zcat /data/basemaps/gz/a2cFullP0.s | grep "Albert Krawczyk" <pro-logic@optusnet.com.au>`  
	Output:  
```
"Albert Krawczyk" <pro-logic@optusnet.com.au>;17abdbdc90195016442a6a8dd8e38dea825292ae
"Albert Krawczyk" <pro-logic@optusnet.com.au>;9cdc918bfba1010de15d0c968af8ee37c9c300ff
"Albert Krawczyk" <pro-logic@optusnet.com.au>;d9fc680a69198300d34bc7e31bbafe36e7185c76
```
* Do the same thing above using oscar.py:  
```
UNIX> python
>>> from oscar import Author
>>> Author('"Albert Krawczyk" <pro-logic@optusnet.com.au>').commit_shas
('17abdbdc90195016442a6a8dd8e38dea825292ae', '9cdc918bfba1010de15d0c968af8ee37c9c300ff', 'd9fc680a69198300d34bc7e31bbafe36e7185c76')
```
* Get the URL of a projects repository using the oscar.py `Project(...).toURL()` function:  
```
UNIX> python
>>> from oscar import Project
>>> Project('notcake_gcad').toURL()
'https://github.com/notcake/gcad'
```
-------	
## Examples of implementing applications -- Simple vs. Complex
### Finding 1st-time imports for AI modules (Simple) 
Given the data available, this is a fairly simple task. Making an application to detect the first time that a repo adopted an AI module would give you a better idea as to when it was first used, and also when it started to gain popularity.  

A good example of this lies in [popmods.py](https://github.com/ssc-oscar/aiframeworks/blob/master/popmods.py). In this application, we can read all 32 c2bPtaPkgO$LANG.{0-31}.gz files of a given language and look for a given module with the earliest import times. The program then creates a <module_name>.first file, with each line formatted as `repo_name;UNIX_timestamp`.  

Usage: `UNIX> python popmods.py language_file_extension module_name`  

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

If you want to compare first-time usage over time for Tensorflow and Keras for the .ipynb language .first files you created, run: `UNIX> python3.6 modtrends.py tensorflow.first keras.first`  
The final graph looks something like this:  
[![Tensorflow vs Keras](../ipynb_first/Tensorflow-vs-Keras.png "Tensorflow vs Keras")](https://github.com/ssc-oscar/aiframeworks/blob/master/charts/ipynb_charts/Tensorflow-vs-Keras.png)
-------
### Detecting percentage language use and changes over time (Complex) 
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

--------
## Useful Python imports for applications
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
----------
## oscar.py vs. Perl scripts
When it comes to creating new relationship files (.tch/.s files), using Perl over Python for large data-reading is more time-saving overall. This situation occurred in the complex application we covered where we modified an existing Perl file to get the initial commit times of each file for each author, rather than using Python to accomplish this task.  
Before making this decision, one of our team members decided to run a test between 2 programs, [a2ft.py](https://bitbucket.org/swsc/lookup/src/master/a2ft.py) and [a2ft.perl](https://bitbucket.org/swsc/lookup/src/master/a2ft.perl). These programs were run at the same time for a period of 10 minutes. Both programs had the same task of retrieving the earliest commit times for each file under each author from a2cFullP{0-31}.s files. The Python version calls the `Commit_info().time_author` and `Commit().changed_file_names` functions from oscar.py. The Perl version ties each of the 32 c2fFullO.{0-31}.tch (Commit().changed_file_names) and c2taFullP.{0-31}.tch (Commit_info().time_author) files into 2 different Perl hashes (Python dictionary equivalent), %c2f and %c2ta. The speed difference between Perl and Python was quite surprising:  
```
[dkennard@da3]/data/play/dkennard% ll a2ftFullP0TEST1.s
-rw-rw-r--. 1 dkennard dkennard 980606 Jul 22 11:56 a2ftFullP0TEST1.s
[dkennard@da3]/data/play/dkennard% ll a2ftFullPTEST2.0.tch
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
