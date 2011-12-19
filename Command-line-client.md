Command line client

This page describes all the commands that are possible with the "storm" command line client. These commands are:

1. jar
1. kill
1. localconfvalue
1. remoteconfvalue
1. repl
1. classpath
1. activate
1. deactivate
1. rebalance
1. nimbus
1. ui
1. drpc
1. supervisor

### jar

Syntax: storm jar `topology-jar-path` `class` `...`

Runs the main method of `class` with the specified arguments. The storm jars and configs in ~/.storm/ are put on the classpath. The process is configured so that [StormSubmitter](http://nathanmarz.github.com/storm/doc/backtype/storm/StormSubmitter.html) will upload the jar at `topology-jar-path` when the topology is submitted.

### kill

Syntax: storm kill `topology-name` \[-w `wait-time`\]

Kills the topology with the name `topology-name`. 