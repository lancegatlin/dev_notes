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

CRITICAL local dev environment notes:
1. terminal is zsh running MacOS
2. use git --no-pager
3. terminal heredocs often fail causing user to have to CTRL-C cancel -- write temp files (use /tmp/llm/) instead (clean them up when done)
4. when reading files from terminal, pipe ('|') may appear missing -- if this happens, read file normally
5. the terminal often stops returning output to LLM (user still sees terminal output) -- if this happens, redirect output to temp file and read the temp file
6. ivy cache is no longer used -- see Coursier local cache here: ~/Library/Caches/Coursier/v1
7. read ~/Code/cat.prm/gitm.yml to discover location of other repos -- each located under the ~/Code/cat.prm directory

```

```
please code review all staged changes, number all issues identified, output as code-review.md

CRITICAL local dev environment notes:
1. terminal is zsh running MacOS
2. use git --no-pager
3. terminal heredocs often fail causing user to have to CTRL-C cancel -- write temp files (use /tmp/llm/) instead (clean them up when done)
4. when reading files from terminal, pipe ('|') may appear missing -- if this happens, read file normally
5. the terminal often stops returning output to LLM (user still sees terminal output) -- if this happens, redirect output to temp file and read the temp file
6. ivy cache is no longer used -- see Coursier local cache here: ~/Library/Caches/Coursier/v1
7. read ~/Code/cat.prm/gitm.yml to discover location of other repos -- each located under the ~/Code/cat.prm directory
```

```
please produce a pre-merge checklist markdown snippet that I can copy to the PR description that includes everything I need to do prior to merging this branch
```

```
please rephrase this comment using less words but retaining the same meaning
```