# 요약

> 템플릿 사용 버튼을 누른 후, 차례대로 진행 해 주세요!

1. 저장소의 settings - Secrets and variables - Actions에서 Repository secret을 새로 만들어 주세요.
   1. `release.yaml`에서 PUBLISHER_TOKEN으로 지정되어 있는 토큰을 만드는 작업이에요.
   2. 기본 세팅과 같은 이름인 PUBLISHER_TOKEN으로 새 토큰을 만드시거나
   3. 혹은 원하시는 다른 이름으로 토큰을 만든 뒤, `release.yaml`을 그 이름으로 수정 해 주세요.
2. [pre-commit](#pre-commit)을 초기화 해 주세요. `pre-commit install`. 설치 되어 있지 않다면 [설치](https://pre-commit.com/#installation) 해 주세요.
3. go 모듈을 [초기화](#모듈-초기화하기) 해 주세요. `go mod init github.com/username/repo`
4. cobra-cli [설치](#cobra-설치하기) 후 root 명령어를 초기화 해 주세요.
5. `.goreleaser.yaml` 파일의 brews 키의 값들을 알맞게 수정 해 주세요.
   1. 여기서 자신의 homebrew tap이 존재하지 않으면 하나 만드실 수 있어요.
   2. 깃허브 저장소에 `homebrew-username` [형식](https://docs.brew.sh/Taps#repository-naming-conventions-and-assumptions)으로 저장소를 만들고 [예시](https://github.com/mindulle/homebrew-mindulle)처럼 설정 해 주세요.
   3. 해당 예시의 README.md 파일을 제외한 모든 파일은 이 저장소의 goreleaser가 자동으로 생성했어요.
6. `.releaserc.yaml`의 repositoryUrl 필드의 값을 생성한 저장소의 git 주소로 변경 해 주세요.
7. 이 모든 과정에서 설정이 부족하거나 수정이 필요하다고 생각되시면 자유롭게 수정 해 주세요!

# 설정하기

## pre-commit

### [설치하기](https://pre-commit.com/#installation)

```shell
brew install pre-commit
```

### git hook script [활성화](https://pre-commit.com/#3-install-the-git-hook-scripts)하기.

```shell
pre-commit install
```

### 추가한 훅의 [옵션](https://pre-commit.com/#pre-commit-install) 설정하기

# 명령어 만들기

## 모듈 초기화하기

1. 템플릿 저장소 clone하기. `git clone https://github.com/username/repo`
2. `go mod init <github.com/username/repo>`로 모듈 생성하기

## Cobra 설치하기

go가 설치되어 있다면, go의 cli 프로그램 생성 도구인 [cobra](https://github.com/spf13/cobra#installing)를 설치 해 주세요.

```shell
go install github.com/spf13/cobra-cli@latest
```

### Cobra CLI 어플리케이션 초기화하기

```shell
cobra-cli init
```

### 명령어 추가하기

> `cobra-cli add command-name` 으로 명령어를 추가해요.

> `cobra-cli add child-command-name -p 'parent-command-nameCmd'` 으로 child 명령어를 만들 수 있어요.

- 'parent-command-nameCmd'를 `camelCase`로 작성 해 주세요.
- [cobra-cli](https://github.com/spf13/cobra-cli#add-commands-to-a-project)에서 지정된 패턴이에요.

### 플래그 추가하기

- [Persistent Flag](https://github.com/spf13/cobra/blob/main/user_guide.md#persistent-flags)와 [Local Flag](https://github.com/spf13/cobra/blob/main/user_guide.md#local-flags)도 추가 할 수 있어요.

```go
// Persistent 플래그는 명령어 자신 뿐만 아니라 하위 명령어에도 적용되는 플래그를 말해요.
// 글로벌 플래그를 만들고 싶다면, 루트 명령어에 Persistent 플래그를 적용 해 주면 된답니다.
rootCmd.PersistentFlags().BoolVarP(&Verbose, "verbose", "v", false, "verbose output")

// 원하는 명령어에만 적용 할 수 있는 플래그도 지정 가능합니다!
localCmd.Flags().StringVarP(&Source, "source", "s", "", "Source directory to read from")
```

# 저장소 설정하기

## 추천 모듈

### 설치하기

```shell
go get -u -v "github.com/user/module"
```

### 목록

|                           모듈                           |                       용도                       |
| :------------------------------------------------------: | :----------------------------------------------: |
| [Bubble Tea](https://github.com/charmbracelet/bubbletea) |   Functional 하고 Stateful한 터미널 앱 만들기    |
|  [Lip gloss](https://github.com/charmbracelet/lipgloss)  | 좋은 터미널 레이아웃을 구성하기 위한 스타일 정의 |
|   [glamour](https://github.com/charmbracelet/glamour)    |  CLI 프로그램용 스타일시트 기반 마크다운 렌더러  |

## 이 저장소의 설정 파일

| 파일                              | 용도                                                                                                                                                                                                                                                                                                                                                                  |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.releaserc.yaml`                 | [semantic release](https://semantic-release.gitbook.io/semantic-release/) 모듈의 설정 파일 입니다. [semantic versioning](https://semver.org/)을 기반으로 한 배포를 자동화 할 수 있습니다.                                                                                                                                                                             |
| `.pre-commit-config.yaml`         | [pre-commit](https://pre-commit.com/) 설정 파일.<br/> `pre-commit install`로 git hook을 설치 할 수 있고, `pre-commit sample-config`로 이 파일을 생성 할 수 있습니다.                                                                                                                                                                                                  |
| `.gorelaser.yaml`                 | [GoReleaser](https://goreleaser.com/) 설정 파일. `goreleaser init` 명령어로 생성합니다. <br>컴파일, 릴리즈 노트 생성, Homebrew Formulae를 생성하고 나의 Homebrew Tap 저장소에 배포하는 작업을 자동화 하는 데 사용됩니다.                                                                                                                                              |
| `.github/workflows/release.yaml`. | Github Action을 이용해 [GoReleaser의 작업을 자동화](https://goreleaser.com/ci/actions/?h=github+ac) 하기 위한 설정 파일. <br/> `Homebrew-tapName`으로 [자신](https://github.com/mindulle/homebrew-mindulle)의 [brew tap](https://docs.brew.sh/Taps)을 만들어 두었다면, 이 템플릿에서 만든 명령어를 자신의 tap에 자동 배포해주는 Github Action을 위한 설정 파일이에요. |

# 배포하기

- 기본적으로 `.releaserc.yaml`에 의해 이 저장소는 태깅 되고 있어요.
- 어느정도 로컬 테스트가 완료되면 `git tag tagName (optionalCommitHash)` 명령으로 tag를 붙인 뒤
- `git push --tags` 명령으로 로컬에서 생성한 tag를 저장소에 push 할 수 있어요.
- 자동으로 생성된 tag와 달리 로컬에서 개발자가 생성한 tag에는 `release.yaml`파일에 설정 된 Github Action이 동작해요.
- 그 결과, 자동으로 goReleaser가 여태까지 작성한 CLI 프로그램을 나의 Homebrew tap에 배포해 준답니다!
- 그 이후에는 tap이 [예시 저장소](https://github.com/mindulle/homebrew-mindulle)와 같은 상태가 되며 brew가 설치된 머신에서라면 만든 명령어를 바로 사용 할 수 있게 된답니다.
