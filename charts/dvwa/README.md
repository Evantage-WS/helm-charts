# k8s-dvwa

## Init
1. `kubectl create ns dvwa`
2. `kubectl create secret generic -n dvwa mysql --from-literal=mysql-root-password=$(openssl rand -hex 20) --from-literal=mysql-replication-password=$(openssl rand -hex 20) --from-literal=mysql-password=$(openssl rand -hex 20)`


## Deploy
1. `helm install dvwa . -n dvwa -f values.yaml`

## Connect
1. `kubectl port-forward -n dvwa svc/dvwa 8000:80`
2. Open browser to `http://127.0.0.1:8000` and login
    Enter `admin` as username
    Enter `password` as password
3. Select "Create / Reset Database"
4. Re-login

## Attack
1. SQL injection: https://youtu.be/5bj1pFmyyBA?si=qeO6kTt5zRVnKEB5
2. MYSQDeny demo: https://holdmybeersecurity.com/2024/07/24/making-damn-vulnerable-web-application-dvwa-almost-unhackable-with-cilium-and-tetragon

## References
* [Define Environment Variables for a Container](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)
* [helm/bitnami/mysql](https://artifacthub.io/packages/helm/bitnami/mysql)
* [DVWA/compose.yml](https://github.com/digininja/DVWA/blob/123256873d226f8b71535366fd05807ab6ad1af4/compose.yml)


So there are multiple things I used in a demo for my prospect:
DVWA
and my own hacky crap :smile:

DVWA - see k8s-dvwa.zip, there is a values.yaml file that you would need to edit to pass the right ingress.
  - I deploy it like this - helm install dvwa . -n dvwa --create-namespace -f values.yaml
  - you can then follow instructions - https://youtu.be/5bj1pFmyyBA?si=qeO6kTt5zRVnKEB5 for SQL injections. I also use it for MYSQDeny demos
  https://holdmybeersecurity.com/2024/07/24/making-damn-vulnerable-web-application-dvwa-almost-unhackable-with-cilium-and-tetragon/?ref=dailydev

My own stuff:
  - deploy victim workloads - you will find all needed files in victim_workloads  directory
  - watch logs coming from victim pod, I usually open a terminals side-by-side and have k9s with logs of victim
  - create a namespace - darkforce, that is where all of the attacking pods will live. you will see a few other directories in the archive, each has a job that will do
    something, names are self-explanatory. Deploy those jobs, watch logs, watch NV's security events.
  - to delete jobs afterwards - for job in $(kubectl get jobs -n darkforce | awk '{print $1}' | sed '1d'); do kubectl delete job $job -n darkforce; done