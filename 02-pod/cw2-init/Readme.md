## Ćwiczenie

Celem ćwiczenia jest pobranie strony www z repozytorium git za pomocą kontenera typu init. Pobrane dane muszą trafić na wolumen typu emptyDir i zostać wykorzystane do serwowania treści w głównym kontenerze.

 * Do kontenera init z git zbuduj obraz na bazie ubuntu. Należy doinstalować git.
 * Do kontenera serwującego treść wykorzystaj nginx.
 * Jeśli nie posiadasz strony w repo możesz wykorzystać https://github.com/PoznajKubernetes/poznajkubernetes.github.io.

 ---

 ```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
  labels:
    name: web
spec:
  containers:
    - name: web
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: workdir
          mountPath: /usr/share/nginx/html
      resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
  initContainers:
    - name: install
      image: ubuntu
      command: ['/bin/sh']
      args: [
        "-c",
        "apt-get update -y",
        "apt-get install -y git",
        "git clone \"https://github.com/PoznajKubernetes/poznajkubernetes.github.io\" /work-dir",
        "",
      ]
      volumeMounts:
        - name: workdir
          mountPath: '/work-dir'
  volumes:
    - name: workdir
      emptyDir: {}
status: {}
 ```