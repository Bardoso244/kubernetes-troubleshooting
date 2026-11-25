# CrashLoopBackOff

* Symptom: STATUS shows CrashLoopBackOff (sometimes Error in between), RESTARTS keeps climbing
* Meaning: the image pulled fine and the container started, but its one process exited. Kubernetes restarts it with longer waits each time. The pod stays the same, only the container inside it restarts
* Check: `k logs deploy/<name>` (add `--previous` for the crashed run, it may be unavailable), then `k describe pod <pod_name>` and read the Last State block (Reason and Exit Code)
* Exit codes: 1 = the app hit an error and quit, 137 = the process was killed (often out of memory), 127 = command not found
* Causes: app crashes on startup (bad setting, missing dependency), or the start command ends straight away (a container only lives as long as its main process)
* Fix: correct the start command or the app problem, for example recreate with a command that keeps running: `k create deployment <name> --image=busybox:1.36 -- sleep 3600`, then `k get pods` to confirm it stays Running

## Real output

Created a deployment whose command exits immediately. The container started, then ended, and Kubernetes kept restarting it.

```
root@controlplane:~$ k create deployment crash --image=busybox:1.36 --replicas=2 -- sh -c 'echo booting; exit 1'
deployment.apps/crash created
root@controlplane:~$ k get pods
NAME                     READY   STATUS    RESTARTS     AGE
crash-79c997d9d6-6pn25   0/1     Error     1 (2s ago)   4s
crash-79c997d9d6-tkmhp   1/1     Running   1 (2s ago)   4s
root@controlplane:~$ k get pods
NAME                     READY   STATUS             RESTARTS     AGE
crash-79c997d9d6-6pn25   0/1     CrashLoopBackOff   1 (4s ago)   7s
crash-79c997d9d6-tkmhp   0/1     CrashLoopBackOff   1 (4s ago)   7s
```

```
    Last State:     Terminated
      Reason:       Error
      Exit Code:    1
```
