# ImagePullBackOff

- **Symptom:** STATUS shows ErrImagePull / ImagePullBackOff, pod never actually starts
- **Meaning:** image could not be downloaded, Kubernetes retries several times, with longer waits each time
- **Check:** `k describe pod <pod_name>`, then read the Events at the bottom
- **Causes:** NotFound = bad name or tag, unauthorized = registry credentials, timeout = network or DNS
- **Fix:** correct the image using `k set image deployment/<name> <container>=<image>:<tag>`, then `k rollout status deployment/<name>`

## Real output

Attempted to create two pods with non-existing images, both failed

root@controlplane:~$ k get pods
NAME                            READY   STATUS             RESTARTS   AGE
 - image-test-1-59584bd5c8-8c4vj   0/1     ImagePullBackOff   0          5m6s
 - image-test-1-59584bd5c8-vvf7x   0/1     ImagePullBackOff   0          5m6s


