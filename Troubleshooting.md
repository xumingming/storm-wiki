## Troubleshooting

This page lists issues people have run into when using Storm along with their solutions.

### Worker processes are crashing on startup with no stack trace

Possible symptoms:
 
 * Topologies work with one node, but workers crash with multiple nodes

Solutions:

 * You may have a misconfigured subnet, where nodes can't locate other nodes based on their hostname. ZeroMQ sometimes crashes the process when it can't resolve a host. There are two solutions:
  * Make a mapping from hostname to IP address in /etc/hosts
  * Set up an internal DNS so that nodes can locate each other based on hostname.
  
### Nodes are unable to communicate with each other

Possible symptoms:

 * Every spout tuple is failing
 * Processing is not working

Solutions:

 * Storm doesn't work with ipv6. You can force ipv4 by adding `-Djava.net.preferIPv4Stack=true` to the supervisor child options and restarting the supervisor. 
 * You may have a misconfigured subnet. See the solutions for `Worker processes are crashing on startup with no stack trace`

### Topology stops processing tuples after awhile

Symptoms:

 * Processing works fine for awhile, and then suddenly stops and spout tuples start failing en masse. 
 
Solutions:

 * This is a known issue with ZeroMQ 2.1.10. Downgrade to ZeroMQ 2.1.7.
 
### Not all supervisors appear in Storm UI

Symptoms:
 
 * Some supervisor processes are missing from the Storm UI
 * List of supervisors in Storm UI changes on refreshes

Solutions:

 * Make sure the supervisor local dirs are independent (e.g., not sharing a local dir over NFS)
 * Try deleting the local dirs for the supervisors and restarting the daemons. Supervisors create a unique id for themselves and store it locally. When that id is copied to other nodes, Storm gets confused. 

