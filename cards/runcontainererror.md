# RunContainerError

* Symptom: STATUS shows RunContainerError, RESTARTS rising, READY stays 0/1
* Meaning: the image pulled fine, but the container runtime could not start the process at all. Different from CrashLoopBackOff, where the process starts and then exits
* Check: `k describe pod <pod_name>`, then read Last State (Reason: StartError) and the Message line
* Causes: start command not found (typo, or a multi-command string given without `sh -c`), wrong path to the program, missing executable in the image
* Fix: correct the command and recreate the deployment, using `sh -c '...'` for several commands or a single program such as `-- sleep 3600`, then `k get pods` to confirm it is Running

## Real output

Created a deployment with the command typed without `sh -c`, so Linux looked for a program literally named `echo booting; exit:1`. The image pulled, but the process could not start.

```
root@controlplane:~$ k create deployment crash --image=nginx --replicas=2 -- 'echo booting; exit:1'
deployment.apps/crash created
root@controlplane:~$ k get pods
NAME                     READY   STATUS              RESTARTS     AGE
crash-68c64456f9-gj7rr   0/1     RunContainerError   1 (3s ago)   4s
crash-68c64456f9-qhd96   0/1     RunContainerError   1 (2s ago)   4s
```

```
Last State:     Terminated
   Reason:       StartError
   Message:      ... exec: " echo booting; exit:1": executable file not found in $PATH
```
