# Git

## VScode como editor padrão do git

```javascript 
git config --global core.editor "code --wait"
```

## Remove last commit

Be careful that this will create an "alternate reality" for people who have already fetch/pulled/cloned from the remote repository. But in fact, it's quite simple:

```bash
git reset HEAD^ # remove commit locally
git push origin +HEAD # force-push the new HEAD commit
```
