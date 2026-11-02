---
title: "레슨 1 - GitHub Copilot CLI 설치하기"
description: "Codespace에 Copilot CLI를 설치하고 인증한 뒤 기본 사용법을 익히고 미리 등록된 필터링 이슈를 찾습니다."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli]는 터미널에서 실행되는 강력한 에이전트형 코딩 도우미입니다. 코드베이스를 살펴보고 코드를 생성하고 명령을 실행하며 외부 도구와 상호 작용하는 작업을 모두 명령줄에서 수행합니다. 작업을 위임하고 변경을 요청하면서 개발 흐름을 이어 갈 수 있습니다. 먼저 도구를 설치해야 합니다. 다행히 이미 익숙한 도구를 사용해 설치할 수 있습니다.

이 레슨에서는 다음을 수행합니다.

- npm을 사용해 GitHub Copilot CLI를 설치합니다.
- GitHub 계정으로 인증합니다.
- 워크숍 리포지토리를 신뢰하고 간단한 대화를 시도합니다.
- 기본 제공 GitHub MCP 서버를 사용해 필터링 이슈를 찾습니다.

## 시나리오

팀은 늘어나는 백로그를 처리하기 위해 AI 에이전트를 사용하기 시작했습니다. Copilot CLI는 많은 개발자가 주로 사용하는 터미널에서 이러한 기능을 제공합니다. 이 레슨에서는 도구를 설치하고 인증하여 워크숍의 나머지 부분에 사용할 준비를 합니다.

## Copilot CLI 설치하기

Copilot CLI는 [npm][install-cli], WinGet, Homebrew로 설치할 수 있습니다. GitHub Codespaces에는 Node.js가 이미 설치되어 있으므로 npm을 사용합니다.

1. Codespace로 돌아가 터미널을 엽니다.
2. Node.js가 설치되어 있고 필요한 버전을 충족하는지 확인합니다.

   ```bash
   node --version
   ```

   버전 24 이상이 표시되어야 합니다.

3. Copilot CLI를 전역으로 설치합니다.

   ```bash
   npm install -g @github/copilot
   ```

4. 설치를 확인합니다.

   ```bash
   copilot --version
   ```

   버전 번호가 표시되어야 합니다.

## GitHub로 인증하기

Copilot CLI를 처음 실행하면 GitHub 계정으로 인증하라는 메시지가 표시됩니다.

1. Copilot CLI를 시작합니다.

   ```bash
   copilot
   ```

2. 메시지가 표시되면 디바이스 코드 안내에 따라 인증하고 Copilot CLI를 승인합니다.
3. Copilot CLI에 다음 메시지가 표시됩니다.

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 경로가 Tailspin Toys 리포지토리인지 확인한 다음 **Yes, and remember this folder for future sessions**를 선택합니다.

> [!NOTE]
> Codespace에서는 GitHub 세션으로 이미 인증되어 있을 수 있습니다. Copilot CLI가 인증 메시지 없이 시작되면 계속 진행하면 됩니다.

## 기본 사용법 익히기

일반 셸 프롬프트에서 명령을 입력하면 Codespace에서 바로 실행됩니다. Copilot CLI가 시작된 뒤 자연어 프롬프트는 에이전트에 전달되고, 슬래시 명령은 대화를 제어합니다.

1. `/model`을 입력하고 화살표 키로 **Auto**를 선택한 뒤 <kbd>Enter</kbd>를 누릅니다. 다시 <kbd>Enter</kbd>를 눌러 확인합니다.
2. `/help`를 입력해 설치된 버전에서 사용할 수 있는 명령을 확인한 다음 <kbd>Esc</kbd>를 눌러 도움말 화면을 닫습니다.
3. 다음 질문을 입력해 모든 항목이 작동하는지 확인합니다.

   ```plaintext
   이 프로젝트에서 중요한 파일은 무엇인가요?
   ```

4. 응답을 읽고 Copilot이 답변 전에 리포지토리를 살펴보는지 확인합니다.
5. `/mcp list`를 입력하고 기본 제공 GitHub MCP 서버를 사용할 수 있는지 확인합니다.
6. Copilot에 필터링 이슈를 찾아 달라고 요청합니다.

   ```plaintext
   GitHub MCP를 사용해 이 리포지토리에서 "Allow users to filter games by category and publisher"라는 제목의 이슈를 찾아 주세요. URL과 간단한 요약을 알려 주세요. 아무것도 변경하지 마세요.
   ```

7. URL을 열어 이슈를 읽습니다. 간단한 변경을 완료한 뒤 이 이슈를 사용합니다.

> [!TIP]
> 일반적인 Copilot CLI 세션은 터미널에서 현재 체크아웃한 브랜치에서 작업하며, 자동으로 워크트리를 만들지 않습니다. 각 변경 작업 전에 기능 브랜치를 만듭니다.

## 워크숍 단축 옵션 사용하기

Copilot CLI는 기본적으로 허용된 권한 밖의 도구를 사용하기 전에 확인을 요청합니다. 이 워크숍에서는 Codespace 안에서 승인 요청을 줄여 실습에 집중할 수 있도록 사용자가 승인한 단축 옵션 `--yolo`를 사용합니다.

> [!CAUTION]
> `--yolo`는 전체 자동 권한(`--allow-all-tools`, `--allow-all-paths`, `--allow-all-urls`)을 활성화합니다. Codespace나 VM 같은 격리된 환경에서만 사용하고, 일상적인 개발을 위한 기본 별칭으로는 절대 설정하지 않습니다. 자세한 내용은 [도구 사용 허용 및 거부][allow-all-warning]를 참조합니다.

이 워크숍에서 `--enable-all-github-mcp-tools`는 이후 레슨에서 이슈와 풀 리퀘스트를 처리하는 데 필요한 읽기/쓰기 GitHub MCP 도구를 활성화합니다. Codespace는 로컬 컴퓨터에 대한 액세스를 제한하지만 인증된 GitHub 리소스에는 실제로 접근합니다. 게시하거나 병합하기 전에 변경 사항을 검토합니다.

1. `/exit`로 Copilot CLI를 종료합니다.
2. 리포지토리 루트에서 다시 시작합니다.

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 프로젝트에 관한 간단한 질문을 다시 입력해 대화가 작동하는지 확인한 다음 `/exit`로 종료합니다.

Copilot은 대화를 자동으로 저장합니다. 이후 지침을 바꾸거나 에이전트를 추가한 뒤에는 `copilot --resume`을 사용해 동일한 기능 대화와 브랜치로 돌아옵니다.

## 요약 및 다음 단계

이 레슨에서는 다음을 수행했습니다.

- npm을 사용해 GitHub Copilot CLI를 설치했습니다.
- GitHub 계정으로 인증했습니다.
- 워크숍 리포지토리를 신뢰하고 간단한 대화를 시도했습니다.
- 기본 제공 GitHub MCP 서버를 사용해 필터링 이슈를 찾았습니다.

다음으로 [첫 번째 변경 작업][next-lesson]을 시작해 Copilot CLI로 게임 카드에 별점을 표시합니다.

## 리소스

- [GitHub Copilot CLI 설치][install-cli]
- [GitHub Copilot CLI 정보][about-copilot-cli]
- [Copilot CLI 명령 참조][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
