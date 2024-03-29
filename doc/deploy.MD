# deploy

The *deploy* subcommand is meant for building plus deploying a [fsst build target](buildtarget.MD). You could consider *deploy* to be the combination of [artifact](artifact.MD) and [artifactdeploy](artifactdeploy.MD) roled into one, without using an intermidiate artifact tile. 

The deploy command has no positional arguments, only optional arguments.

The primary optional arguments are:

* --dir : The top level build directory of the [fsst build target](buildtarget.MD) tree.
* --target : The target as defined in build.json (found in the dir specified by --dir.
* --network : The network name for the database to deploy to.

Not that the full deployment database name is composed from the supplied network and the target name. If no network name is specified, a name will be generated by the tool.

The --dir argument defaults to the name "fluree_parts". --target defaults to the "default" target.


```bash
./fsst deploy --dir experiments --target user_ok
```

Like the [artifactdeploy](artifactdeploy.MD) and the [test](test.MD) subcommands, the deploy subcommand has the optional *--host* and *--port* arguments (defaulting to *localhost* and *8090*) that can be used to designate a FlureeDB host.

```bash
./fsst deploy --dir experiments --target user_ok --host otherhost --port 17080
```

Also like the [artifactdeploy](artifactdeploy.MD) and the [test](test.MD) subcommands, the *deploy* subcommand uses signed operations against the FlureeDb *command* API endpoint, and for this requires an ECDSA signing to perform its operations.  To provide this key, there are three optional arguments.

## --dockerfind <tag>

If you used the [dockerstart](dockerstart.MD) command to start a *fsst* FlureeDB docker container on your local system, you can use --dockerfind to specify that *deploy* should try and find the docker container and from this docker container, extract the signing key. A *tag* designates the FlureeDB version to use. You can download different tagged versions of the *fsst/FlureeDB* docker image from [dockerhub](https://hub.docker.com/r/pibara/fsst/tags?page=1&ordering=last_updated). 

If none of the tree key related arguments (--dockerfind, --createkey. --keyfile) is specified, --dockerfind with the tag *beta* is implied.

```bash
./fsst deploy --dir experiments --target user_ok --dockerfind fluree-snapshot-20201213
```

## --keyfile <file>

If instead of a docker container with FlureeDB, you are using a local install of FlureeDB, it can be usefull to designate the key file containing the default signing key with this option.

```
./fsst deploy --dir experiments --target user_ok --keyfile fluree-0.15.7/default-private-key.txt
```

## --createkey <key>

The third way to supply the command with a signing key is to supply the yey itself on the commandline using the --createkey option.

```bash
./fsst deploy --dir experiments --target user_ok --createkey d7ecd0776898eb4cb8c064fffa62d91988a2a2233ba74de8649349c246fb7996
```

Please note that on systems where *bitcoinlib* isn't available, an additional argument --createid will need to be supplied.
