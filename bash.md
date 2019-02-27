# Bash

### envsubst

Use as template

```
# file
--------------------
It's a path: $PATH
--------------------

envsubst '$PATH' < file > new_file

# new_file
----------
It's a path: /path
----------
```