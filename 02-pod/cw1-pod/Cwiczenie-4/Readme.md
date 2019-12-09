## Ćwiczenie 4

### 1.Posprzątaj swój klaster tak by nie było na nim stworzonych przez Ciebie Pod.

```bash
kubectl delete pod busybox
```

### 2. Pamiętaj o get i delete.

```bash
kubectly get pods
```

Output
```bash
kubectly get pods
No resources found.
```

### 3. Czy masz pomysł jak to zautomatyzować?

Usuwanie wszystkich podów
```bash
kubectl delete pods --all
```

Usuwanie podów pod nazwą busybox

```bash
kubectl get pods | grep busybox | awk '/busybox/' | xargs kubectl delete pod
```

Usuwanie podów pod nazwą label
```bash
kubectl delete pods -l name=busybox
```

Usuwanie podów pasujących do definicji z pliku

```bash
kubectl delete -f ./pod.yaml
```