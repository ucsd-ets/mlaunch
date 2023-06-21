
# mlaunch: launch parallel jobs on Datahub/DSMLP

### Description:

'mlaunch' converts a standalone Datahub/DSMLP pod/container into a Job
comprising identical instances running in parallel.

When run within the cluster, 'mlaunch' derives the pod configuration
(container image, cpu/memory resources, filesystem mounts) from the currently-running pod.    

Command and arguments supplied to 'mlaunch' replace those from the template.

Total number of executions may be specified with the "-n <NUM>" option,
and by default all will be executed simultaneously if resources permit.
A maximum number of parallel instances may be set using the "-p NUM" option.

Outside of the cluster (e.g. on the login node), any of the
'launch-...' scripts may be used to generate a template using the "-d" (dump) option,
with that template then piped into 'mlaunch -s', e.g.: `launch-scipy-ml.sh -d -c 1 -m 1 | mlaunch -s -n 10 ./myjob`

After launch, standard tools may be used to manage the job or its component pods, e.g.

```
View logs:             kubectl logs --prefix --timestamps -l job-name=job-13517
List pods/containers:  kubectl get pods -l job-name=job-13517
Shell into pod:        kubesh job-13517-1-abcde
Terminate job:         kubectl delete job job-name=job-13517
```

By default, job & output 

### Usage:

```
mlaunch [<args>] [--] <container command> [args]

Command line options:
         -s                   accept pod template on standard input (default: duplicate host pod configuration)
         -t <num>             preserve job logs/pods for <num> seconds (default: 10800)
         -N <name>            specify job name (default: auto-generated)
         -n <num>             number of iterations to be executed (default: 1)
         -p <num>             number of parallel instances (default: run all iterations simultaneously)
         -d                   dump kubernetes 'job' object to stdout
         -h                   Display usage instructions
         --                   End 'mlaunch' argument processing and pass remainder to container
```
