# Git hooks

This repository contains some hooks for `git`.

# Git global hooks - git 2.9+

Pre-push script: [pre-push](./my-git-hooks/hooks/pre-push) - will warn if pushing to protected branch, script below:

```bash
#!/usr/bin/env bash
# Warn before pushing to protected branches
# Make script executable with chmod u+x pre-push
# Bypass with git push --no-verify
# Change default git hooks:
# git config --global core.hooksPath /path/to/my/hooks
BRANCH=$(git rev-parse --abbrev-ref HEAD)
PROTECTED_BRANCHES="^(master|dev|release-*|patch-*)"
if [[ "$BRANCH" =~ $PROTECTED_BRANCHES ]]; then
  read -p "[LOG] Are you sure you want to push to \"$BRANCH\" ? (y/n): " -n 1 -r < /dev/tty
  echo
  if [[ $REPLY =~ ^[Yy]$ ]]; then
    exit 0
  fi
  echo "[ERR] Push aborted."
  exit 1
fi
exit 0
```

Download and enable **global hooks**:

```
cd ~
mkdir -p ~/.my-git-hooks/hooks
wget 'https://raw.githubusercontent.com/kaczla/git-hooks/master/my-git-hooks/hooks/pre-push' -O ~/.my-git-hooks/hooks/pre-push
chmod u+x ~/.my-git-hooks/hooks/pre-push
git config --global core.hooksPath "$(realpath ~)/.my-git-hooks/hooks"
cd -
```

Now, **global hooks** should be enabled, to check run command:

```bash
$ git config --get core.hookspath
/home/MY_USER_NAME/.my-git-hooks/hooks
```
