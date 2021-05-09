# kubebuilder

# 소개

Note: 급한 사람을 [시작하기](https://book.kubebuilder.io/quick-start.html)를 바로 봐도 된다.

Kubebuilder v1을 사용하고 있다면 [예전 문서](https://book-v1.book.kubebuilder.io/)를 확인해라.

## 누구를 위한 것인가

### Kubernetes 사용자
Kubernetes의 사용자는 API가 설계 방식과 구현의 기반이 되는 개념을 배우면서 Kubernetes를 더 깊이 이해하게 될 것이다. 이 책은 자신만의 Kubernetes API를 개발하는 방법과 핵심 Kubernetes API가 설계된 원칙을 알려줄 것이다.

다음의 내용이 포함되어 있다.

* Kubernetes API와 리소스의 구조
* API 버저닝의 의미
* 자연치유(Self-healing)
* 가비지 컬렉션과 파이널라이저(Finalizer)
* 선언적(Declarative) API vs 명령형(Imperative) API
* 레벨 기반 API vs 엣지 기반 API
* 리소스 vs 서브 리소스

### Kubernetes API 확장 개발자
API 확장 개발자는 빠른 실행을 위한 간단한 도구와 라이브러리뿐 아니라 표준 Kubernetes API를 구현의 배경이 되는 원칙과 개념을 배울 것이다. 이 책은 확장 개발자가 일반적으로 겪게 되는 함정과 오해를 다루고 있다.

다음의 내용이 포함되어 있다.

* 단일 조정(reconciliation) 호출을 하는 방법
* 주기적인 조정을 설정하는 방법
* 이어지는 내용
  * 언제 lister 캐시를 사용하고 라이브 검색을 사용하는지
  * 가비지 컬렉션 vs 파이널라이저(Finalizer)
  * 선언적 밸리데이션 vs 웹훅 밸리데이션을 사용하는 방법
  * API 버저닝을 구현하는 방법

## 왜 Kubernetes API인가
Kubernetes API는 일관되고 풍부한 구조에 연결된 객체에 대한 일관되고 잘 정의된 엔드포인트를 제공한다.

이 접근은 Kubernetes API와 동작하는 도구와 라이브러리의 풍부한 생태계를 만든다.

사용자는 yaml이나 json으로 객체를 선언하고 객체를 관리하는 일반적인 도구를 사용해서 API를 이용한다.

Kubernetes API처럼 서비스를 만들면 다음과 같이 평범하고 오래된 REST(plain old REST)에 많은 장점을 준다.

* 호스팅 된 API 엔드포인트, 스토리지, 밸리데이션
* `kubectl`이나 `kustomize`같은 풍부한 도구와 CLI
* Authn과 세밀한 Authz 지원
* API 버저닝과 변환을 통한 API 발전 지원
* 사용자의 개입 없이 시스템에서 변경사항에 지속해서 응답하는 적응형/자연 치유 API의 간편화
* 호스팅 된 환경으로서의 Kubernetes

운영 중인 Kubernetes 클러스터에 설치할 자신만의 Kubernetes API를 만들어서 발행할 것이다.

## 기여
이 책이나 코드에 기여하고 싶다면 [기여](https://github.com/kubernetes-sigs/kubebuilder/blob/master/CONTRIBUTING.md) 가이드라인 문서를 먼저 읽어보기 바란다.

## 리소스
* 저장소: [sigs.k8s.io/kubebuilder](https://sigs.k8s.io/kubebuilder)
* 슬랙 채널: [#kubebuilder](http://slack.k8s.io/#kubebuilder)
* Google 그룹: [kubebuilder@googlegroups.com](https://groups.google.com/forum/#!forum/kubebuilder)
