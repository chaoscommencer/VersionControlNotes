
# AccuRev

Helpful tips and tricks for configuring and using AccuRev are provided below.

_NOTE: WIP._

## Configuration

Global configuration changes...

## Routine Usage

The following commands have more common usecases used outside of initial setup.

### Change Workspace

Change to the specified workspace

```shell
accurev chws -w <path> -l . -m <machine>
```

_NOTE: The \<path\> argument..._
_NOTE: The \<machine\> argument..._

#### Example

```shell
accurev chws -w target/workspace -l . -m my_machine
```

### Force Update

Force update current local repository with remote changes.

```shell
accurev pop -O -R .
```
