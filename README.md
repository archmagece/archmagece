# 이채응 (archmagece)

**백엔드·플랫폼 엔지니어** — 제품이 배포되고 운영되는 전체 흐름을 다룹니다.
Backend & platform engineer. Spring/Kotlin services, Kubernetes delivery, and the tooling in between.

```text
Backend    Kotlin · Java · Spring Boot / Security / Cloud Gateway · JPA · gRPC
Messaging  Kafka · RabbitMQ
Auth       OAuth2 / OIDC · JWT · Keycloak (custom SPI)
Delivery   Docker · Kubernetes · Helm · ArgoCD · GitHub Actions · Terraform · Ansible
Data       PostgreSQL · MySQL · Redis · Elasticsearch
Observe    Prometheus · Grafana · Loki · Jaeger · Fluent Bit · ELK
```

인증·결제·메시징 같은 핵심 도메인을 Spring/Kotlin으로 설계·구현하고, 그 서비스가 Kubernetes에 올라가는 배포 파이프라인과 관측성까지 직접 구성해 왔습니다. `@Transactional` 남용으로 생긴 성능 저하, Realm 과다로 인한 Keycloak 부팅 지연처럼 **운영 중에야 드러나는 구조적 문제를 원인부터 고치는 작업**을 중요하게 봅니다. 반복되는 인증·패키지·배포 작업은 플랫폼 기능으로 코드화하는 쪽을 택합니다.

## 📦 공개 프로젝트

개인 계정에는 포크와 블로그가 대부분이고, 만든 것들은 **[@ScriptonBasestar](https://github.com/ScriptonBasestar)** · **[@ScriptonBasestar-containers](https://github.com/ScriptonBasestar-containers)** · **[@gizzahub](https://github.com/gizzahub)** 조직에 모아 두었습니다. 아래는 그중 실제로 설치해서 쓰고 있는 것들입니다.

**배포·개발환경 도구**

| 프로젝트 | 무엇인가 | 스택 |
|---|---|---|
| **[sbkube](https://github.com/ScriptonBasestar/sb-kube-app-manager)** | Helm 차트·YAML 매니페스트·Git 리소스를 하나의 선언적 설정으로 묶어 k3s/Kubernetes에 배포하는 CLI. [PyPI 배포](https://pypi.org/project/sbkube/) | Python · Helm · Kubernetes |
| **[dva](https://github.com/ScriptonBasestar/dva)** | `dva.yml` 하나로 Docker Compose·Kubernetes·Helm·로컬 프로세스를 함께 띄우는 개발환경 오케스트레이터. 실행 조합을 named plan으로 고릅니다. `go install` 배포 | Go |
| **[gzh-cli](https://github.com/gizzahub/gzh-cli)** (`gz`) | 개발 환경 통합 CLI — GitHub/GitLab/Gitea/Gogs 다중 플랫폼 관리, IDE·코드품질·패키지매니저 서브커맨드를 하나의 인터페이스로 | Go |
| **[gzh-cli-gitforge](https://github.com/gizzahub/gzh-cli-gitforge)** (`gz-git`) | 수십 개 저장소를 한 번에 다루는 벌크 Git CLI + Go 라이브러리. ad-hoc 스캔 엔진과 `.gz-git.yaml` 선언 엔진을 분리해, 워크스페이스 동기화와 태스크 브랜치 통합까지 규약으로 굳혔습니다 | Go |
| **[gzh-cli-package-manager](https://github.com/gizzahub/gzh-cli-package-manager)** (`gz-pm`) | Homebrew·npm·pip·asdf·apt를 한 인터페이스로 갱신·설정하는 패키지 매니저 오케스트레이터 | Go |

**컨테이너 · 차트**

| 프로젝트 | 무엇인가 | 스택 |
|---|---|---|
| **[sb-helm-charts](https://github.com/ScriptonBasestar-containers/sb-helm-charts)** | 자체 운영용 Helm 차트 모음. GitHub Pages 차트 저장소와 GHCR OCI 레지스트리 양쪽으로 배포 | Helm · OCI · GitHub Actions |
| **[sb-docker-images](https://github.com/ScriptonBasestar-containers/sb-docker-images)** | 개발·테스트용 이미지/Compose 스택 56종. CI에서 전량 빌드 검증 후 CD로 퍼블리시 | Docker · Compose · GitHub Actions |

**인증 확장**

| 프로젝트 | 무엇인가 | 스택 |
|---|---|---|
| **[sb-keycloak-exts](https://github.com/ScriptonBasestar/sb-keycloak-exts)** | Keycloak 확장 — 카카오·네이버·LINE Identity Provider SPI와, 인증 이벤트를 Kafka/RabbitMQ/NATS/Redis로 흘리는 Event Listener | Kotlin · Keycloak SPI |

## 🧱 자체 플랫폼 구성요소 (설계 · 비공개 저장소)

자체 서비스를 준비하면서 플랫폼 구성요소를 직접 설계하고 **AI 개발 도구로 구현**했습니다. 코드는 self-hosted GitLab에 있어 공개 링크가 없습니다 — 아래는 설계 범위이며, 대규모 프로덕션 운영 경험이 아닙니다.

- **인가 서버** — FAPI 2.0 프로파일 OIDC/OAuth2, 적합성 스위트로 검증
- **DBaaS 컨트롤 플레인** — DB 라이프사이클 오케스트레이션(CloudNativePG·PXC 오퍼레이터 연동), 비동기 작업의 멱등성·펜싱
- **패키지 프록시 + OCI 레지스트리** — apt/npm/Docker 프록시와 컨테이너 레지스트리
- **DNS 라이프사이클** — 존/레코드 선언과 드리프트 조정, AWS/GCP/Azure Terraform
- **배포 안전장치** — Argo Rollouts 기반 progressive delivery, 롤백 오케스트레이션, 승인 게이트

설계 의도와 동작, 플랫폼에서의 역할은 설명할 수 있습니다. 구현 디테일은 AI 도구로 작성했습니다.

## ✍️ 링크

- 기술 블로그 — https://archmagece.github.io
- 조직 — [@ScriptonBasestar](https://github.com/ScriptonBasestar) (제품·라이브러리) · [@ScriptonBasestar-containers](https://github.com/ScriptonBasestar-containers) (이미지·차트) · [@gizzahub](https://github.com/gizzahub) (개발자 도구)

<!--
관리: 이 파일은 생성물이다. 정본은 개인 vault
JOB/80-퍼블릭프로필/10-프로필README-원고.md 이며 make profile-readme-sync 로 갱신한다.
-->
