# Github-Actions

### Github Actions란?

> Github Repository 기반으로 하는 개발 Workflow 자동화 툴, Github에서 제공하는 CI/CD 도구

[공식 Reference](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions#overview), [공식 Page](https://github.com/features/actions)

<br/>

![Workflow overview](./imgs/overview-actions-simple.png)

* Github Actions는 이벤트 기반으로 명령이 실행된다.
* event가 발생하면, job이 포함된 workflow를 자동으로 triggers 한다.
* 그러면 해당 job은 steps를 이용해 actions가 실행되는 순서를 제어한다.

<br/>

<br/>

### Github-Actions의 Components

> Github Actions 실행을 위해 상호작용 하며 함께 작동하는 Components

<br/>

![구성 요소 및 서비스 개요](./imgs/overview-actions-design.png)

<br/>

#### Workflows

> 사실상 최상위 개념에 해당하는 자동화 될 작업들의 사이클

* repository에 추가하는 자동화된 절차
* 하나 이상의 jobs로 구성되며, events에 의해 예약 또는 실행(trigger)될 수 있음
* Github project를 build, test, package, release, deploy(배포) 하는데 사용할 수 있음

<br/>

#### Events

> 자동화 절차인 Workflow를 실행시킬 이벤트

* Workflow를 실행하는 특정 활동을 말함
* 예시 상황
  * commit을 repository에 push 하는 경우
  * 문제 발생 또는 pull request 생성되는 경우
  * 외부 이벤트 발생 시 repository dispatch webhook을 사용하는 경우
  * [그 외의 경우에 대한 전체 목록](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)

<br/>

#### Jobs

> 동일한 러너에서 실행되는 하나의 단계로, Workflow를 구성하는 작업 단위

* 하나의 runner 에서 실행되는 일련의 단계
* 병렬 실행을 기본으로 하지만, 순차 실행 및 의존 관계로 구성 가능

<br/>

#### Steps

> Job을 구성하는 개별 작업 단위

* Job에서 명령(task)을 실행할 수 있는 개별 작업
* Action 또는 shell commeand 일 수 있음
* 동일한 runners에서 실행되므로 job 안의 action들이 서로 데이터 공유 가능

<br/>

#### Actions

> Steps를 구성하는 가장 작은 작업 단위

* Job을 생성하는 steps로 결합되는 독립된 실행 명령
* Workflow에서 가장 작은 작업 단위
* 재사용이 가능한 구성요소 (이식성)
* 고유한 작업을 만들거나 community에서 다른 사람이 만든 Actions를 가져다 사용 가능

<br/>

#### Runners

> Workflow가 수행되기 위한 가상 서버 인스턴스

* GitHub Actions runner application이 설치된 서버로, workflows가 가동되는 인스턴스
  * [Ubuntu Linux, MS Windows, macOS 기반](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
* Github에서 제공하는 러너를 사용하거나 직접 호스팅해서 사용 가능
* Runner는 jobs의 실행 요청에 대한 수신을 항상 대기
* 한 번에 하나의 job을 실행하며, 진행 상황과 로그 및 결과를 github에 보고
* Workflow의 각 jobs는 새로운 가상 환경에서 실행
  * 타 운영체제 사용 희망시, [자체 러너 호스팅](https://docs.github.com/en/actions/hosting-your-own-runners) 가능

<br/>

<br/>

### Workflow 예제를 만들어보자

#### workflow 파일 정의하기

1. Repository에서 `.github/workflows/` 에 해당하는 디렉토리 경로를 생성한다.

2. 해당 디렉토리에 workflow 파일을 생성한다.

   * workflow 파일은 `.yml` 형식의 확장자를 가진다.

   * ex) `learn-github-actions.yml`

     ```yaml
     name: learn-github-actions
     on: [push]
     jobs:
       check-bats-version:
         runs-on: ubuntu-latest
         steps:
           - uses: actions/checkout@v2
           - uses: actions/setup-node@v2
             with:
               node-version: '14'
           - run: npm install -g bats
           - run: bats -v
     ```

3. 변경 사항을 add, commit 후 git repository에 push 한다.

<br/>

#### workflow 파일 이해하기

> 위에서 정의한 `learn-github-actions.yml` 파일을 분석해보자.

<br/>

```yaml
name: learn-github-actions
```

* 해당 workflow 파일의 이름

<br/>

#### workflow 활동 보기

1. Github repository 기본 page로 이동한다.

2. Actions를 클릭한다.

   * 아직 workflow_.yml 파일이 push 되지 않았다면

     다음과 같이 github action 시작 안내 화면이 뜬다.

   ![image-20210807151010751](./imgs/image-20210807151010751.png)

3. 왼쪽 사이드바에서 보려는 Workflow를 클릭한다.

   ![image-20210807170032462](./imgs/image-20210807170032462.png)

4. workflow run 에서 보고싶은 작업 이름을 클릭한다.

   ![image-20210807170057264](./imgs/image-20210807170057264.png)

5. Jobs 아래에서 보고싶은 job 이름을 클릭한다.

   ![image-20210807170348441](./imgs/image-20210807170348441.png)

6. 각 step의 결과를 볼 수 있다.

   ![image-20210807170524577](./imgs/image-20210807170524577.png)

<br/>

<br/>

