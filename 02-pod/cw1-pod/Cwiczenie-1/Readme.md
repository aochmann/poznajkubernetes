## Ćwiczenie 1

### 1. Utwórz definicje (plik YAML) pod z obrazem busybox za pomocą polecenia kubectl run z opcjami lub używając edytora tekstowego. Pamiętaj o opcjach --dry-run oraz -o yaml


```bash
kubectl run busybox --image=busybox --generator=run-pod/v1 --dry-run -o yaml > pod.yaml
```

#### Output

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  containers:
  - image: busybox
    name: busybox
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

### 2. Używając polecenia kubectl create stwórz Pod na podstawie definicji z punktu 1.

```bash
kubectl create -f pod.yaml
```

### 3. Sprawdź status tworzenia Pod za pomocą get. Możesz też ustawić w osobnym oknie sprawdzanie „ciągłe” z opcją -w czyli --watch i zostawić je do końca ćwiczeń.

```bash
kubectl get pod -w
```

```
NAME      READY   STATUS    RESTARTS   AGE
busybox   0/1     Pending   0          0s
busybox   0/1     Pending   0          0s
busybox   0/1     ContainerCreating   0          0s
busybox   0/1     Completed           0          7s
```

### 4. Zastanów się dlaczego Pod przeszedł w stan Completed.

Proces w kontenerze zakończył prace.