# Coolblue Git Workshop

Clone this repository if you want to do the Coolblue Git workshop!

`git clone https://github.com/coolblue-development/git-workshop`



## Setup

Use the command line, you will get more out of the workshop that way. Plus all of the exercise answers are given as command line commands.

### Handy git log view

To check your progress you will need to see the git log of this repository. If you have a graphical git client that you like, you can use that. Alternatively, set up this git alias to print a nice git log in your console:

`git config --global alias.l "log --oneline --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset'"`

Now you can run `git l` to get a nicely formatted log.

### VI Editor

When git needs you to enter something it will open the VI Editor and ask you to edit the
file in the editor. VI is confusing to use if you are not familiar with it, so here's a
quick cheatsheet:

* `i` Enter insert mode, so now you can type
* `<esc>` Exit insert mode and go back to command mode
* `y` Yank, cut the current line
* `p` Paste the cut line below the current line
* `dd` Delete a line
* `:wq` Write and quit


## Exercises

You're ready to go. All the exercises deal with making changes to the file `shopping.list` that you can find in the root of this repository.


### Exercise 1: Change the last commit

You have just realised that your last commit has a mistake, you forgot to add milk to  the shopping list, so you want to fix it before you push it to your remote for your colleagues to see.

Rewrite the last commit to add the milk and update it's commit message to be better formatted, for example:

`[ABC-123] Add milk and eggs to shopping list`

1. Use your favourite text editor to add "milk" to the shopping list.
2. Use `git add -p` to add the change to the stage.
3. Use `git commit --amend` to amend the previous commit rather than create a new commit.
4. Change the commit message in the editor
5. Check your changes by using `git l`.

```
* cd4b226 - (HEAD -> master) [ABC-123] Add eggs and milk (5 minutes ago) <Paul James>
* 71bc934 - Add stroopwafels (40 minutes ago) <Paul James>
* 7a090d3 - [ABC-123] Add cheese (62 seconds ago) <Paul James>
* efbb44b - [ABC-123] Create shopping list (40 minutes ago) <Paul James>
```


#### Exercise 2: Rewrite a previous commit message

The commit before the last commit also had a mistake, you need to fix the commit message because you forgot to add the ticket number to the beginning:

`[ABC-123] Add stroopwafels`

But you can't use amend as that only edits the last commit, instead you will need to use the interactive rebase:

1. Use `git rebase -i HEAD~2` to start a rebase 2 commits into the past.
2. Find the commit in the list and mark it for rewording (`reword` or `r`)
3. Exit the editor to continue the rebasing process.
4. Change the commit message in the editor and exit again.
5. Check your changes by using `git l`.

If you run into trouble while rebasing, you can use `git rebase --abort` to exit the rebasing process and go back to how things were.

```
* cd4b226 - (HEAD -> master) [ABC-123] Add eggs and milk (8 minutes ago) <Paul James>
* 71bc934 - [ABC-123] Add stroopwafels (43 minutes ago) <Paul James>
* 7a090d3 - [ABC-123] Add cheese (62 seconds ago) <Paul James>
* efbb44b - [ABC-123] Create shopping list (43 minutes ago) <Paul James>
```


#### Exercise 3: Reorder commits

Actually, milk and eggs should have been added to the list before stroopwafels, you don't want people to think you value stroopwafels above mike and eggs. Better reorder those commits.

1. Use `git rebase -i HEAD~2` to start a rebase 2 commits into the past.
2. Switch the order of the two commits in the rebase list (vim command: `ddp`).
3. Exit the editor to continue the rebasing process.
4. Check your changes by using `git l`.

```
* 83c4a1c - (HEAD -> master) [ABC-123] Add stroopwafels (2 seconds ago) <Paul James>
* 5d2f073 - [ABC-123] Add eggs and milk (2 seconds ago) <Paul James>
* 7a090d3 - [ABC-123] Add cheese (62 seconds ago) <Paul James>
* efbb44b - [ABC-123] Create shopping list (60 minutes ago) <Paul James>
```


#### Exercise 4: Change a previous commit

We had added "Cheese" to the list but we hadn't decided which type of cheese, but now we know, so we want to go back and change that entry. Rewrite the "Add cheese" commit to include the type of cheese.

There are two ways of doing this, you can either:

##### Create a new commit and then fix it up into the old commit in the past

Use this method if you have already made your amends in your working copy and now need to squash them into your git history.

1. Create a new commit containing the change.
2. Use `git rebase -i HEAD~4` to start a rebase 3 commits into the past.
3. Move the new commit in the list to be on the line below the cheese commit.
4. Change the command in front of the new commit from `pick` to `fixup` or just `f`.
5. Exit the editor to continue the rebasing process.
6. Check your changes by using `git l`.

This method is especially useful if you have many small changes, you can create many fixup commits from the changes and then rebase them all into your git history in one go.

##### Do the edit as part of the interactive rebase

Use this method if you have a quick small amend.

1. Use `git rebase -i HEAD~3` to start a rebase 3 commits into the past.
2. Change the command in front of the cheese commit from `pick` to `edit` or just `e`.
3. Exit the editor to continue the rebasing process.
4. Make your change to the shopping list
5. Add the change to the stage `git add shopping.list`
6. Continue the rebase with `git rebase --continue`
7. Check your changes by using `git l`.

One trick is to stash your uncommitted amends that you want to apply, then start the rebase and unstash them during the edit phase of the rebase.

```
* 83c4a1c - (HEAD -> master) [ABC-123] Add stroopwafels (2 seconds ago) <Paul James>
* 5d2f073 - [ABC-123] Add eggs and milk (2 seconds ago) <Paul James>
* 7a090d3 - [ABC-123] Add cheese (62 seconds ago) <Paul James>
* efbb44b - [ABC-123] Create shopping list (60 minutes ago) <Paul James>
```


#### Exercise 5: Break a commit into two separate commits

You've now decided that the eggs and milk commit contains items that are unrelated, so you want to split it up into 2 separate commits. Use rebase to split this commit in two.

1. Use `git rebase -i HEAD~2` to start a rebase 2 commits into the past.
2. Change the command in front of the eggs and milk commit from "pick" to "edit".
3. Exit the editor to continue the rebasing process.
4. Remove the old commit by resetting your current location `git reset HEAD~1`
5. Add just the eggs change to the stage `git add -p` and commit `git commit`
6. Then add the milk change to the stage `git add -p` and commit `git commit`
7. Continue the rebase with `git rebase --continue`
8. Check your changes by using `git l`.

```
* 08ec64a - (HEAD -> master) [ABC-123] Add stroopwafels (2 seconds ago) <Paul James>
* da27175 - [ABC-123] Add milk (9 seconds ago) <Paul James>
* 9c948ff - [ABC-123] Add eggs (23 seconds ago) <Paul James>
* 7a090d3 - [ABC-123] Add cheese (81 minutes ago) <Paul James>
* a33f912 - [ABC-123] Create shopping list (81 minutes ago) <Paul James>
```


#### Exercise 6: Squash commits together

After reviewing the commits, you have decided that milk and cheese are the practically the same thing, so you want to squash those two commits together.

1. Use `git rebase -i HEAD~4` to start a rebase 4 commits into the past.
2. Move the line for the milk commit to be below the cheese commit and change it's command to "squash"
3. Exit the editor to continue the rebasing process.
4. Edit the commit message to be "[ABC-123] Add dairy products"
5. Check your changes by using `git l`.

You will most likely get a conflict when you try to do this rebase. This is because the commits you are squashing both edit the same lines of the file. Git will drop you to the console where you must resolve the conflict in your editor, then stage and continue the rebase.

1. Fix conflict in editor.
2. Add changes to the stage `git add shopping.list`
3. Continue the rebasing `git rebase --continue`

```
* ebd81f6 - (HEAD -> master) [ABC-123] Add stroopwafels (8 seconds ago) <Paul James>
* 4337fc3 - [ABC-123] Add eggs (16 seconds ago) <Paul James>
* 7262431 - [ABC-123] Add dairy products (70 seconds ago) <Paul James>
* a33f912 - [ABC-123] Create shopping list (3 hours ago) <Paul James>
```
