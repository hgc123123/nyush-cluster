# Custom Environment Modules

This document contains a few tips for helping you using environment modules more effectively.
As the general online documentation is lacking a bit, we also give the most popular commands here.

## How does it Work?

Environment modules are descriptions of software packages.
The `module` command is provided which allows the manipulation of environment variables such as `PATH`, `MANPATH`, etc., such that programs are available without passing the full path.
Environment modules also allow specifying dependencies between packages and conflicting packages (e.g., when the same binary is available in two packages).
Further, environment variables allow the parallel installation of different software versions in parallel and then using software "a la carte" in your projects.

## Popular Commands

### Querying

List currently loaded modules:

```terminal
$ module list
```

Show all available modules

```terminal
$ module avail
```

### Loading/Unloading Modules

Load one module, make sure to use a specific version to avoid ambiguities.

```terminal
$ module load cuda/12.1.0-gcc-8.5.0
```

Unload one module

```terminal
$ module unload cuda
```

Unload all modules

```terminal
$ module purge
```

### Getting Help

Get help for environment modules

```terminal
$ module help
```

Get help for a particular environment module

```terminal
$ module help cuda/12.1.0-gcc-8.5.0
```

### Using your own Module Files

You can also create your own environment modules.
Simply create a directory with module files and then use `module use` for using the modules from the directory tree.

```terminal
$ module use path/to/modules
```
