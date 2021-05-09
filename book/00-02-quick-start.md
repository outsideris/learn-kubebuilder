# 시작하기
이 시작하기 가이드는 다음을 다룰 것이다.

* [프로젝트 생성하기](https://book.kubebuilder.io/quick-start.html#create-a-project)
* [API 생성하기](https://book.kubebuilder.io/quick-start.html#create-an-api)
* [로컬에서 실행하기](https://book.kubebuilder.io/quick-start.html#test-it-out)
* [클러스터에서 실행하기](https://book.kubebuilder.io/quick-start.html#run-it-on-the-cluster)

## 전제조건

* [go](https://golang.org/dl/) v1.15 이상 1.16 미만의 버전
* [docker](https://docs.docker.com/install/) 17.03 이상의 버전
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.11.3 이상의 버전
* Kubernetes v1.11.3 이상의 클러스터에 대한 접근 권한

### 버전과 지원
Kubebuilder로 생성한 프로젝트는 `Makefile`을 가지고 있고 이 파일은 생성할 때 정의된 버전으로 도구를 설치할 것이다. 다음의 도구들이 설치된다.

* [kustomize](https://kubernetes-sigs.github.io/kustomize/)
* [controller-gen](https://github.com/kubernetes-sigs/controller-tools)

`Makefile`과 `go.mod` 파일에 정의된 버전은 테스트 된 버전이므로 지정한 버전을 사용하기를 추천한다.

## 설치
[kubebuilder](https://sigs.k8s.io/kubebuilder)를 설치한다.

```bash
# download kubebuilder and install locally.
curl -L -o kubebuilder https://go.kubebuilder.io/dl/latest/$(go env GOOS)/$(go env GOARCH)
chmod +x kubebuilder && mv kubebuilder /usr/local/bin/
```

### master 브랜치 사용하기
`https://go.kubebuilder.io/dl/master/$(go env GOOS)/$(go env GOARCH)`로 설치해서 master를 사용할 수도 있다.

### Enabling shell autocompletion
Kubebuilder는 `kubebuilder completion <bash|zsh>` 명령어로 Bash와 Zsh의 자동완성을 지원해서 많은 타이핑을 줄일 수 있다. 자세한 내용은 [completion](https://book.kubebuilder.io/reference/completion.html) 문서를 참고해라.

## 프로젝트 생성하기
다음과 같이 디렉터리를 만들고 `init` 명령어를 실행해서 새로운 프로젝트를 초기화해라.

```bash
mkdir -p ~/projects/guestbook
cd ~/projects/guestbook
kubebuilder init --domain my.domain --repo my.domain/guestbook
```

### $GOPATH에서 개발하기
`GOPATH` 내에서 프로젝트를 초기화했다면 암묵적으로 호출된 `go mod init`이 모듈 경로를 추가할 것이다. 아니면 `--repo=<module path>`를 반드시 설정해야 한다.

모듈 시스템에 익숙지 않다면 Go modules에 대해서 읽어봐라.

## API 생성하기
다음 명령어를 실행해서 새로운 API(group/version)를 `webapp/v1`로 만들고 여기에 새로운 Kind(CRD) Guestbook을 만든다.

```bash
$ kubebuilder create api --group webapp --version v1 --kind Guestbook
```

### 옵션 선택
Create Resource [y/n]와 Create Controller [y/n]에서 `y`를 눌렀다면
API가 정의된 `api/v1/guestbook_types.go` 파일과 이 Kind(CRD)에 대한 조정(reconciliation) 비즈니스 로직이 구현된 `controllers/guestbook_controller.go` 파일을 생성할 것이다.

선택적: API 정의와 조정 비즈니스 로직을 수정해라. 자세한 내용은 [API 설계](https://book.kubebuilder.io/cronjob-tutorial/api-design.html)와 [Controller에는 무엇이 있는가](https://book.kubebuilder.io/cronjob-tutorial/controller-overview.html)를 살펴봐라.


`(api/v1/guestbook_types.go)` 예시이다.

```go
// GuestbookSpec defines the desired state of Guestbook
type GuestbookSpec struct {
    // INSERT ADDITIONAL SPEC FIELDS - desired state of cluster
    // Important: Run "make" to regenerate code after modifying this file

    // Quantity of instances
    // +kubebuilder:validation:Minimum=1
    // +kubebuilder:validation:Maximum=10
    Size int32 `json:"size"`

    // Name of the ConfigMap for GuestbookSpec's configuration
    // +kubebuilder:validation:MaxLength=15
    // +kubebuilder:validation:MinLength=1
    ConfigMapName string `json:"configMapName"`

    // +kubebuilder:validation:Enum=Phone;Address;Name
    Type string `json:"alias,omitempty"`
}

// GuestbookStatus defines the observed state of Guestbook
type GuestbookStatus struct {
    // INSERT ADDITIONAL STATUS FIELD - define observed state of cluster
    // Important: Run "make" to regenerate code after modifying this file

    // PodName of the active Guestbook node.
    Active string `json:"active"`

    // PodNames of the standby Guestbook nodes.
    Standby []string `json:"standby"`
}

// +kubebuilder:object:root=true
// +kubebuilder:subresource:status
// +kubebuilder:resource:scope=Cluster

// Guestbook is the Schema for the guestbooks API
type Guestbook struct {
    metav1.TypeMeta   `json:",inline"`
    metav1.ObjectMeta `json:"metadata,omitempty"`

    Spec   GuestbookSpec   `json:"spec,omitempty"`
    Status GuestbookStatus `json:"status,omitempty"`
}
```

## 테스트해 보기
실행할 Kubernetes 클러스터가 필요할 것이다. 테스트할 로컬 클러스터를 위해 [KIND](https://sigs.k8s.io/kind)를 사용하거나 원격 클러스터에서 실행할 수 있다.

클러스터에 CRD를 설치한다.

```bash
$ make install
```

컨트롤러를 실행해라.(이는 포그라운드(foreground)로 실행되므로 실행된 채로 두려면 새로운 터미널을 사용해야 한다.)

```bash
$ make run
```

### 사용되는 컨텍스트
컨트롤러는 자동으로 kubeconfig 파일의 현재 컨텍스트를 사용할 것이다.(예: `kubectl cluster-info`이 보여주는 정보)

## 커스텀 리소스의 인스턴스 설치
Create Resource [y/n]에서 `y`를 누르면 예제의 커스텀 리소스 정의(CRD)의 커스텀 리소스(CR)를 생성할 것이다.(API 정의를 바꾸었다면 변경했는지 확인해라.)

```bash
$ kubectl apply -f config/samples/
```

## 클러스터에서 실행하기
`IMG`로 지정한 위치에 이미지를 빌드하고 푸시해라.

```bash
$ make docker-build docker-push IMG=<some-registry>/<project-name>:tag
```

`IMG`로 지정한 이미지로 클러스터에 컨트롤러를 배포해라.

```bash
$ make deploy IMG=<some-registry>/<project-name>:tag
```

### RBAC 에러
RBAC 에러가 발생한다면 cluster-admin 권한을 부여하거나 어드민으로 로그인해야 한다. 이 문제에 해당하는지 [GKE 클러스터 v1.11.x나 그 이전에서 Kubernetes RBAC을 사용할 때 전체조건](https://cloud.google.com/kubernetes-engine/docs/how-to/role-based-access-control#iam-rolebinding-bootstrap)을 살펴보라.

## CRD 언인스톨
클러스터에서 CRD를 제거하려면 다음 명령어를 실행해라.

```bash
$ make uninstall
```

## 다음 단계
더 나은 개요는 [아키텍처 개념 다이어그램](https://book.kubebuilder.io/architecture.html)을 참고하고 데모 예제 프로젝트를 개발로 동작 방식을 더 이해하려면 [CronJob 튜토리얼](https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial.html)을 따라 해봐라.
