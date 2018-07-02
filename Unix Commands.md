# Display Lines with length more than 10 chars:
	awk 'length>10' file

# Display the lines for given line number
	sed -n '37p' file
	
# Find number of lines in a file:
    awk 'END {print NR}' file.txt  (or)
	wc -l file.txt       # Prints along with file name
	
# Display only specified line
	awk 'NR==5' file
	
# Print the number of characters in each line of a text file
	awk '{ print length($0); }' file

# Length of specific line in a file
	awk 'NR==5' file | wc -L (or)
	
# Display Length of Longest Line
	wc -L file

# List only directories
	ls -d */

# Display appending 2 files
	cat file1 file2

# Range of lines from a file
	awk 'FNR>=20 && FNR<=40' file_name  (or)  awk 'NR>=20 && NR<=40' file_name
 	sed -n '20,40p;41q' file_name   (or)  sed -n '20,40p' file_name

# Delete line range from a file
   sed -i '3,7d' SampleFile.txt

# Delete file content without opening the file  (* try)
	> File
	sed -n '1p;$p' file
	tail -n +2 "$FILE" | sponge "$FILE"   # + sign kind of inverts the argument. Hence prints everything except 1st line
	sed -i '1d' filename   # may not work
	sed -i '1,$d' filename
	
# Remove blank lines
	sed -i '/^$/d' file

# Remove all possible spaces at the end of the line  
	sed 's/ *$//g' file_name  

# Removes all blank lines, even ones beginning with white space	
	sed -i_bkp '/^ *$/d' file_name  # Creates a back-up file with _bkp option

# Executing two sed commands together:
	sed 's/ *$//g;/^ *$/d' file_name  	#Removes ending white spaces + blank lines in single command

# Time the operation
	time <command>
	
# Search files from given directory for a keyword
	find . -type f | xargs grep -winr "keyword"

# Delete files older than 30 days:
	find . -mtime +30 -exec rm {} \;
	
# Save the deleted files to a log file:
	find /home/a -mtime +5 -exec ls -l {} \; > mylogfile.log

# Removing files older than 7 days:
	find /path/to/ -type f -mtime +7 -name '*.gz' -execdir rm -- '{}' \;
	* More efficient
		find /path/to/ -type f -mtime +7 -name '*.gz' -print0 | xargs -r0 rm --  
	* Much faster (rm only once for all)
		find /path/to/ -type f -mtime +7 -name '*.gz' -execdir rm -- '{}' +
	* Equivalent to above but using delete
		find /path/to/ -type f -mtime +7 -name '*.gz' -delete
		
# "wait" is a built in shell command and when you execute "wait" if you 
	executed background command ("&") shell will wait and does not execute any more command until background 
	command is finished. 

# Sleep = x amount of seconds before continuing or starting 

# Remove duplicate lines in a file
	awk '!seen[$0]++' file.txt
	
# http://www.theunixschool.com/2014/08/sed-examples-remove-delete-chars-from-line-file.html	

# Identifying and removing null characters in UNIX
	sed -i 's/\x0//g' null.txt
	
# du -sh <dir-name>	
	
# Variable	Description
	$0	The filename of the current script.
	$n	These variables correspond to the arguments with which a script was invoked. 
		Here n is a positive decimal number corresponding to the position of an argument 
		(the first argument is $1, the second argument is $2, and so on).
	$#	The number of arguments supplied to a script.
	$*	All the arguments are double quoted. If a script receives two arguments, $* is equivalent to $1 $2.
	$@	All the arguments are individually double quoted. If a script receives two arguments, $@ is equivalent to $1 $2.
	$?	The exit status of the last command executed.
	$$	The process number of the current shell. For shell scripts, this is the process ID under which they are executing.
	$!	The process number of the last background command.	

# Extract common lines between files
  comm -12 File1.txt File2.txt

# List only files in the alphabetic order (In base directory only, no need to check in sub directory)
 * Using ls command list only files in a dir
    ls -l | grep -v ^d

 * For any available file
   find ./ maxdepth 1 -type f | sort -u      

 * For file withName starting with Data
   find ./ maxdepth 1 -name 'Data*' | sort -u  
   
# Replace first occurrence of the keyword
 * Ignoring case (i)
  sed -i.bkp 's/foo/bar/i' FileName
  
 * Replace 2nd Occurrence of the file
   sed 's/foo/FOO/2' 
   
   Ex: testline="foo bar foo bar foo bar foo bar"
     echo "$testline" | sed 's/foo/FOO/3g'
     foo bar foo bar FOO bar FOO bar
	 
# Replace exact word:
  * \<\> will search for exact word, " " surrounded will allow to use variables inside
   sed "s/\<sample_name\>/$SAMPLE/g" FileName
  
# Add 3rd column values if pattern matches
  awk -F '|' '$1 ~ /pattern/ {sum += $3} END {print sum}' FileName
  
# Add all values at column 3 
   (Ignores character values and considers only numericals at col 3)
   awk '{k+=$3} END {print k}' FileName 
   
# Add column values from selected row number
   ( Column to be considered = 3rd, from row 6 )
   awk 'NR>5 {k+=$3} END {print k}' FileName

# Move line/Block of lines in VI editor
  :m 12	   -move current line to after line 12  
  :5,7m 21 -move lines 5, 6 and 7 to after line 21

--------------------------------------------------------------------------------------------------------   
# Links:
 Sed usage: http://www.theunixschool.com/2014/08/sed-examples-remove-delete-chars-from-line-file.html
 awk usage: https://www.thegeekstuff.com/2010/01/awk-introduction-tutorial-7-awk-print-examples/
 xargs usgae: https://www.thegeekstuff.com/2013/12/xargs-examples/
 
--------------------------------------------------------------------------------------------------------