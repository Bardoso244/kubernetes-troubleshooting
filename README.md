# Kubernetes troubleshooting

Learning how to administer and troubleshoot Kubernetes through hands-on practice, using Killercoda lab, all deployments I created, edited, broken and then fixed myself.

> Scope: lab-level Kubernetes (kubectl, pods, Services, logs). Using my network and cloud engineering background I followed the standard troubleshooting steps, find and isolate the issue, check logs, apply fix and verify.

## The loop I use for every fault

`get -> describe -> logs -> apply fix -> verify`

1. `kubectl get pods`: read STATUS, READY and RESTARTS
2. `kubectl describe pod <name>`: read the Events at the bottom
3. `kubectl logs <pod>`: add `--previous` after a crash when available
4. Fix the root cause
5. Verify that the fix has actually worked, check status via describe and logs

## Fault cards

| STATUS you see in `k get pods` | One line description | Link to the card |
|---|---|---|
| ImagePullBackOff | image could not be downloaded | [cards/imagepullbackoff.md](cards/imagepullbackoff.md) |
| CrashLoopBackOff | container starts, then its process exits | [cards/crashloopbackoff.md](cards/crashloopbackoff.md) |
| RunContainerError | image pulled, but the process could not start at all (bad command) | [cards/runcontainererror.md](cards/runcontainererror.md) |
| Pending | scheduler does not have a node with enough CPU or memory | [cards/pending.md](cards/pending.md) |

## How each card is laid out

Symptom / Meaning / Check / Causes / Fix / Real output from my terminal

## How I practise

- Break it on purpose, predict the output, then run it
- Repeat each drill from a blank terminal a few days later
- Notes written from memory first, then checked against the cluster

## Environment

Killercoda Kubernetes playground, kubectl with `alias k=kubectl`.
