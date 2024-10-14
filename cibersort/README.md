# JupyterLab Job
The following guide will walk users through scheduling and connecting to a CIBERSORT job on the TIDE cluster.

This guide assumes that the user has completed the [Getting Access](https://sdsu-research-ci.github.io/softwarefactory/gettingaccess) directions and installed [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) on their local machine.

Please make sure to have a local copy of this repository. You can get this repo locally by cloning with git:
```
git clone https://github.com/SDSU-Research-CI/george-lab.git
```

Before running the examples below, please open this repository in your terminal and navigate to the cibersort directory:

```
cd cibersort
```

## Deploy

### Set Context

If you wish to not pass the namespace name for each command, run the following to set the namespace context:

```
kubectl config set-context nautilus --namespace=sdsu-george-lab
```

### Schedule the Job
Schedule the job with the following command:

```
kubectl apply -f cibersort-fractions.yaml -n sdsu-george-lab
```

## Accessing CIBERSORTxFRACTIONS

Get the pod for your job and ensure it is running:

```
kubectl get pods -n sdsu-george-lab --watch
```
- Your pod is ready when the READY columns shows '1/1' and STATUS column shows 'Running'
- You can stop the watch command with `ctrl + c`

Optionally, copy input files to the pod:
```
kubectl -n sdsu-george-lab cp [path/to/local/file] cibersort-pod:/src/data/[file]
```
- *Note*: Replace `path/to/local/file` with your path and `file` with the desired remote/pod filename, and remove both sets of `[]` brackets

Attach a remote terminal to the pod:
```
kubectl -n sdsu-george-lab exec -it cibersort-pod -- bash
```
- If you copied new files, first run `chown -R root:root /src/data/*` to fix file permissions

You can then run your code:
```
./CIBERSORTxFractions  --username svagus@sdsu.edu --token $CIBERSORT_TOKEN --single_cell TRUE --refsample LM22.txt --mixture updated_gene_mixture_GSE239773.txt --fraction 0 --rmbatchSmode TRUE
```
- *Note*: Repalce the --refsample and --mixture files as appropriate (files should exist in /src/data)

## Shutdown / Cleanup
Optionally, transfer any files back to your local machine:
Optionally, copy input files to the pod:
```
kubectl -n sdsu-george-lab cp cibersort-pod:/src/data/[file] [path/to/local/file] 
```
- *Note*: Replace `file` with the desired remote/pod filename and replace `path/to/local/file` with your local path, and remove both sets of `[]` brackets

When done, delete your job:
```
kubectl delete -f cibersort-fractions.yaml -n sdsu-george-lab
```

`Note: Your volume will persist so you can start the pod again and have acecss to your data.`
