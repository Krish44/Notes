
### env setup
###### printenv - print all or part of environment
	Ex         : printenv HOME  
	similar to : echo $HOME
###### Release check
	cat /etc/redhat-release  # redhat
	cat /etc/lsb-release  # To get more details on OS

###### script syntax check
	sh -n <script name>

###### special char check in a file
	cat -A <FileName>
	
### Parameter handling
###### Variable	Description
	$0	The filename of the current script.
	$n	These variables correspond to the arguments with which a script was invoked. 
		Here n is a positive decimal number corresponding to the position of an argument 
		(the first argument is $1, the second argument is $2, and so on).
	$#	The number of arguments supplied to a script.
	$*	All the arguments are double quoted. 
		If a script receives two arguments, $* is equivalent to $1 $2.
	$@	All the arguments are individually double quoted. 
	        If a script receives two arguments, $@ is equivalent to $1 $2.  
	$?	The exit status of the last command executed.
	$$	The process number of the current shell. For shell scripts, this is the process ID 
	        under which they are executing.
	$!	The process number of the last background command.	
	
### Directory and Disk handling
###### List only directories
	ls -d */

###### List only hidden files in a directory
	ls -d .!(|.)
	(or)
	ls -d .?*
		
###### Get hidden files   
	ls -l .??*  
	(or)  
	ls  -a| grep -E "^\."   
   
   
###### Sort as per size   
	ls -S | head -5   
	(or)  
	ls -s| sort -nr | head -5   

###### Multiple Directory Structure in a single command
	mkdir -p {a,b,c}    # Multiple directories in the same location
	mkdir -p a/b/c    # Diretory chain
	mkdir -p {a/a1/a2,b/{b1,b2},c}

###### Size of a directory
	du -sh <dir-name>	
###### Available space in current disk
	df -kh .
	
###### Dirname opp to basename 
	INPUT:  /home/parent/child1/filename   
		dirname INPUT  
	output: /home/parent/child1/ 
	    
		basename INPUT
	output: filename 

### User
###### Get user's details
	id -a <UserID>             # Gives UID, GID
	#The root user has a UID of 0
	
### vi editor
###### Move line/Block of lines in VI editor
	  :m 12	   #move current line to after line 12    
	  :5,7m 21 #move lines 5, 6 and 7 to after line 21  

###### Temp exit to command mode
	:!sh     (or)   :sh  (or)  :bash  
	$exit  - to get back   

Directly from the editor:  	

	:! <command>  
	Ex: :! pwd 
	
### File management
###### Display appending 2 files
	cat file1 file2
###### Delete file content without opening the file  (* try)
	> File
	sed -n '1p;$p' file
	tail -n +2 "$FILE" | sponge "$FILE"   # + sign kind of inverts the argument. Hence prints everything except 1st line
	sed -i '1,$d' filename

###### Search files from given directory for a keyword
	find . -type f | xargs grep -winr "keyword"

###### Find all empty files (zero byte file)    
	find ./ -maxdepth 3 -empty   
	find ~ -empty     # (From home directory)   
   
###### List only the non-hidden empty files only in the current directory.   
	find . -maxdepth 1 -empty -not -name ".*"   

###### exec and {}  
	find src -name "*.java" -type f -exec grep -l interface {} \;   
	* Used the -exec option to execute the grep command on the list of files returned by the find command.  
	* Semi-colon at the end causes the grep command to be executed for each file, one at a time, as the {} is replaced by the current file name.  
	* Backslash is required to escape the semi-colon from being interpreted by the shell.  
	
###### Delete files older than 30 days:
	find . -mtime +30 -exec rm {} \;

###### Save the deleted files to a log file:
	find /home/a -mtime +5 -exec ls -l {} \; > mylogfile.log

###### Removing files older than 7 days:
	find /path/to/ -type f -mtime +7 -name '*.gz' -execdir rm -- '{}' \;
	* More efficient
		find /path/to/ -type f -mtime +7 -name '*.gz' -print0 | xargs -r0 rm --  
	* Much faster (rm only once for all)
		find /path/to/ -type f -mtime +7 -name '*.gz' -execdir rm -- '{}' +
	* Equivalent to above but using delete
		find /path/to/ -type f -mtime +7 -name '*.gz' -delete
###### Delete file starting with special char using inode number
	Get the inode number of the file
		ls -i
	Remove the file using find commad
		find . -inum 106701465 | xargs rm 
###### Identifying and removing null characters from file in UNIX
	sed -i 's/\x0//g' null.txt
	
###### http://www.theunixschool.com/2014/08/sed-examples-remove-delete-chars-from-line-file.html

###### tee command
	- Output of a program can be both displayed and saved in a file
	wc -l file1.txt| tee file2.txt
	Displays on screen as well as output wil be moved to file too.

###### Extract common lines between files
     comm -12 File1.txt File2.txt
###### Find lines from a file which are not present in another file
     comm -23 a.txt b.txt
    By default, comm outputs 3 columns: left-only, right-only, both.  
    The -1, -2 and -3 switches suppress these columns.   
    So, -23 hides the right-only and both columns,  
    showing the lines that appear only in the first (left) file.   

###### Find Files based on size
    To find files larger than 100MB:  
		find . -type f -size +100M	
    To find files smaller than 100MB:
		find . -type f -size -100M  
	
###### Sort texts inside a file in alphabetic order
	sort <fileName> -o <fileName>	

###### List only files in the alphabetic order (In base directory only, no need to check in sub directory)
 	* Using ls command list only files in a dir  
       		ls -l | grep -v ^d  
 	* For any available file   
       		find ./ -maxdepth 1 -type f | sort -u        
 	* For file withName starting with Data    
       		find ./ -maxdepth 1 -name 'Data*' | sort -u   
 	 
###### Replace exact word:
\<\> will search for exact word, " " surrounded will allow to use variables inside  

	sed "s/\<sample_name\>/$SAMP  
###### Replace first occurrence of the keyword
	* Ignoring case (i)  
		sed -i.bkp 's/foo/bar/i' FileName
	* Replace 2nd Occurrence of the file    	
		sed 's/foo/FOO/2'   
	
	Ex: testline="foo bar foo bar foo bar foo bar"  
		echo "$testline" | sed 's/foo/FOO/3g'  
	o/p: foo bar foo bar FOO bar FOO bar
	
##### awk syntax
	BEGIN { Actions}   
	{ACTION} # Action for everyline in a file   
	END { Actions }   

	# is for comments in Awk  

	awk '/search pattern1/ {Actions}
	     /search pattern2/ {Actions}' file
	     
###### sample data:   
	$cat employee.txt   
	100  Thomas  Manager    Sales       $5,000   
	200  Jason   Developer  Technology  $5,500   
	300  Sanjay  Sysadmin   Technology  $7,000   
	400  Nisha   Manager    Marketing   $9,500   
	500  Randy   DBA        Technology  $6,000   
   
###### Print 2 matching patterns   
	awk '/Thomas/   
	> /Nisha/' employee.txt   
   
###### Print records whose 1st field value greater than 200   
	awk '$1 >200' employee.txt   
   
###### Print records corresponding to Technology dept   
	awk '$4 ~/Technology/' employee.txt   
   
###### Get the count for a matching patrtern in a specific field   
	awk 'BEGIN { count=0;}   
	$4 ~ /Technology/ { count++; }   
	END { print "Number of employees in Technology Dept =",count;}' employee.txt

###### Add 3rd column values if pattern matches
  	awk -F '|' '$1 ~ /pattern/ {sum += $3} END {print sum}' FileName
  
###### Add all values at column 3 
Ignores character values and considers only numericals at col 3  
	
	awk '{k+=$3} END {print k}' FileName 
   
###### Add column values from selected row number
	   (Column to be considered = 3rd, from row 6 )  
	   	awk 'NR>5 {k+=$3} END {print k}' FileName

###### Print range of char in a file  
	* If range is known		
		cut -c 1-7 state.txt  
	* Till end   	
		cut -c 1- state.txt  
		
###### Gives last field in a file  
	awk -F"/" '{print $NF}' file  
	
###### Print rows between pattern  
		awk '/PAT1/,/PAT2/' file    
	Print lines between PAT1 and PAT2 - not including PAT1 and PAT2   
		awk '/PAT1/{flag=1; next} /PAT2/{flag=0} flag' file  
	
	https://stackoverflow.com/questions/38972736/how-to-select-lines-between-two-patterns  

###### Extract 2nd field after the search key  
		awk '/potato:/ {print $2}'  file.txt
	Equivalent to:   
		grep 'potato:' file.txt | cut -d"\" -f2 
		
### Lines  
###### Find number of lines in a file:
    awk 'END {print NR}' file.txt  (or)
	wc -l file.txt       (Prints along with file name)
	wc -l < file.txt     (Prints only line number)  
	
###### Display only specified line
	awk 'NR==5' file
	sed -n '37p' file
	
###### Print the number of characters in each line of a text file
	awk '{ print length($0); }' file
	
###### Display Lines with length more than 10 chars:
	awk 'length>10' file
	
###### Length of specific line in a file
	awk 'NR==5' file | wc -L (or)
	
###### Display Length of Longest Line
	wc -L file

###### Remove duplicate lines in a file
	awk '!seen[$0]++' file.txt
	
###### Range of lines from a file
	awk 'FNR>=20 && FNR<=40' file_name  (or)  awk 'NR>=20 && NR<=40' file_name
 	sed -n '20,40p;41q' file_name   (or)  sed -n '20,40p' file_name

###### Delete line range from a file
     sed -i '3,7d' SampleFile.txt
    
###### Delete lines containing multiple keywords
     sed '/Dayan\|RinRao/d' records.txt

###### Remove blank lines
	sed -i '/^$/d' file
	
###### Remove all possible spaces at the end of the line  
	sed 's/ *$//g' file_name  

###### Removes all blank lines, even ones beginning with white space	
	sed -i_bkp '/^ *$/d' file_name  # Creates a back-up file with _bkp option

###### Executing two sed commands together:
	sed 's/ *$//g;/^ *$/d' file_name  	#Removes ending white spaces + blank lines in single command

### Time management
###### Time the operation
	time <command>
		
###### "wait" is a built in shell command and when you execute "wait" if you 
	executed background command ("&") shell will wait and does not execute any more command  
	until background command is finished.  

###### Sleep
	Sleep = x amount of seconds before continuing or starting 	

### String operations
###### substr
	a=hello
	b=`expr substr $a 2 3` 
	echo $b  # output: ell
###### rev command  
	Reverses the input  string
     
--------------------------------------------------------------------------------------------------------   
### Links:
  - Sed usage: http://www.theunixschool.com/2014/08/sed-examples-remove-delete-chars-from-line-file.html
  - awk usage: https://www.thegeekstuff.com/2010/01/awk-introduction-tutorial-7-awk-print-examples/  
               https://www.thegeekstuff.com/2010/02/awk-conditional-statements/  
	       https://www.thegeekstuff.com/2010/01/8-powerful-awk-built-in-variables-fs-ofs-rs-ors-nr-nf-filename-fnr/  
  - xargs usgae: https://www.thegeekstuff.com/2013/12/xargs-examples/  
  - find command: https://www.thegeekstuff.com/2009/03/15-practical-linux-find-command-examples/
  - Parameter Expansion: http://mywiki.wooledge.org/BashFAQ/073?action=show&redirect=ParameterExpansion  
  - IO redirection: http://tldp.org/LDP/abs/html/io-redirection.html   
  - Vim moves: https://medium.com/usevim/vim-101-quick-movement-c12889e759e0
  - User setups: https://linuxize.com/post/how-to-setup-passwordless-ssh-login/
                 https://linuxize.com/post/create-a-sudo-user-on-centos/
		 https://linuxize.com/post/how-to-create-a-sudo-user-on-ubuntu/
  - Tricky:  http://www.theunixschool.com/2012/03/join-every-2-lines-in-file.html
 
--------------------------------------------------------------------------------------------------------
