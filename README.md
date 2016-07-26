# htcomments
Hash tag comments


## Motivation

This attempts to:
 - let people 'tag' parts of a codebase
 - provide a way to do 'codebase review' as opposed 'code review'/'commit review'/'pull request review'
 - keep some sort of referential integrity, i.e. all tags found must be 'registered' with explanations, 
else fail loudly
 - keep all tags and explanations within version control, embedded in the source code


### Ideas of tags to get you started

(prefix all of these tags with a pound sign.  I would have included it but I don't feel like fighting with 
Markdown syntax right now)

 - ht_consider_making_function
 - ht_duplicated_code
 - ht_typo_in_var_name
 - ht_possible_outdated_comment
 - ht_needs_more_comments
 - ht_dan_confused
 - ht_needs_link_to_docs
 - ht_this_is_dangerous
 - ht_style_violation
 - ht_we_have_a_function_for_this_please_use_it


### Why not just grep -r for things?

You really don't have to use this as a tool, and can just borrow the convention
of tagging and 'git grep' etc for the tags if you want.

This is by no means a replacement for grep.  However, consider a use case of
searching your codebase for everywhere you deal with "revenue".

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
  -l          list registered tags and explanations
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
exclude fake
tag #htSqlite places where we initialize sqlite connections
tag #htLanguageParse places where we write our own parsers for little languages or config files
```

The 'include' directive says to take the regex that follows and only search files with a full path matching 
that regex.  The regex must be compatible with the 're' module in Python.

The 'exclude' directive is just like the include directive. The program will apply all excludes 
before applying includes, so excludes take precedence.

The 'tag' directive must be followed by a space and a tag starting with '#ht'.


## FAQ

Q: Your people might forget to tag things... What do you do about that?
A: I don't know - this doesn't attempt to solve that.  That's probably something your org needs to 
figure out.  This was intended for big orgs with big messy repos.


## Future work
Hoping to add support for multiple repos shortly.
