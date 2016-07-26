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

 - #ht_consider_making_function
 - #ht_duplicated_code
 - #ht_typo_in_var_name
 - #ht_possible_outdated_comment
 - #ht_needs_more_comments
 - #ht_dan_confused
 - #ht_needs_link_to_docs
 - #ht_this_is_dangerous
 - #ht_style_violation
 - #ht_we_have_a_function_for_this_please_use_it

Or, more business oriented:

 - #ht_revenue_logic
 - #ht_a_b_tests
 - #ht_product_pricing
 - #ht_acme_corp_gold_rewards
 - #ht_imputation
 - #ht_decision_waivers

### Why not just grep -r for things?

You really don't have to use this as a tool, and can just borrow the convention
of tagging and 'git grep' etc for the tags if you want.

This is by no means a replacement for grep.  However, consider a use case of
searching your codebase for everywhere you deal with "revenue".

Did the developer use the term "revenue"?  Or did they use the term "business_income"?  Or did they 
use the term "business_ear" (for estimated annual revenue)?  Or did they just type "annual_rev"?  

Do you want to search places where revenue is declared in a class file?  Do you only want parts 
where you do logic around revenue?  A grep -i for "revenue" will probably turn up both.  If you 
only want places where there is business logic around revenue, maybe revenue_logic is a worthwhile 
tag. Your organization may want to be able to make such a distinction, depending on who's looking 
at what code.

Also, grep -r requires arguments to exclude files you may not want, like files with the wrong extensions 
or files in a folder full of unused stuff. This lets you specify the patterns just once, and store the preferences 
in Git.


## Usage

```

usage: htcomments [-h] [-c] [-l] [-o] [-t T] [-v]

optional arguments:
  -h, --help  show this help message and exit
  -c          count by tag
  -l          list registered tags and explanations
  -o          open results in pager
  -t T        tag to search. pound sign optional
  -v          validate tags in codebase
```

Put the htcomments executable on your path.  Execute it inside of a git repo.

'htcomments -t ht_duplicated_code' would show you code snippets near that tag in your 
codebase.

If you are having trouble finding the exact tag name, you could try 'htcomments -l | grep dup', 
for example.


## Configuration file

Put a '.htcomments' file in your repo root.  Here is an example:

```
include py$
exclude json$
tag #ht_duplicated_code places where we seem to be copy pasting a lot
tag #ht_a_b_test places where we do logic around a/b tests
tag #ht_should_use_existing_function places where we seem to not know about an existing utility function
```

The 'include' directive says to take the regex that follows and only search files with a full path matching 
that regex.  The regex must be compatible with the 're' module in Python.

The 'exclude' directive is just like the include directive. The program will apply all excludes 
before applying includes, so excludes take precedence.

The 'tag' directive must be followed by a space and a tag starting with '#ht_'.


## Anticipated FAQ

Q: Using both # and ht seems kind of redundant.  Why the #ht_ prefix?  Why not just ht_ or #?
A: Just '#' is too likely to  yield false positives in normal comments.  #ht_ makes it more likely that it's a comment and not a variable name, even for languages where # doesn't indicate a comment. Just 'ht_' might conflict with variable names etc.

Q: I don't want to have to use this script right now.  What should I do?
A: Just grep for the tags that others have left.  But, if leaving your own tags, you will have to work hard to make sure you are not mistyping tags or that each tag you leave is "registered" and explained.

Q: I am in the middle of coding and it's tough to look up the right tag name constantly.  How can I make this easier?
A: If you use vim, try using a little vimscript for this.  You can also just keep your tags open in any editor.

Q: Your people might forget to tag things... What do you do about that?
A: I don't know - this doesn't attempt to solve that.  That's probably something your org needs to 
figure out.  This was intended for big orgs with complex/messy codebases.

## Future work
Hoping to add support for multiple repos eventually.
