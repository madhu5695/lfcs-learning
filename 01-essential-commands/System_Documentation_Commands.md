## System Documentation ##

1. help 
- a standalone command for shell built-ins
	Syntax: help [options] <command>
				
    options: 
	       -d: Displays a brief description of the command.
           -m: Formats the information in a style similar to a manual (man) page.
           -s: Shows only a short usage synopsis or syntax.
		   
    Example: help -m cd
		   
    Limitations: Doesn't work for external commands like ls, mkdir etc
	
2. --help				
- A flag appended to other commands

	Syntax: <command> --help
	
	Example: ls --help
	

3. man pages
- provides the most detailed information about a command, configuration file, or system call.

- man pages has 8 sections:

    Section 1: General user commands (e.g., ls, cat, rm).
    Section 2: System calls provided by the kernel (e.g., open, read, write).
    Section 3: Library functions, primarily the C standard library.
    Section 4: Special files (usually devices in /dev).
    Section 5: File formats and configuration file conventions (e.g., /etc/passwd).
	Section 6: Games  
    Section 7: Miscellaneous 
    Section 8: System administration and privileged commands (e.g., iptables). 
	
    Syntax: man [options] [section-number] <command> 
	
	Options: 
           -f : Display a concise one-line description of the command (Same as whatis).
		   -k : Search for commands related to a given keyword.
		   -a : Display all matching manual pages for the specified command.
		   -w : returns the location of the manual page for a given command.
		   -I : makes the search case-sensitive.
		   
    Examples: man -f journalctl
	         man 5 passwd


    Navigation:
           /keyword : Search inside man page
           n : Next match
           N : Previous match
	       Spacebar : Move forward one page in the manual.
           Enter : Move forward one line in the manual.
		   B : Move backward one page in the manual.
		   Q : Quit the manual viewer.
	
4. apropos
- utility used to search the manual (man) page descriptions for keywords or specific topics.
- searches the "NAME" and short description sections of all installed man pages for search term.
- similar to running man -k command
- It searches a pre-compiled database (often located at /var/cache/man/whatis)
- If it returns no results for known commands, update the database using sudo mandb

    Syntax: apropos [options] <keyword>
	
	Options: 
          -e : Returns names and descriptions that match the keyword exactly.
          -d : Prints debugging messages.
          -w : Searches for the keyword with wildcards.
          -a : Functions as logical AND. Returns output when all the keywords match.
          -l : Stops output trimming.
          -C : Uses user-configuration files instead of the $MANPATH.
          -s : Searches only in specific man pages sections.
          -M : Sets the search path to PATH rather than the default $MANPATH.
          -m : Looks for man page descriptions from other OSs.
          -L : Sets the locale for the search.
          -r : Interprets each keyword as a regex.

    Examples: apropos directory
	         apropos -a list directory
			 apropos "list directory"
			 apropos delete terminate remove
			 apropos set
			 apropos -e set
			 apropos -s 1,8 list
			 apropos '^list'
			 apropos "zip(note|cloak|info)"
			 apropos -a -s 3,8 "^list" "(implementation|devices|users)"
			 
5. info 
- provides detailed information about various Linux commands, utilities, and system functions.
- similar to the man command, but it provides a more structured and interactive way to access documentation.
    
	Syntax: info [options] <command> 
	
	Options: 
          -a : Use all matching manuals.
          -k : Look up STRING in all indices of all manuals.
          -d : Add DIR to INFOPATH.
          -f : Specify the Info manual to visit.
          -h : Display help and exit.
          -n : Specify nodes in the first visited Info file.
          -o : Output selected nodes to FILE.
          -O : Go to the command-line options node.
          -v : Assign VALUE to the Info variable VAR.
          -w : Print the physical location of the Info file.
		   
    Examples: info -a cvs 
	         info -k cvs 
			 info -d cvs 
			 info -O cvs 
			 info -w cvs
			 
6. whatis 
- A utility that provides a brief description of a command or system component by searching the system's manual page database. 

    Syntax: whatis [options] <command>
	
	Options: 
	     -a : Search for keywords in the manual page names and descriptions, instead of just the descriptions.
         -n : Specify the manual section to search, such as 1 for user commands, 5 for file formats, etc.
         -r : Interpret the keyword as a regular expression.
         -s : Specify a comma-separated list of manual sections to search.
	
	Examples: whatis -a grep
	          whatis -n 5 passwd
			  whatis -r '^ls'
			  whatis du
			  
7. /usr/share/doc 
- A directory that contains package documentation, examples, README files. 

    Example: ls /usr/share/doc

----------------------------------------------------------------------------------------------
Usage Strategy (LFCS):
- Use --help → Quick syntax
- Use man → Detailed options
- Use man -k / apropos → Find commands
- Use whatis → Quick description
- Use /usr/share/doc → Examples
-----------------------------------------------------------------------------------------------