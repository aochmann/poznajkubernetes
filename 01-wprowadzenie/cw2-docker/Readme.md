## Docker

Pora pobrudzić sobie trochę palce. Celem ćwiczenia jest powtórzenie materiału z lekcji w sposób świadomy, tak by go utrwalić. W lekcji o Docker mówiliśmy o sposobie budowania, najlepszych praktykach i o tagowaniu obrazów. Przećwiczmy to teraz:

### 1. Uruchom swój aktualny projekt w Docker — może to być Twój projekt poboczny lub projekt firmowy, nad którym aktualnie siedzisz. Chodzi o sam fakt uruchomienia go w kontenerze. Możesz też użyć aplikacji z lekcji – https://github.com/PoznajKubernetes/code/tree/master/demos/docker

#### 1.1 Aplikacja z lekcji

go.mod
```
module github.com/poznajkubernetes/code/demos/docker/build/helloapp

go 1.13
```
---
main.go


```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "Cześć, 🚢")
}

func main() {
	http.HandleFunc("/", handler)
	fmt.Println("server started")
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```
---

### 2. Nadaj mu różne tagi

##### Z użyciem CLI

```bash
docker build . --tag some-go-example:v1
```

```bash
docker build . --tag some-go-example:v2
```

##### Z użyciem docker-compose

Z użyciem build args

``` bash
TAG_NAME=latest docker-compose build go-app-first-example
```

Bez użycia build args

``` bash
docker-compose build go-app-second-example
```

### 3. Zmierz czas budowania obrazu (wykorzystaj komendę time w shell, Measure-Command w PowerShell)

#### Z użyciem docker-compose

```bash
time TAG_NAME=latest docker-compose build go-app-first-example
```

Output:
```bash
TAG_NAME=latest docker-compose build go-app-first-example  0.44s user 0.33s system 58% cpu 1.313 total
```

#### Z użycuen CLI
```bash
time docker build . --tag some-go-example:v1
```

Output:
```bash
docker build . --tag some-go-example:v1  0.03s user 0.05s system 0% cpu 9.634 total
```


### 4. Zobacz co się stanie jak powtórzysz komendę budowania obrazu bez zmiany w kodzie i ze zmianami w kodzie

#### Bez zmian w kodzie

Warstwy są pobierane z cache

#### Ze zmianami w kodzie

Korzysta z cache do momentu warstwy z kopiowaniem plików

### 5. Czy jesteś wstanie zmniejszyć czas budowania obrazu? Tak by zmiany w plikach nie powodowały niepotrzebnego opóźnienia w budowaniu obrazu.


### 6. Zobacz, jaki ma rozmiar obraz. Czy jesteś wstanie go zmniejszyć?

#### Rozmiar obrazów
```bash
docker image ls | grep some-go-example
```

| Name | Tag | Size |
| --- | --- | --- |
| some-go-example | 1.8 | 5.84MB | 
| some-go-example | v1 | 7.39MB |
| some-go-example | v2 | 7.39MB |
| some-go-example | v3 | 7.39MB |
| some-go-example | v4 | 7.39MB |
| some-go-example | latest | 7.39MB |
| some-go-example | test | 7.39MB |

#### Czy jesteś wstanie go zmniejszyć?
Tak, używając obrazów mało ważących oraz usuwanie plików po src po procesie budowy.