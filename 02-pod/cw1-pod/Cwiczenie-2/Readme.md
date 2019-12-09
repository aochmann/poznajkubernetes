## Ćwiczenie 2

### 1. Korzystając ze stanu klastra z ćwiczenia 1 oraz pliku YAML, przystąp do ćwiczenia.

### 2. Popraw plik YAML dodając command tak by Pod nie kończył od razu swojej pracy. Np: dodaj lub zmień tag w obrazie. Poprawne tag dla busybox znajdziesz na stronie: https://hub.docker.com/_/busybox

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

### 3. Użyj polecenia kubectl create by wgrać nową definicję.

```
Error from server (AlreadyExists): error when creating "pod.yaml": pods "busybox" already exists
```

### 4. Zastanów się dlaczego nie działa.

Bark opcji --save-config=true w trackie tworzenia poda.

### 5. Usuń starą definicję za pomocą kubectl delete.

```bash
kubectl delete pod busybox
```