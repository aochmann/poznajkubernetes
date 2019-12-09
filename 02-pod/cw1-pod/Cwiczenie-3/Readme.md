## Ćwiczenie 3

### 1. Utwórz w YAML definicję Pod, który nie kończy od razu swojej pracy. Możesz skorzystać z pliku stworzonego YAML w ćwiczeniu 2.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  labels:
    name: busybox
spec:
  containers:
  - name: busybox
    image: busybox:1
    imagePullPolicy: IfNotPresent
    resources: {}
    command: ["sleep"]
    args: ["3600"]
    dnsPolicy: ClusterFirst
    restartPolicy: Never
status: {}
```

### 2. Wgraj go na klaster używając polecenia create.

```bash
kubectl create -f pod-1.yaml --save-config
```

### 3. Wykonaj modyfikację pliku np: zmień czas dla komendy sleep Spróbuj wgrać modyfikacje na klaster używając create lub apply.

create error:
```
Error from server (AlreadyExists): error when creating "pod-1.yaml": pods "busybox" already exists
```

apply error:
```
The Pod "busybox" is invalid: spec: Forbidden: pod updates may not change fields other than `spec.containers[*].image`, `spec.initContainers[*].image`, `spec.activeDeadlineSeconds` or `spec.tolerations` (only additions to existing tolerations)
{"Volumes":[{"Name":"default-token-6sr78","HostPath":null,"EmptyDir":null,"GCEPersistentDisk":null,"AWSElasticBlockStore":null,"GitRepo":null,"Secret":{"SecretName":"default-token-6sr78","Items":null,"DefaultMode":420,"Optional":null},"NFS":null,"ISCSI":null,"Glusterfs":null,"PersistentVolumeClaim":null,"RBD":null,"Quobyte":null,"FlexVolume":null,"Cinder":null,"CephFS":null,"Flocker":null,"DownwardAPI":null,"FC":null,"AzureFile":null,"ConfigMap":null,"VsphereVolume":null,"AzureDisk":null,"PhotonPersistentDisk":null,"Projected":null,"PortworxVolume":null,"ScaleIO":null,"StorageOS":null,"CSI":null}],"InitContainers":null,"Containers":[{"Name":"busybox","Image":"busybox:1","Command":["sleep"

A: ],"Args":["3500"],"WorkingDir":"","Ports":null,"EnvFrom":null,"Env":null,"Resources":{"Limits":null,"Requests":null},"VolumeMounts":[{"Name":"default-token-6sr78","ReadOnly":true,"MountPath":"/var/run/secrets/kubernetes.io/serviceaccount","SubPath":"","MountPropagation":null,"SubPathExpr":""}],"VolumeDevices":null,"LivenessProbe":null,"ReadinessProbe":null,"Lifecycle":null,"TerminationMessagePath":"/dev/termination-log","TerminationMessagePolicy":"File","ImagePullPolicy":"IfNotPresent","SecurityContext":null,"Stdin":false,"StdinOnce":false,"TTY":false}],"RestartPolicy":"Never","TerminationGracePeriodSeconds":30,"ActiveDeadlineSeconds":null,"DNSPolicy":"ClusterFirst","NodeSelector":null,"ServiceAccountName":"default","AutomountServiceAccountToken":null,"NodeName":"docker-desktop","SecurityContext":{"HostNetwork":false,"HostPID":false,"HostIPC":false,"ShareProcessNamespace":null,"SELinuxOptions":null,"RunAsUser":null,"RunAsGroup":null,"RunAsNonRoot":null,"SupplementalGroups":null,"FSGroup":null,"Sysctls":null},"ImagePullSecrets":null,"Hostname":"","Subdomain":"","Affinity":null,"SchedulerName":"default-scheduler","Tolerations":[{"Key":"node.kubernetes.io/not-ready","Operator":"Exists","Value":"","Effect":"NoExecute","TolerationSeconds":300},{"Key":"node.kubernetes.io/unreachable","Operator":"Exists","Value":"","Effect":"NoExecute","TolerationSeconds":300}],"HostAliases":null,"PriorityClassName":"","Priority":0,"DNSConfig":null,"ReadinessGates":null,"RuntimeClassName":null,"EnableServiceLinks":true}

B: ,"15"],"Args":null,"WorkingDir":"","Ports":null,"EnvFrom":null,"Env":null,"Resources":{"Limits":null,"Requests":null},"VolumeMounts":[{"Name":"default-token-6sr78","ReadOnly":true,"MountPath":"/var/run/secrets/kubernetes.io/serviceaccount","SubPath":"","MountPropagation":null,"SubPathExpr":""}],"VolumeDevices":null,"LivenessProbe":null,"ReadinessProbe":null,"Lifecycle":null,"TerminationMessagePath":"/dev/termination-log","TerminationMessagePolicy":"File","ImagePullPolicy":"IfNotPresent","SecurityContext":null,"Stdin":false,"StdinOnce":false,"TTY":false}],"RestartPolicy":"Never","TerminationGracePeriodSeconds":30,"ActiveDeadlineSeconds":null,"DNSPolicy":"ClusterFirst","NodeSelector":null,"ServiceAccountName":"default","AutomountServiceAccountToken":null,"NodeName":"docker-desktop","SecurityContext":{"HostNetwork":false,"HostPID":false,"HostIPC":false,"ShareProcessNamespace":null,"SELinuxOptions":null,"RunAsUser":null,"RunAsGroup":null,"RunAsNonRoot":null,"SupplementalGroups":null,"FSGroup":null,"Sysctls":null},"ImagePullSecrets":null,"Hostname":"","Subdomain":"","Affinity":null,"SchedulerName":"default-scheduler","Tolerations":[{"Key":"node.kubernetes.io/not-ready","Operator":"Exists","Value":"","Effect":"NoExecute","TolerationSeconds":300},{"Key":"node.kubernetes.io/unreachable","Operator":"Exists","Value":"","Effect":"NoExecute","TolerationSeconds":300}],"HostAliases":null,"PriorityClassName":"","Priority":0,"DNSConfig":null,"ReadinessGates":null,"RuntimeClassName":null,"EnableServiceLinks":true}
```

### 4. Zastanów się i opisz na grupie kiedy warto używać create, a kiedy apply.

| Kubectl apply | Kubectl create |
| --- | --- |
| It directly updates in the current live source, only | It first deletes the resources and then creates it from the file provided. |
| The file used in apply can be an incomplete spec  | The file used in create should be complete |
| Apply works only on some properties of the resources  | Create works on every property of the resources |
| You can apply a file that changes only an annotation, without specifying any other properties of the resource. | If you will use the same file with a replace command, the command would fail, due to the missing information. |
