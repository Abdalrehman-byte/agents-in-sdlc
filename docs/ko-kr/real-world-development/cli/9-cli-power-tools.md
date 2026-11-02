---
title: "레슨 9 - GitHub Copilot CLI의 슬래시 명령"
description: "컨텍스트 관리, 모델 선택, 세션 공유, cloud agent에 선택적으로 위임하는 슬래시 명령을 살펴봅니다."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

다른 CLI 도구와 마찬가지로 GitHub Copilot CLI에도 다양한 슬래시 명령이 있습니다. 이 명령으로 고급 기능, 내부 정보, 추가 구성 옵션을 사용할 수 있습니다. `/diff`, `/mcp`, `/skills`, `/agent`, `/pr` 등을 이미 사용했습니다. 이제 몇 가지 유용한 명령을 살펴봅니다.

이 레슨에서는 다음을 수행합니다.

- `/context`와 `/compact`로 Copilot이 대화 컨텍스트를 관리하는 방식을 살펴봅니다.
- `/model`로 사용할 수 있는 모델을 확인합니다.
- `/share`로 세션을 내보내거나 공유하는 방법을 알아봅니다.
- 병렬 작업, 워크트리, cloud agent 위임을 위한 선택적 명령을 살펴봅니다.

## 시나리오

CLI 핵심 작업 흐름을 마쳤습니다. 이제 컨텍스트 관리, 모델 전환, 세션 공유, [Copilot cloud agent][about-cloud-agent]에 선택적으로 위임하는 방법 등 추가 기능을 살펴봅니다.

## Copilot CLI 컨텍스트 살펴보기

크거나 복잡한 작업을 수행하면 모델의 최대 컨텍스트 창에 도달할 수 있습니다. Copilot CLI는 필요할 때 대화를 자동으로 압축하며, 슬래시 명령을 사용해 직접 확인하거나 압축할 수도 있습니다.

1. Codespace로 돌아갑니다. Copilot CLI가 열려 있지 않다면 리포지토리 루트에서 시작합니다.
2. 다음을 입력합니다.

   ```plaintext
   /context
   ```

3. 모델, 현재 토큰 사용량, 시스템 지침, 도구, 메시지, 사용 가능한 공간에 컨텍스트가 어떻게 나뉘는지 살펴봅니다.
4. 대화를 압축합니다.

   ```plaintext
   /compact
   ```

5. 다시 `/context`를 입력해 결과를 비교합니다. 대화가 이미 짧다면 큰 차이가 없을 수 있습니다.

> [!NOTE]
> 컨텍스트 창이 가득 차면 Copilot CLI가 자동으로 컨텍스트를 압축합니다. `/compact`를 사용하면 압축 시점을 직접 정할 수 있습니다. 관련 없는 작업으로 전환하면서 새 대화를 시작하려면 `/clear` 또는 `/new`를 사용합니다.

## 모델 선택하기

모델마다 강점이 다르고 개발자마다 선호하는 모델이 다릅니다. Copilot CLI에서 사용할 수 있는 모델을 살펴보고 원하는 모델을 선택할 수 있습니다.

1. 다음을 입력합니다.

   ```plaintext
   /model
   ```

2. 사용 가능한 모델과 사용 정보를 살펴봅니다.
3. 현재 모델을 유지하거나 다른 모델을 선택합니다. 목록을 닫으려면 <kbd>Esc</kbd>를 누릅니다.

## 세션 공유하기

팀으로 협업하고 배운 내용을 공유하면 모두가 AI 도구를 더 잘 사용할 수 있습니다. `/share` 명령으로 세션을 Markdown 또는 HTML 파일로 내보내고 공유 가능한 링크를 만들거나 GitHub gist에 게시할 수 있습니다.

1. `/help`를 입력하고 설치된 버전의 `/share` 옵션을 살펴봅니다.
2. 세션을 공유하려면 필요에 맞는 대상을 선택합니다. 예를 들어 `/share file`은 Markdown 파일을 로컬에 내보냅니다.
3. 내보낸 내용을 다른 사람에게 보내거나 게시하기 전에 검토합니다. 세션 내보내기 파일에 프롬프트, 응답, 프로젝트 세부 정보가 포함될 수 있습니다.

링크나 gist를 게시하는 것은 선택 사항입니다. 팀에서 공유하지 않기로 한 리포지토리 또는 대화 내용을 게시하지 않습니다.

## 선택 사항: 병렬 작업 또는 위임하기

핵심 워크숍을 마쳤습니다. Copilot CLI에는 더 큰 작업을 위한 명령도 있습니다.

- `/fleet`은 서로 독립적인 하위 작업을 하위 에이전트에 나눠 병렬로 실행할 수 있습니다.
- `/worktree`는 별도 작업을 위한 격리된 Git 워크트리를 만들 수 있습니다.
- `/delegate`는 비동기적으로 작업하고 풀 리퀘스트를 열 수 있는 Copilot cloud agent에 작업을 보낼 수 있습니다.

이 명령을 사용하면 워크트리나 원격 작업이 추가될 수 있으므로 선택 사항입니다. 사용해 보려면 범위가 명확한 새 작업에서 시작하고 일반적인 워크플로에 따라 결과를 검토합니다. 비동기 에이전트 작업을 더 자세히 알아보려면 [cloud agent 워크숍][cloud-workshop]을 진행합니다.

## 요약 및 다음 단계

Copilot CLI의 슬래시 명령을 사용해 도구를 구성하고 세션을 공유하고 내부 상태를 확인했습니다. 이 레슨에서는 다음을 수행했습니다.

- `/context`와 `/compact`로 Copilot이 대화 컨텍스트를 관리하는 방식을 살펴봤습니다.
- `/model`로 사용할 수 있는 모델을 확인했습니다.
- `/share`로 세션을 내보내거나 공유하는 방법을 알아봤습니다.
- 병렬 작업, 워크트리, cloud agent에 선택적으로 위임하는 방법을 살펴봤습니다.

Copilot CLI에서 사용할 수 있는 명령과 기능은 더 많습니다. 다음으로 [지금까지 배운 내용과 다음 단계][next-lesson]를 검토합니다.

## 리소스

- [Copilot CLI 명령 참조][cli-reference]
- [Copilot CLI의 컨텍스트 관리][context-management]
- [Copilot cloud agent 정보][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
