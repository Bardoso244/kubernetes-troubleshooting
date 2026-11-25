# ImagePullBackOff

- **Symptom:** STATUS shows ErrImagePull / ImagePullBackOff, pod never actually starts
- **Meaning:** image could not be downloaded, Kubernetes retries several times, with longer waits each time
- **Check:** `k describe pod <pod_name>`, then read the Events at the bottom
- **Causes:** NotFound = bad name or tag, unauthorized = registry credentials, timeout = network or DNS
- **Fix:** correct the image using `k set image deployment/<name> <container>=<image>:<tag>`, then `k rollout status deployment/<name>`

## Real output

(paste a few lines from your own terminal here)
