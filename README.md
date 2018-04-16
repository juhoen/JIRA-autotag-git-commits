# JIRA autotag Git commits
Adds JIRA ticket id automatically to each Git commit. After naming your branch according to corresponding JIRA ticket (e.g. features/ABC-123-example-branch) this hook automatically detects ticket id (ABC-123) and adds it automatically to each commit message (e.g. ABC-123 Initial commit). If the branch name doesn't contain ticket id (e.g. master), this hook doesn't modify original commit at all.

**Step 1**: Enable Git templates globally

```
git config --global init.templatedir "~/.git-templates"
```

**Step 2**: Create directory

```
mkdir -p ~/.git-templates/hooks
```

**Step 3**: Create file *commit-msg* with following content and save it inside *~/.git-templates/hooks/*

```
#!/bin/sh  
#
# Automatically adds branch tag to every commit message.
#

BRANCH=$(git rev-parse --abbrev-ref HEAD)
FILENAME=$(basename $BRANCH)

# This line copies tag from the branch name
TAG=$(echo "$FILENAME" | sed -n 's/\([A-Z]\{1,4\}-[0-9]\{1,6\}\).*/\1/p')

echo "$TAG"' '$(cat "$1") > "$1"
```

**Step 4**: Make sure commit-msg it executable.

```
chmod +x ~/.git-templates/hooks/commit-msg
```

**Step 5**: Initialize your existing Git repositories

```
git init
```
