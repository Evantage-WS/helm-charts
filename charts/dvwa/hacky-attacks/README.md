# hacky-attacks

## Init
Create a namespace - darkforce, that is where all of the attacking pods will live.

## Deploy
1. Deploy one of the attacks, watch logs coming from victim pod

## Cleanup
To delete jobs afterwards - for job in $(kubectl get jobs -n darkforce | awk '{print $1}' | sed '1d'); do kubectl delete job $job -n darkforce; done