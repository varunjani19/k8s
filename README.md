# Kubernetes Imperative Commands

# Pre-preparation OR Tips

Alias for kubectl only is enough:
alias k=kubectl

Export some repitative commands:
export do="--dry-run=client -o yaml" // We will see how to use this in commands below($do)

# Short names of different K8s Objects

cm = configmaps
ds = daemonsets
deploy = deployments
ep = endpoints
ev = events
ing = ingresses
limits = limitranges
ns = namespaces
no = nodes
pvc = persistentvolumeclaims
pv = persistentvolumes
po = pods
rs = replicasets
rc = replicationcontrollers
quota = resourcequotas
sa = serviceaccounts
svc = services


# Let's start with Pods, k is an alias for kubectl!

# Pod Creation

k run pod01 --image nginx // Creates Pod pod01 with nginx image

k run pod02 --image nginx --port=80 // Creates pod02 and allows traffic on port 80

k run pod03 --image busybox -it --echo 'Hello World!' // Creates pod03 and echoes "Hello World". We can use -it to run any other command along with the pod creation

k run pod03 --image busybox -it --echo 'Hello World!' --rm // Flag --rm deletes the pod03 after running echo command. Use this when you want pod to do a specific task and go down once done.

k run pod04 --image busybox --env=var1=value1 // Creates pod04 and sets environment variable var1=value1

k run pod05 --image busybox --labels=key1=value1 // Creates pod05 with label key1=value1

k run pod06 --image nginx --requests='cpu=100m,memory=250Mi' --limits='cpu=200m,memory=512Mi" // Creates pod06 with specified requests and limits

# Displaying Pods

k get po // Lists all pods

k get po pod01 // Returns pod01 and its specifics

k get po -o wide // Displays extra columns in the pod list

k get po --show-lables // Lists all pods with respective labels

k get po -L key1 // Returns "key1" label of all pods

k get po -l key1=value1 // Returns pods with label key1=value1

k get po -l 'key1 in (value1, value2) // Returns pods with label key1=value1 or key1=value2

k get po pod01 --v=7 // Display Pod pod01 with verbocity(in simple words, details) level of 7. Use this to get more details about the pod based on requirement/ understanding

k get po -o=custom-columns="POD_NAME:.metadata.name, POD_STATUS:.status.contStatus[].state" // Returns extra columns POD_NAME and POD_STATUS with specific values

# Label/ Annotate

k label po pod01 app=v1 // Adds label app=v1 to pod01

k label po pod01 app=v2 --overwrute // Overwrites value of label app in pod01

k label po pod01 app- // Removes "app" label from pod01

k annotate po pod01 desc="Some description" // Annotation "desc" gets added to pod01

k annotate po pod01 desc- // Removes annotation "desc" from pod01

# Execute a command

k exec -it pod01 -c cont01 --/bin/sh // Connects to a container "cont01" under pod01. We can then run any command under connected container.

k exec -it pod01 -c cont01 -- ls // Connects to cont01 and runs "ls" command. If container name is not provided with -c flag, by default first declared container gets connected.

# Display/ stream Logs

k logs pod01 // Displays pod logs

k logs -f pod01 // Stream logs for pod01

k logs -f pod01 cont01 // Stream logs for container "cont01" under pod01


# Moving to Deployments

k create deploy mydeploy01 --image nginx // Creates a Deployment "mydeploy01" with nginx image

k scale deploy mydeploy01 --replicas=3 // Scales deployment to have 3 replicasets

k set image deploy mydeploy01 pod001=busybox // Sets image of pod001 to busybox

k set image deploy mydeploy01 cont01=nginx cont02=busybox cont03=bash // Used for multi-container pods, to set an image for containers

k autoscale deploy mydeploy01 --min=5 --max=10 --cpu-percent=80 // Autoscales deployment between 5 to 10 with max CPU utilization at 80%




