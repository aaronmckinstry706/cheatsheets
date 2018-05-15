# Useful Git Commands

I need to...

* print a list of all remotes.
  > `git remote -v`

* ammend a commit message when I have *not* pushed it to the server.

  `git commit --amend` will open the default CLI text editor in order to ammend the commit message. 
  
* take not-staged changes in `<oops-branch>` and apply those changes to `<ah-branch>`.
  
  First, try the obvious: `git checkout <ah-branch>`.
  
  If that doesn't work because it says `error: your local changes would be overwritten by checkout`, then you need to:
  - `git stash push -m <optional-commit-message> <list-of-paths>` to store unstaged changes to all files in `<list-of-paths>`, or simply `git stash save <optional-commit-message>` in order to store *all* unstaged changes;
  - `git checkout <ah-branch>`;
  - `git stash apply`;
  - check to see that there are no merge conflicts between stashed changes and current branch (if so, you must resolve them), `git add` and `git commit`;
  - `git stash drop` to (sort of) delete the stash. 

* unstage uncommitted files from the index.
  
  `git rm --cached <file>`
  `git rm -rf --cached <dir>`

## Resources

[Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)
