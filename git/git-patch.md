# Git Patch

## Creating Patches

```
$ git diff <commit_before_patch> > <patch_filename>.patch
```

```
$ git format-patch -1 <commit-hash>
```

## Applying Patches
```
$ git apply <patch_filename>.patch>
```