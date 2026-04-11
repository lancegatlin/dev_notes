```
please fix or explain why no fix the following PR comments:

C1:
<file>:<line>
<comments>
```
```
please fix up all docs in:
- staged files 

Use the following criteria to guide doc edits (more important to less important):

1. Match existing patterns: ensure new docs match existing patterns in the repo (e.g. doc style, doc location, doc level of detail)
2. Accurate: ensure documentation aligns with code
3. More Concise: where ever possible attempt to use less words to express the same ideas
4. Right Location: ensure the right comment and right level of detail for each location (e.g. top level comments high
level less detail -- comments in code, high detail)
5. Short Summary (and refer to common details comment): if a comment needs to be repeated (in same file or across
multiple files), provide a very short summary and then refer to a single doc that has the details
6. Avoid Repetition: don't repeat essentially the same comment in a single file
7. Do not use emojis or non-ascii characters (though diagram symbols are ok if no ascii alternative)
8. Let Readers Infer: assume readers have good understanding of tech used in this repo and remove or shorten any
comments those readers could infer by reading the code (focus on 'why' code is there)
```