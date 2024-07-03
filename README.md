<div align="center">

   # git amend
</div>

The `git commit --amend` command in Git is used to modify the most recent commit. This is particularly useful when you want to:

1. Change the commit message of the latest commit.
2. Add or remove files from the latest commit.
3. Combine staged changes with the previous commit.

### How to Use `git commit --amend`

1. **Changing the Commit Message**:
   If you just want to change the message of the latest commit, you can use:
   ```sh
   git commit --amend
   ```
   This will open your default text editor where you can edit the commit message.

2. **Adding More Changes to the Last Commit**:
   If you want to add more changes to the previous commit, first stage the changes:
   ```sh
   git add <file>
   ```
   Then amend the commit:
   ```sh
   git commit --amend --no-edit
   ```
   The `--no-edit` option keeps the existing commit message.

3. **Removing Files from the Last Commit**:
   If you want to remove files from the previous commit, you can:
   ```sh
   git reset HEAD <file>   # Unstage the file
   git commit --amend --no-edit
   ```

### Points to Consider

- **Amending Commits in Shared Branches**: If the commit you are amending has already been pushed to a shared branch, you need to be cautious. Amending changes the commit hash, which can cause issues for others who have pulled the previous version.
  
- **History Rewriting**: `git commit --amend` rewrites history, which is generally safe in your local repository but should be handled with care in shared repositories.

Using `git commit --amend` effectively can help keep your commit history clean and concise, making it easier to manage and understand.


Sure, let's go through an example involving a local and remote repository.

### Scenario 2 with amend example

You have a local Git repository with some commits, and you have pushed those commits to a remote repository (e.g., on GitHub). Now, you realize you need to amend the most recent commit.

### Step-by-Step Example

1. **Initialize a Local Repository**:
   ```sh
   git init my-project
   cd my-project
   ```

2. **Create a File and Make a Commit**:
   ```sh
   echo "Initial content" > file.txt
   git add file.txt
   git commit -m "Initial commit"
   ```

3. **Add a Remote Repository** (assuming you've already created it on GitHub):
   ```sh
   git remote add origin https://github.com/yourusername/my-project.git
   ```

4. **Push to the Remote Repository**:
   ```sh
   git push -u origin master
   ```

5. **Amend the Last Commit**:
   Let's say you want to add another file to the last commit.
   ```sh
   echo "Some more content" > another-file.txt
   git add another-file.txt
   git commit --amend -m "Initial commit with additional file"
   ```

6. **Force Push the Amended Commit to the Remote Repository**:
   Since the commit history has changed (the commit hash is different), you need to force push the changes:
   ```sh
   git push --force origin master
   ```

### Detailed Explanation

1. **Initialize a Local Repository**: You start by creating a new directory for your project, initializing a Git repository, and navigating into it.

2. **Create a File and Make a Commit**: You create a file, add it to the staging area, and commit it to the repository.

3. **Add a Remote Repository**: You link your local repository to a remote repository on GitHub (or any other Git hosting service).

4. **Push to the Remote Repository**: You push the initial commit to the remote repository. The `-u` flag sets the upstream tracking for the `master` branch.

5. **Amend the Last Commit**: You create another file, add it to the staging area, and amend the last commit to include this new file. You also change the commit message.

6. **Force Push the Amended Commit**: Because the commit history is rewritten, you need to force push the changes to the remote repository. This overwrites the remote commit with your amended commit.

### Important Notes

- **Force Push Caution**: Use `--force` (or `-f`) with caution, especially on shared branches. It can overwrite commits that others might have based their work on.
- **Collaborative Workflow**: If working with others, communicate changes to avoid conflicts. Alternatively, use `--force-with-lease` to ensure you don’t overwrite any commits that were pushed by others since your last fetch.

Using `git commit --amend` and `git push --force` allows you to correct and refine your commit history, but it must be used responsibly to avoid disrupting the workflow of others.


### Example
   ```sh
   -- The `git commit --amend` command in Git is used to modify the most recent commit. This is particularly useful when you want to:
      - Change the commit message of the latest commit.
      - Add or remove files from the latest commit.
      - Combine staged changes with the previous commit.
      
     -- git commit --amend  //This will open your default text editor where you can edit the commit message.
        --

     
     -- git commit --amend -m "Initial commit with additional file"   //with -m flag
        -- git push --force origin master   
            //Since the commit history has changed (the commit hash is different), you need to force push the changes:
     
     -- Using `git commit --amend` and `git push --force` allows you to correct and refine your commit history, but it must be used responsibly to avoid disrupting the workflow of others.
     
     -- git commit --amend --no-edit    //The --no-edit option keeps the existing commit message.
   ```



<br>
<br>
<br>

<div align="center">

   # git rebase
</div>

### What is Git Rebase Interactive?

`git rebase -i` (interactive rebase) is a powerful Git command that allows you to rewrite and manipulate your commit history. It lets you modify individual commits, combine them, reorder them, or even remove them. This is particularly useful for cleaning up a series of commits before merging them into a main branch.

### When to Use Interactive Rebase

- **Squashing Commits (squash)**: Combine multiple commits into one.
- **Reordering Commits**: Change the order of your commits.
- **Editing Commit Messages (reword)**: Modify the messages of previous commits. //edit an old commit message
- **Removing Commits (drop)**: Delete specific commits from the history.
- **Splitting Commits**: Divide a single commit into multiple smaller commits.

### Sample command which are frequently used in Rebase
```sh
git rebase -i 6fabd0d
git rebase --continue
git rebase --abort
git rebase --edit-todo
git commit --amend
git commit --amend --edit-todo
```

<br>

# Scenario and Example With `reword` commit messages.

You have a branch with a few commits, and you want to change the commit messages of these commits.
One of the very popular use cases of interactive rebase is that you can edit an old commit message after the fact. You might be aware that `git commit --amend` also allows you to change a commit’s message — but only if it’s the very latest commit. For any commit older than that, we have to use interactive rebase!

### Step-by-Step Example

1. **Initial Setup**: You have a branch `feature` with three commits.

2. **Check Your Branch and Commit History**:
   ```sh
   git checkout feature
   git log --oneline
   ```
   Example output:
   ```
   a1b2c3d Third commit
   d4e5f6g Second commit
   h1i2j3k First commit
   ```

3. **Start Interactive Rebase**:
   ```sh
   git rebase -i HEAD~3
   ```
   This command opens an editor with the last three commits listed.

4. **Interactive Rebase Editor**:
   ```
   pick h1i2j3k First commit
   pick d4e5f6g Second commit
   pick a1b2c3d Third commit
   ```
   Change `pick` to `reword` (or `r`) for the commits you want to change the messages of:
   ```
   pick h1i2j3k First commit
   reword d4e5f6g Second commit
   reword a1b2c3d Third commit
   ```

5. **Save and Close the Editor**:
   After saving and closing (press `esc`, `:wq` //or `esc`, `shift + zz`), Git will open an editor for each commit you marked as `reword`.

6. **Edit the Commit Messages**:
   For the second commit:
   ```
   Second commit
   ```
   Change to:
   ```
   Improved the functionality
   ```
   Save and close the editor.

   For the third commit:
   ```
   Third commit
   ```
   Change to:
   ```
   Added unit tests
   ```
   Save and close the editor.

7. **Finish the Rebase**:
   Git will apply the changes. If there are conflicts, resolve them as prompted, and continue the rebase:
   ```sh
   git rebase --continue
   ```

8. **Push the Changes**:
   If the branch has already been pushed to a remote repository, force push the changes:
   ```sh
   git push --force origin feature
   ```

### Detailed Explanation

1. **Check Your Branch and Commit History**: Ensure you are on the correct branch and view the commit history.

2. **Start Interactive Rebase**: The `HEAD~3` argument indicates you want to rebase the last three commits. This opens an editor with a list of commits.

3. **Interactive Rebase Editor**: Change `pick` to `reword` for the commits you want to change the messages of. The commits you leave as `pick` will remain unchanged.

4. **Save and Close the Editor**: Git processes the instructions and opens an editor for each `reword` commit.

5. **Edit the Commit Messages**: Change the commit messages as desired, then save and close the editor for each commit.

6. **Finish the Rebase**: If there are no conflicts, the rebase completes. Otherwise, resolve conflicts and continue.

7. **Push the Changes**: Use `--force` to push the rewritten commit history to the remote repository.

Using `git rebase -i` to reword commits allows you to maintain a clean and meaningful commit history, making your project easier to manage and collaborate on.

<br>
<br>



# Scenario and Example With `drop`

You have a branch with several commits, and you want to remove (drop) a specific commit from the history.


Deleting an Unwanted Commit:

Interactive rebase also allows you to delete an old commit from your history that you don’t need (or want) anymore. Just imagine you have accidentally included a personal password in a recent commit: sensitive information like this should not, in most cases, be included in a codebase.

Also remember that simply deleting the information and committing again doesn’t really solve your problem: this would mean the password is still saved in the repository, in the form of your old commit. What you really want is to cleanly and completely delete this piece of data from the repository altogether!

`git rebase -i <6bcf266b commit hash which have to be deleted>`

This time, we’re using the drop action keyword to get rid of the unwanted commit. Alternatively, in this special case, we could also simply delete the whole line from the editor. If a line (representing a commit) is not present anymore when saving and closing the window, Git will delete the respective commit.

However you choose to do it, after you’ve saved and closed the editor window, the commit will be deleted from your repository’s history!


### Step-by-Step Example

1. **Initial Setup**: You have a branch `feature` with four commits.

2. **Check Your Branch and Commit History**:
   ```sh
   git checkout feature
   git log --oneline
   ```
   Example output:
   ```
   a1b2c3d Fourth commit
   d4e5f6g Third commit
   h1i2j3k Second commit
   l4m5n6o First commit
   ```

3. **Start Interactive Rebase**:
   ```sh
   git rebase -i HEAD~4
   ```
   This command opens an editor with the last four commits listed.

4. **Interactive Rebase Editor**:
   ```
   pick l4m5n6o First commit
   pick h1i2j3k Second commit
   pick d4e5f6g Third commit
   pick a1b2c3d Fourth commit
   ```
   Change `pick` to `drop` (or `d`) for the commit you want to remove. For example, to drop the second commit:
   ```
   pick l4m5n6o First commit
   drop h1i2j3k Second commit
   pick d4e5f6g Third commit
   pick a1b2c3d Fourth commit
   ```

5. **Save and Close the Editor**:
   After saving and closing (press `esc`, `:wq` //or `esc`, `shift + zz`), Git will reapply the commits, skipping the one marked as `drop`.

6. **Finish the Rebase**:
   Git will rewrite the history, excluding the dropped commit. If there are conflicts, resolve them as prompted, and continue the rebase:
   ```sh
   git rebase --continue
   ```

7. **Push the Changes**:
   If the branch has already been pushed to a remote repository, force push the changes:
   ```sh
   git push --force origin feature
   ```

### Detailed Explanation

1. **Check Your Branch and Commit History**: Ensure you are on the correct branch and view the commit history.

2. **Start Interactive Rebase**: The `HEAD~4` argument indicates you want to rebase the last four commits. This opens an editor with a list of commits.

3. **Interactive Rebase Editor**: Change `pick` to `drop` for the commits you want to remove. The commits marked as `drop` will be excluded from the history.

4. **Save and Close the Editor**: Git processes the instructions and replays the remaining commits, skipping the dropped ones.

5. **Finish the Rebase**: If there are no conflicts, the rebase completes. Otherwise, resolve conflicts and continue.

6. **Push the Changes**: Use `--force` to push the rewritten commit history to the remote repository.

Using `git rebase -i` to drop commits allows you to maintain a clean and meaningful commit history, making your project easier to manage and collaborate on.


<br>
<br>
<br>



# Scenario Example With `squash`

Suppose you have a feature branch with several commits that you want to clean up before merging into the main branch.
squash/combine multiple commits.

1. **Initial Setup**: You have a branch `feature` with three commits.
2. **Interactive Rebase**: You want to squash the second and third commits into the first commit and edit their messages.

### Step-by-Step Example

1. **Check Your Branch and Commit History**:
   ```sh
   git checkout feature
   git log --oneline
   ```
   Example output:
   ```
   a1b2c3d Third commit
   d4e5f6g Second commit
   h1i2j3k First commit
   ```

2. **Start Interactive Rebase**:
   ```sh
   git rebase -i HEAD~3
   ```
   This command opens an editor with the last three commits listed.

3. **Interactive Rebase Editor**:
   ```
   pick h1i2j3k First commit
   pick d4e5f6g Second commit
   pick a1b2c3d Third commit
   ```
   Edit the lines to change `pick` to `squash` (or `s`) for the commits you want to combine:
   ```
   pick h1i2j3k First commit
   squash d4e5f6g Second commit
   squash a1b2c3d Third commit
   ```

4. **Save and Close the Editor**:
   After saving and closing (press `esc`, `:wq` //or `esc`, `shift + zz`), another editor will open, allowing you to edit the commit message for the squashed commits.

5. **Edit the Commit Message**:
   Combine the commit messages into a single, cohesive message:
   ```
   First commit
   - Second commit
   - Third commit
   ```
   Save and close the editor.

6. **Finish the Rebase**:
   Git will rewrite the history and squash the commits. If there are conflicts, resolve them as prompted, and continue the rebase:
   ```sh
   git rebase --continue
   ```

7. **Push the Changes**:
   If the branch has already been pushed to a remote repository, force push the changes:
   ```sh
   git push --force origin feature
   ```

### Detailed Explanation

1. **Check Your Branch and Commit History**: Ensure you are on the correct branch and view the commit history.

2. **Start Interactive Rebase**: The `HEAD~3` argument indicates you want to rebase the last three commits. This opens an editor with a list of commits.

3. **Interactive Rebase Editor**: Change `pick` to `squash` for the commits you want to combine. The first commit is left as `pick`.

4. **Save and Close the Editor**: Git processes the instructions and opens another editor to combine commit messages.

5. **Edit the Commit Message**: Combine the messages in a meaningful way, then save and close the editor.

6. **Finish the Rebase**: If there are no conflicts, the rebase completes. Otherwise, resolve conflicts and continue.

7. **Push the Changes**: Use `--force` to push the rewritten commit history to the remote repository.

Interactive rebase allows you to maintain a clean, understandable commit history, making your project easier to manage and collaborate on.

<br>
Sure! Let's go through the process of squashing the commits "br 2 added" and "br 1 added" into one commit using an interactive rebase.

### Step-by-Step Example of `squash` real life

1. **Check Your Branch and Commit History**:
   ```sh
   git log --oneline
   ```
   Example output:
   ```
   6f07c31 (HEAD -> feature) br 3 added
   34518cb br 2 added
   b041811 br 1 added
   21cd881 drop added in readme
   e8d0182 p added after br
   d45d1d0 rebase by squash 1-2-3 commit
   d3c53ba commit 2
   28229e4 first commit
   ```

2. **Start Interactive Rebase**:
   You want to rebase from the commit before "br 1 added" (which is "21cd881 drop added in readme"), so you'll start the rebase from `HEAD~4`:
   ```sh
   git rebase -i HEAD~4
   ```
   This command opens an editor with the last four commits listed.

3. **Interactive Rebase Editor**:
   The editor will show something like this:
   ```
   pick 21cd881 drop added in readme
   pick b041811 br 1 added
   pick 34518cb br 2 added
   pick 6f07c31 br 3 added
   ```
   Change `pick` to `squash` (or `s`) for the commit you want to squash into the one above it. For example, to squash "br 2 added" into "br 1 added":
   ```
   pick 21cd881 drop added in readme
   pick b041811 br 1 added
   squash 34518cb br 2 added
   pick 6f07c31 br 3 added
   ```
   Change `pick` to `squash` for the commit "br 2 added". This will combine it into the "br 1 added" commit above it.

4. **Save and Close the Editor**:
   After saving and closing (press `esc`, `:wq` //or `esc`, `shift + zz`), another editor will open, allowing you to edit the combined commit message.

5. **Edit the Combined Commit Message**:
   Combine the commit messages into a single, cohesive message. For example:
   ```
   br 1 and br 2 added
   - br 1 added
   - br 2 added
   ```
   Save and close the editor.

6. **Finish the Rebase**:
   Git will rewrite the history and squash the commits. If there are conflicts, resolve them as prompted, and continue the rebase:
   ```sh
   git rebase --continue
   ```

7. **Push the Changes**:
   If the branch has already been pushed to a remote repository, force push the changes:
   ```sh
   git push --force origin feature
   ```

### Detailed Explanation

1. **Check Your Branch and Commit History**: Ensure you are on the correct branch and view the commit history.

2. **Start Interactive Rebase**: The `HEAD~4` argument indicates you want to rebase the last four commits. This opens an editor with a list of commits.

3. **Interactive Rebase Editor**: Change `pick` to `squash` for the commit "br 2 added". This will combine it into the "br 1 added" commit above it.

4. **Save and Close the Editor**: Git processes the instructions and opens another editor to combine commit messages.

5. **Edit the Combined Commit Message**: Change the messages in a meaningful way, then save and close the editor.

6. **Finish the Rebase**: If there are no conflicts, the rebase completes. Otherwise, resolve conflicts and continue.

7. **Push the Changes**: Use `--force` to push the rewritten commit history to the remote repository.

By following these steps, you'll have successfully squashed the "br 2 added" and "br 1 added" commits into a single commit.

<br>

Let's go through the process of squashing the three specific commits: "21cd881 drop added in readme," "b041811 br 1 added," and "34518cb br 2 added" into one commit.

### Step-by-Step Example of another `squash` real life problem

1. **Check Your Branch and Commit History**:
   ```sh
   git log --oneline
   ```
   Example output:
   ```
   6f07c31 (HEAD -> feature) br 3 added
   34518cb br 2 added
   b041811 br 1 added
   21cd881 drop added in readme
   e8d0182 p added after br
   d3c53ba commit 2
   28229e4 first commit
   ```

2. **Start Interactive Rebase**:
   You want to rebase from the commit before "21cd881 drop added in readme" (which is "e8d0182 p added after br"), so you'll start the rebase from `HEAD~4`:
   ```sh
   git rebase -i HEAD~5
   ```
   This command opens an editor with the last four commits listed.

3. **Interactive Rebase Editor**:
   The editor will show something like this:
   ```
   pick e8d0182 p added after br
   pick 21cd881 drop added in readme
   pick b041811 br 1 added
   pick 34518cb br 2 added
   pick 6f07c31 br 3 added
   ```
   Change `pick` to `squash` (or `s`) for the commits you want to squash into the commit above them. To squash "drop added in readme," "br 1 added," and "br 2 added" into one:
   ```
   pick e8d0182 p added after br
   pick 21cd881 drop added in readme
   squash b041811 br 1 added
   squash 34518cb br 2 added
   pick 6f07c31 br 3 added
   ```

4. **Save and Close the Editor**:
   After saving and closing, another editor will open, allowing you to edit the combined commit message.

5. **Edit the Combined Commit Message**:
   Combine the commit messages into a single, cohesive message. For example:
   ```
   Combined changes: drop added in readme, br 1 added, br 2 added
   - drop added in readme
   - br 1 added
   - br 2 added
   ```
   Save and close the editor.

6. **Finish the Rebase**:
   Git will rewrite the history and squash the commits. If there are conflicts, resolve them as prompted, and continue the rebase:
   ```sh
   git rebase --continue
   ```

7. **Push the Changes**:
   If the branch has already been pushed to a remote repository, force push the changes:
   ```sh
   git push --force origin feature
   ```

### Detailed Explanation

1. **Check Your Branch and Commit History**: Ensure you are on the correct branch and view the commit history.

2. **Start Interactive Rebase**: The `HEAD~4` argument indicates you want to rebase the last four commits. This opens an editor with a list of commits.

3. **Interactive Rebase Editor**: Change `pick` to `squash` for the commits "br 1 added" and "br 2 added". This will combine them into the "drop added in readme" commit above them.

6. **Finish the Rebase**: If there are no conflicts, the rebase completes. Otherwise, resolve conflicts and continue.

7. **Push the Changes**: Use `--force` to push the rewritten commit history to the remote repository.

By following these steps, you'll have successfully squashed the "drop added in readme," "br 1 added," and "br 2 added" commits into a single commit.


<br>
<br>
<br>


# Scenario and Example With `edit`

Certainly! Let's explore how to use `git rebase -i` with the `edit` command. Interactive rebase (`git rebase -i`) is a powerful tool for rewriting commit history in Git. The `edit` command within an interactive rebase allows you to stop at a particular commit, modify it, and then continue the rebase process. This is useful for making changes to a specific commit after it has been created.

### Step-by-Step Example

Let's say you have the following commit history:

```sh
git log --oneline
```
```
e3a1f50 (HEAD -> feature) Commit 4
d2b3c9a Commit 3
c1d2e3f Commit 2
b1a2b3c Commit 1
```

You want to edit "Commit 2" to change its content.

#### Step 1: Start Interactive Rebase

Start an interactive rebase, specifying the parent of the commit you want to edit. In this case, you want to edit the third commit, so you will start the rebase from `HEAD~3`.

```sh
git rebase -i HEAD~3
```

#### Step 2: Modify the Interactive Rebase Editor

This command will open an editor with a list of commits:

```
pick b1a2b3c Commit 1
pick c1d2e3f Commit 2
pick d2b3c9a Commit 3
pick e3a1f50 Commit 4
```

Change `pick` to `edit` (or `e`) for the commit you want to modify:

```
pick b1a2b3c Commit 1
edit c1d2e3f Commit 2
pick d2b3c9a Commit 3
pick e3a1f50 Commit 4
```

Save and close the editor.

#### Step 3: Git Stops at the Specified Commit

Git will stop at "Commit 2," allowing you to make changes:

```
Stopped at c1d2e3f... Commit 2
You can amend the commit now, with:

    git commit --amend

Once you're satisfied with your changes, run:

    git rebase --continue
```

#### Step 4: Make Your Changes

Make any necessary changes to your working directory. For example, let's say you want to modify a file:

```sh
echo "Additional content" >> file.txt
```

#### Step 5: Amend the Commit

Stage the changes and amend the commit:

```sh
git add file.txt
git commit --amend
```

This will open an editor to modify the commit message if needed. After saving and closing the editor, the changes are now part of "Commit 2."

#### Step 6: Continue the Rebase

Continue the rebase process:

```sh
git rebase --continue
```

Git will reapply the remaining commits on top of your amended commit. If there are any conflicts, resolve them as prompted and continue the rebase:

```sh
git rebase --continue
```

#### Step 7: Push the Changes

If the branch has already been pushed to a remote repository, force push the changes:

```sh
git push --force origin feature
```

### Detailed Explanation

1. **Start Interactive Rebase**: Begin an interactive rebase with `HEAD~3` to include the commits up to and including "Commit 2."
2. **Modify the Interactive Rebase Editor**: Change `pick` to `edit` for the commit you want to modify.
3. **Git Stops at the Specified Commit**: Git pauses the rebase at "Commit 2," allowing you to make changes.
4. **Make Your Changes**: Modify the files as needed in your working directory.
5. **Amend the Commit**: Stage the changes and amend the commit using `git commit --amend`.
6. **Continue the Rebase**: Resume the rebase process with `git rebase --continue`, resolving any conflicts as they arise.
7. **Push the Changes**: Force push the updated commit history to the remote repository.

Using `git rebase -i` with the `edit` command allows you to modify specific commits in your history, providing flexibility in managing your project's commit history.

### Practice History of `edit`
```sh
  -- edit   //edit a specific commit/file
    -- git log --oneline
    -- git rebase -i 6fabd0d  //j commit edit korbo tar agher commit hash
    -- press `i`
    -- `edit` instead of `pick`  //for edit specific commit code/file //(in new window of editor)
    -- press `esc`, `:wq` //or `esc`, `shift + zz`
    -- then oi file e giye edit kore nibo code.
    -- git add .
    -- git status //checking edited/updated code tracked or not
    -- git commit --amend  // taile windown eshe commit messg ta edit korte bolbe.
    -- git rebase --continue
    -- git push --force origin feature
```


<br>
<br>