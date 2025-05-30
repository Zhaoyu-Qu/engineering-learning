# Git Notes

## Concepts
- A file is called a “blob”, standing for `Binary Large Object`. Git treats all files as binary files. It does not differentiate between file types.

- A directory is called a “tree”. `Tree` or `directory` is a high-level concept referring to the hierarchical structure in a repository. The physical representation of a tree is called a `tree object`, a file with its internal structure. Internally, a tree object contains a list of entries, and each entry is composed of:
  - Mode: Indicates file permissions and the type of entry (e.g., 040000 for directories).
  - Type
  - SHA-1 Value
  - File Names

  Example:

  jason.qu@xxxxxx Jason-qu-grad-engineering-portfolio % git cat-file -p 2c6c02a91ea49eec2de7983ab7b260980a1a0905  
  100644 blob 8561f996434b4e4340186988d817e6cc6ba1e13e    Readme.md  
  040000 tree bf2b4eb6d7cc16a3eb29550ed21dff0cf783f0ac    general  
  040000 tree 19abd38933c8333e273d978ab76c61cf08f2ec82    human-skills  
  040000 tree 83f0a9a54b15e306ee489f5a3f7bce94b228c7dc    optional-technical-skills  
  040000 tree 60b89622167a7db0681d0612dd7c1d9e23eb1d05    technical-skills  

- A snapshot is the state of the entire repository (i.e., the top-level tree) at a specific point in time. Git calls snapshots `commit`s. A commit object has an internal structure too, which is composed of:
  - parents: a set of commits
  - author: string
  - message: string
  - snapshot: tree (usually/always the top-level tree since it captures the state of the entire repository)

  Example:

  jason.qu@xxxxxx Jason-qu-grad-engineering-portfolio % git cat-file -p 0cc765b7cf7ccc80baf3c7cc1a416800e1585695  
  tree d1810e95fd6893e19743eb17c439773904733a9f  
  parent f158e38d91920ea02af583883355c8da3e287a73  
  parent b37fe4d165c7f668120fbbcd7bc09c0fb124f7cb  
  author Jason Qu <jason.qu@xxx.com.au> 1746354582 +1000  
  committer Jason Qu <jason.qu@xxx.com.au> 1746354582 +1000  

  Merge remote-tracking branch 'remotes/my/main'  
  update GitHub Action notes  

- A SHA-1 value reflects the state of an object **at a particular point in time**. Whenever the state of an object changes, a new SHA-1 value will be generated for the object, and the old SHA-1 value together with the state that it represents will be preserved as part of the repo's history. Over time, if objects are no longer reachable from any reference, they will be pruned by a garbage collection (GC) process to save space.

  Blobs, trees and commits are all objects identified by unique SHA-1 hash values. They are all stored in `.git/objects` - all git objects are stored the same way.

## Mechanism
In Git, a history is a directed **acyclic** graph (DAG) of commits. Each commit has a set of parents rather than a single parent because a commit might descend from multiple parents, for example, due to combining (merging) two parallel branches of development. All Git stores are objects and references. Every command you type is manipulating the underlying graph data structure.

While each commit can be identified by its SHA-1 hash, it is impractical for humans to remember strings of 40 hexadecimal characters. The solution is to use `references`, which are mutable pointers to commits. For example, the `main` reference usually points to the latest commit in the main branch of development. The `HEAD` reference points to where we currently are.

### Staging Area
The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit.  
You need to specify which modifications should be included in the next snapshot by adding the files to the “staging area” (`git add`). Simply saving the files is not enough.

## Commands
### Basics
- `git init`
- `git status`
- `git add path/to/file`
- `git add --all`
- `git commit --message <message>` commits staged files
- `git log`
- `git diff`
- `git checkout` updates HEAD and current branch
- `git stash`
- `git stash apply` applies a stash (default is the latest, named stash@{0})

### Branching and merging
- `git branch --all` show all branches
- `git branch <name>` creates a branch
- `git checkout -b <name>` creates a branch and switches to it
- `git merge <revision>` merges into current branch
- `git rebase`

### Remotes
- `git remote --verbose` lists existing remotes with their names and URLs
- `git fetch` fetches data from the remote repository without affecting the local working directory or branches.
- `git pull` same as `git fetch` + `git merge`
- `git clone`

### Undo
- `git reset --soft HEAD~1` undoes the last commit. All committed changes will be put back to the staging area. The old commit object is not deleted, but since it has become unreachable, it may get pruned by the GC process at some point.
- `git reset --hard HEAD~1` undoes the last commit and discards the changes. The old commit object is still not immediately deleted. The difference is that the undone changes won't show up in the staging area since they are discarded. But you can still retrieve the old commit as long as it hasn't been deleted by the GC process.
- `git reset` unstages everything
- `git commit --amend` edits a commit's contents/message
- `git checkout -- <file>` discards changes

## GitHub CLI commands
### Authentication
- `gh auth login`
- `gh auth status` checks if you are logged in
- `gh auth logout`
- `gh auth switch` switches to (activates) a different account