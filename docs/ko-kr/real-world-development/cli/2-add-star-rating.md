---
title: "레슨 2 - 빠른 성과로 별점 추가하기"
description: "Copilot CLI로 게임 카드에 작은 변경을 적용하고, 포워딩된 브라우저에서 검토한 뒤, 첫 풀 리퀘스트로 병합합니다."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI를 설치하고 대화를 시도했으니 이제 프로젝트를 처음으로 변경합니다. 범위는 작게 유지합니다. 게임 데이터에는 별점이 있지만 홈페이지의 게임 카드에는 표시되지 않습니다. 에이전트에 별점을 표시하도록 요청하고 변경 내용을 검토한 뒤 첫 풀 리퀘스트로 병합합니다.

이 레슨에서는 다음을 수행합니다.

- 기능 브랜치에서 Copilot과 새 대화를 시작합니다.
- 에이전트에 프로젝트를 작게 변경하도록 요청합니다.
- `/diff`로 변경 사항을 검토합니다.
- 앱을 실행하고 포워딩된 브라우저에서 변경 내용을 확인합니다.
- 첫 풀 리퀘스트를 만들고 병합합니다.

## 시나리오

Tailspin Toys의 각 게임에는 별점이 있을 수 있으며, 별점은 이미 게임 상세 페이지에 표시됩니다. 하지만 홈페이지의 게임 카드에는 제목, 카테고리, 퍼블리셔, 설명만 표시됩니다. 워밍업으로 에이전트에 각 카드의 별점을 표시하도록 요청합니다. 첫 세션에 적합한 작고 독립적인 변경입니다.

## 대화의 구성

**대화(conversation)**는 Copilot CLI로 작업을 수행하는 곳입니다. Copilot app과 달리 일반적인 CLI 대화는 터미널에서 현재 체크아웃한 리포지토리와 Git 브랜치에서 진행되며 전용 워크트리를 만들지 않습니다. 저장된 대화를 이어서 사용할 수 있으며, 파일과 브랜치는 일반적인 Git 상태로 디스크에 남습니다.

대화에는 세 가지 요소가 표시됩니다. 프롬프트와 에이전트 응답, 파일을 살펴보고 편집하는 동안의 도구 활동, `/diff`로 확인할 수 있는 변경 사항입니다.

## 대화를 시작하고 변경 요청하기

새 대화를 시작하고 기능을 구현합니다.

1. Codespace로 돌아갑니다.
2. 터미널이 열려 있지 않다면 Mac에서는 <kbd>Control</kbd>+<kbd>\`</kbd>를, Windows/Linux에서는 <kbd>Ctrl</kbd>+<kbd>\`</kbd>를 누릅니다.
3. 아직 Copilot CLI를 시작하지 않았다면 다음 명령을 실행합니다.

   ```bash
   copilot --yolo
   ```

4. `/new` 슬래시 명령을 입력하고 <kbd>Enter</kbd>를 눌러 새 세션을 시작합니다.
5. 다음 프롬프트로 변경을 요청합니다.

   ```plaintext
   목록 페이지의 게임 카드에 각 게임의 starRating을 5점 만점으로 표시해 주세요. 별점이 null이면 "No rating yet"를 표시합니다. 카드 레이아웃은 그대로 유지하고 테스트를 추가한 뒤 관련 검사를 실행해 주세요.
   ```

Copilot은 프로젝트를 살펴보고 게임 세부 정보를 표시하는 파일을 찾아 필요한 코드를 작성합니다. Copilot CLI로 새 기능을 추가했습니다.

## diff 검토하기

작은 변경이라도 AI가 생성한 모든 변경 사항은 병합하기 전에 검토해야 합니다. Copilot CLI에서 변경 내용을 살펴봅니다.

1. `/diff`를 입력하고 변경된 파일을 모두 살펴봅니다.
2. 별점이 있는 경우 게임 카드에 숫자 값이 표시되고 `starRating`이 `null`인 경우 `No rating yet`가 표시되는지 확인합니다.
3. 두 가지 상태를 테스트하는지 확인합니다.
4. Copilot이 실행한 검사 결과를 검토하고 실패한 항목이 있으면 수정하도록 요청합니다.
5. 검토가 끝나면 <kbd>Esc</kbd>를 눌러 diff 화면을 닫습니다.

> [!NOTE]
> 모든 생성형 AI 도구와 마찬가지로 Copilot은 결정론적이 아니라 확률적으로 작동하므로 정확한 코드는 다를 수 있습니다. 한 가지 구현을 그대로 기대하기보다 동작을 검토합니다.

## 변경 내용 확인하기

코드를 읽고 작동한다고 가정하지 말고, Codespaces에서 포워딩된 브라우저로 업데이트된 UI를 확인하도록 Copilot에 요청합니다.

1. Copilot에 앱을 시작하도록 요청합니다.

   ```plaintext
   브라우저에서 별점 변경을 확인할 수 있도록 앱을 시작해 주세요. URL을 알려 주고 서버는 계속 실행 상태로 두세요.
   ```

2. Codespaces에서 포트 `4321`을 사용할 수 있다고 표시되면 **Open in Browser**를 선택합니다.
3. 게임 카드에 별점이 5점 만점으로 표시되는지 확인합니다.
4. Copilot으로 돌아가 시작한 서버를 중지하도록 요청합니다.

   ```plaintext
   시작한 개발 서버를 중지해 주세요.
   ```

## 첫 풀 리퀘스트 열기 및 병합하기

기능을 만들었으니 새 코드를 프로젝트에 병합할 풀 리퀘스트(PR)를 만듭니다.

1. 기본 에이전트에 변경 사항을 커밋하도록 요청합니다.

   ```plaintext
   검토한 별점 변경 사항을 적절한 커밋 메시지로 커밋해 주세요.
   ```

2. `/pr create`를 입력합니다. Copilot CLI는 PR을 만들 때 기존 커밋을 푸시하고 PR URL을 표시합니다.
3. <kbd>Command</kbd>(Mac) 또는 <kbd>Ctrl</kbd>(Windows/Linux)을 누른 상태에서 Copilot CLI에 표시된 URL을 선택해 PR을 엽니다.
4. 변경된 파일과 검사를 검토합니다.
5. 준비가 되면 **Merge pull request**를 선택한 뒤 병합을 확인합니다.
6. Codespace로 돌아가 `/exit`로 Copilot CLI를 종료합니다.
7. 로컬 `main`을 업데이트합니다.

   ```bash
   git checkout main
   git pull
   ```

## 요약 및 다음 단계

GitHub Copilot CLI를 사용해 첫 변경 사항을 전달했습니다. 이 레슨에서는 다음을 수행했습니다.

- 기능 브랜치에서 Copilot 대화를 시작했습니다.
- 에이전트에 게임 카드를 작게 변경하도록 요청했습니다.
- `/diff`로 변경 사항을 검토했습니다.
- 앱을 실행하고 포워딩된 브라우저에서 별점을 확인했습니다.
- 첫 풀 리퀘스트를 열고 병합했습니다.

다음으로 [필터링 이슈에서 시작해 Plan 및 Autopilot 모드][next-lesson]를 사용하여 더 큰 기능을 만듭니다.

## 리소스

- [GitHub Copilot CLI 정보][about-copilot-cli]
- [Copilot CLI 명령 참조][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
