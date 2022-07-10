# Git

## wording

- [Good references](https://stackoverflow.com/a/3690796)
- [Usage of index explained](http://web.archive.org/web/20090210020404id_/http://whygitisbetterthanx.com/#the-staging-area)
- index = staging area
- working tree = working directory
- (branch) head: pointer to the newest commit on a branch
- tag: fixed pointer to commit
- HEAD: movable pointer which usually points to the head/tip of a branch. The next commit will take action on the commit which HEAD points to.
- reset (HEAD vs index vs working tree)
  - Reset Soft (HEAD only)
  - Mixed (Head and index)
  - Hard (Head, index, Working Tree)

## clear working directory (with untracked files)

With reset:

- To last commit including ignored files: `git reset --hard && git clean -dfx`
- To last commit: `git reset --hard && git clean -df`

With stash. Allows to revert changes, but not recommended with many files: 

- To last commit including ignored files: `git stash --all`
- To last commit: `git stash --include-untracked `
- Permanently deleting files: `git stash drop`
- Revert changes: `git stash pop`

## revert or delete commits

- [interactive wizard](https://sethrobertson.github.io/GitFixUm/fixup.html)

```shell
# rework last commit ()
git reset HEAD^

# delete last commit
git reset --hard HEAD^
```

## change old commit 

[Source](https://stackoverflow.com/a/2719636/6872190)

```shell
git rebase -i HEAD~10
# mark commit with 'e' or 'edit'
# change files and add to stage
git add <stuff>
git commit --amend --no-edit
git rebase --continue

```
## clean files in history

If you want to delete passwords or other sensible stuff from git history use [bfg](https://rtyley.github.io/bfg-repo-cleaner/). It does not touch your current commit (HEAD) just older commits.

Afterwards use force push to change history: `git push --force`

## git submodules

```shell
# init submodule
git submodule update --init --recursive

# point submodule to HEAD
git submodule update --remote --merge
```

## certs

- ignore:
  - via system env variable: `env GIT_SSL_NO_VERIFY=true`
  - or via cli: `git config --global http.sslVerify "false"`
- Windows:
  - `git config --global http.sslbackend channel`

## tag

```shell 
git tag -a 1.2.3 -m "message
git push --follow-tags

```

## password manager

```
# check existing methods
git help -a | grep credential

# there options might be available 
## credential           Retrieve and store user 
## credential-cache     Helper to temporarily store passwords in memory
## credential-store     Helper to store credentials on disk in plaintext (~/.git-credentials)

# credential-cache
git config --global credential.helper "cache --timeout 30000"

# credential-store 
git config --global credential.helper "store"

# you might need to install manager-core to use it
# and then set it via
# git config --global credential.helper "manager-core"
```

## ssh generate keys

```shell
ssh-keygen -t rsa -b 4096
```

## ssh remember passphrase

In bash:

```shell
echo 'eval `ssh-agent -s`' >> ~/.bashrc && source ~/.bashrc
ssh-add ~/.ssh/*_rsa
```

For WSL:

```shell
sudo apt install keychain
echo "eval `keychain --quiet --eval --agents ssh id_rsa`" >> ~/.bashrc  && source ~/.bashrc
```

## work with gpg

Sign commitsa

```bash
# for one commit
git co -S -m ""
# for all
git config --global commit.gpgsign true

# in vscode add to settings json:
"git.enableCommitSigning": true
```

create and configure gpg keys:
 
- [Gitlab doc](https://docs.gitlab.com/ee/user/project/repository/gpg_signed_commits/#add-a-gpg-key-to-your-account)

Save passphrase for twelve hours

```bash
echo "default-cache-ttl 43200" > ~/.gnupg/gpg-agent.conf
```
