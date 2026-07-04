
```
please review all staged changes -- please slice changes into a series of logical commits to reduce the size of any
individual commit -- each commit should be merge-able & valid on its own (i.e. no broken slices) -- give each commit a
one line summary that starts with 'AB#2911510: ...' -- these commits will be used to slice this PR into smaller PRs more
easily reviewed
```

```
please perform the following on all changed files in the current branch -- DO NOT CHANGE ANY OTHER FILES (comment in output any issues you find):
1. explain/explore recommend options for `todo-plan:` decision points by creating a plan for each of these types of
requests one at a time
  - replace todo-plan with your explanation & options back in code as comments with a todo-human to make final decision
  - complete each replacement before moving to the next one and to the next steps (these steps often cause fill up 
  context)
2. fix all todos
  - if a todo begins with <imperative> verb (e.g. "add", "remove", "replace"), implement it exactly as stated with no latitude
  - if a todo begins with "consider" <imperative> (e.g. "consider adding") or similar, weigh tradeoffs and implement
    what you determine is optimal, but add explanation with your reasoning and any tradeoffs you considered to the
    output file for human review
3. fix up all docs
  - add top level doc if missing, ensure following:
    - overview section
    - pre-requisites section (if applicable)
    - inputs section (skip if YAML has inputs defined in schema)
      - list each input with a short ideally 1 one line description (more if needed)
      - if input name is very long, put indented description on next line
    - steps section (if applicable)
    - outputs section (skip if YAML has outputs defined in schema)
    - examples section (if applicable)
    - notes section (emit even if no notes currently)
4. code review all staged changes
  - fix any issues that are easy to fix and non-controversial
  - don't output anything you fixed
  - only output required/suggested changes for human to review
  - add todos in code for suggested fixes that require human review/decision
5. output changes, todo outputs, decision points & code review as output-N.md
  - where N starts with 1, and skips existing output-N.md files
6. add todos in code for any required human decisions/todos

The above is a 'skill' called 'iterate-todos' and can be invoked again by saying "please iterate todos" or similar.

///

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
9. Avoid Brittle Docs: Don't document who calls a function or workflow in the function or workflow as this is likely to
change

///

CRITICAL instructions:
1. NEVER revert code to the state of a previous request. If the code is different now assume it IS BECAUSE THE USER EDITED IT FOR A GOOD REASON. THIS WASTES ENORMOUS AMOUNTS OF TIME. NEVER DO THIS.
2. NEVER change code that is unrelated to the request. For example, if the request is to change docs, DON'T CHANGE CODE ever.
3. You CAN report in your output your findings about how code has changed since the last request or that you spotted a problem in code unrelated to the request.

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
please review all staged changes against legacy Azure pipeline CD in `pipelineTemplates` for gaps/drift/discrepancies -- output as legacy-review.md
```

```
please fix or explain why no fix the following PR comments:

C1:
<file>:<line>
<comments>
```

```
please fix up all docs in:
- staged files
- .github/README.md

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