# Software Installation with Conda
## Conda
Users do not have the rights to install system packages on the NYUSH HPC cluster.
For the management of AI software we therefore recommend using the conda package manager.
Conda provides software in different “channels” and one of those channels contains a huge selection of software.
Generally packages are pre-compiled and conda just downloads the binaries from the conda servers.

You are in charge of managing your own software stack, but conda makes it easy
to do so. We will provide you with a description on how to use it. 
Of course there are many online resources that you can also use.
Please find a list at the end of the document.

Also note that some system-level software is managed through environment modules.

## Loading conda modules

```bash
module load miniconda3
```

Example1: To make bioinformatics software available, we have to add the `bioconda` and
some other channels to the conda configuration:

```bash
 $ conda config --add channels bioconda
 $ conda config --add channels default
 $ conda config --add channels conda-forge
```

## Installing software with conda
Installing packages with conda is straight forward:

```bash
 $ conda install <package>
```

This will install a package into the conda base environment. 
We will explain environments in detail in the next section.
To search for a package, e.g. to find the correct name in conda or if it exists
at all, issue the command:

```bash
 $ conda search <string>
```

To choose a specific version (conda will install the latest version that is
compatible with the current installed Python version), you can provide the
version as follows:

```bash
 $ conda install <package>=<version>
```

Please note that new conda installs may ship with a recently update Python version and not all packages might have been adapted.
E.g., if you find out that some packages don't work after starting out/upgrading to Python 3.8, simply try to downgrade Python to 3.7 with `conda install python=3.7`.

!!! hint
    As resolving the dependency tree of an installation candidate can take a lot of
    time in Conda, especially when you are installing software from an `environment.yaml`
    file, an alternative resolver has been presented that you can use to install
    software into your Conda environment. The time savings are immense and an
    installation that took more than an hour can be resolved in seconds.

    Simply run

    ```bash
     $ conda install mamba
    ```

    With that, you can install software into your environment using the same syntax as for Conda:

    ```bash
     $ mamba install <package>
    ```

## Creating an environment

Conda lets you create environments, such that you can test things in a different
environment or group your software. Another common use case is to have different
environments for the different Python versions. Since conda is Python-based,
conflicting packages will mostly struggle with the Python version.

By default, conda will install packages into its home environment. 

To create a Python 3.10 environment and activate it, issue the following commands:

```bash
 $ conda create -n py310 python=3.10
 $ source activate py310
(py310)  $
```

From now on, conda will install packages into the `py310` environment when you issue
the `install` command. To switch back to the home environment, simply deactivate the
`py310` environment:

```bash
(py310)  $ source deactivate py310
 $
```
