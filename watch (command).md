---
title: watch (command)
date: "2022-08-13 12:17:00"
tags:
  - linux
public: true
---

# watch (command)

The command runs other commands or jobs repeatedly in N time intervals

## Git: auto-commit all changes every 15 mins

```bash
watch -n 900 "git pull origin main && (git ls-files --modified --others --exclude-standard | grep . > /dev/null) && { git add . ; git commit -m 'auto-commit' ; git push origin main; }"
```


