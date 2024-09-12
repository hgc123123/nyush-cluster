# Frequently Asked Questions

## Where can I get help?
- Talk to your colleagues!
- For problems while connecting and logging in, please contact [shanghai.it.help@nyu.edu](mailto:shanghai.it.help@nyu.edu).

## I cannot connect to the cluster. What's wrong?
Please see the section [Connection Problems](../connecting/connection-problems.md).

## Connecting to the cluster takes a long time.
The most probable cause for this is a conda installation which defaults to loading the _(Base)_ environment on login.
To disable this behaviour you can run:

```sh
$ conda config --set auto_activate_base false
```

