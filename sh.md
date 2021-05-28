# Programming Notes and Tips for Shell Scripts in Bash

## Exit On Error

Recommend adding `set -euo pipefail` to the beginning of all Bash script files.

The `-e` and `-o pipefail` causes the script to exit with a non-zero return
code as soon as any child command returns a non-zero return code.

Note `-o pipefail` is really a compliment to `-e`, as it also catches failures
in part of a pipe, e.g. `true | false | true` will not trigger exit in `-e` only
but will do so if you add `-o pipefail`.

To catch a child command that may fail, you can do:

```sh
# Use an if-else-fi block
if command that may fail; then
    echo 'That's good'
else
    echo 'That's bad' >&2
fi

# Use an OR logic
rc=0
command that may fail || rc=$?
if ((rc == 0)); then
    echo 'That's good'
else
    echo 'That's bad' >&2
fi
```

The `-u` (error on referencing undefined variables) helps to catch typos.
However, if you need to source a third party script into the current environment,
you may encounter scripts that may not be compatible with `-u`.
In which case, you can use `set +u` to temporarily switch the option off
before turning it back on. E.g.:

```
# Sourcing an incompatible script
set +u  # turn off
. some-script
set -u  # turn it back on again
```

## Linter

Use a linter such as [shellcheck](https://www.shellcheck.net/) to ensure good
style.
