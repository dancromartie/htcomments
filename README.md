# htcomments
Hash tag comments


## Motivation

This attempts to:
 - let people 'tag' parts of a codebase
 - keep some sort of referential integrity, i.e. all tags must be 'registered' with explanations
 - keep all tags and explanations within version control, embedded in the source code
 - be really simple so almost anyone can contribute tags

### Why not just grep -r for things?
This is by no means a replacement for grep.  It's more geared towards easing governance and 
reviews by a wide audience - one that has the resources to go around tagging things and the vastness 
to need such a process.

Consider a use case of searching your codebase for everywhere you deal with "revenue".

Did the developer use the term "revenue"?  Or did they use the term "business_income"?  Or did they 
use the term "business_ear" (for estimated annual revenue)?  Or did they just type "annual_rev"?  

Do you want to search places where revenue is declared in a class file?  Do you only want parts 
where you do logic around revenue?  A grep -i for "revenue" will probably turn up both.  If you 
only want places where there is business logic around revenue, maybe revenueLogic is a worthwhile 
tag. Your organization may want to be able to make such a distinction, depending on who's looking 
at what code.

Also, grep -r requires arguments to exclude files you may not want, like files with the wrong extensions 
or files in a folder full of unused stuff. This lets you specify the patterns just once, and store the preferences 
in Git.


## Usage

```
usage: htcomments [-h] [-l] [-t T]

optional arguments:
  -h, --help  show this help message and exit
  -l          list registered tags
  -t T        tag to search. pound sign optional
```

Put the htcomments executable on your path.  Execute it inside of a git repo.

'htcomments -t htRevenue' would show you code snippets where you have tagged 'htRevenue' in your 
codebase.  Presumably you tagged this because it shows up in many parts of your codebase and 
there is somebody who cares about finding it easily.

If you are having trouble finding the exact tag name, you could try 'htcomments -l | grep rev', 
for example.


## Configuration file

Put a '.htcomments' file in your repo root.  Here is an example:

```
include py$
tag #htSqlite
tag #htLanguageParse
```

The 'include' directive says to take the regex that follows and only search files matching 
that regex.  The regex must be compatible with the 're' module in Python.

The 'tag' directive must be followed by a space and a tag starting with '#ht'.
