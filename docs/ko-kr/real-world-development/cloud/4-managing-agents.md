---
title: "연습 4 - 에이전트 모니터링 및 관리"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

앞선 두 연습에서는 Copilot cloud agent에 사용자 경험을 개선하고 기능을 추가하는 세 가지 작업을 요청했습니다. Cloud agent는 비동기적이고 자율적으로 작동하지만, 작업을 모니터링하는 것도 중요합니다.

GitHub.com의 [agents 페이지][agents-page] 등 cloud agent에 할당한 작업을 관리하는 도구가 다양하게 있습니다. 이 제어 센터에서 열린 풀 리퀘스트(PR)가 있는 모든 에이전트 작업을 볼 수 있습니다. 수행한 작업을 살펴보고, 진행 중인 세션에 추가 지침을 보내 방향을 조정할 수도 있습니다.

이 레슨에서는 다음을 수행합니다.

- agents 페이지에서 cloud agent 작업을 모니터링합니다.
- 진행 중인 세션에 추가 기능을 요청합니다.

## 시나리오

접근성 에이전트가 고대비 모드를 만들도록 요청한 뒤, 팀은 밝은 모드도 함께 추가하기로 했습니다. 사이트 스타일을 업데이트하고 토글 기능을 만드는 중이므로 밝은 모드도 포함하는 편이 자연스럽습니다. 에이전트에 추가 지침을 보내 고대비 모드와 밝은 모드를 모두 구현하도록 합니다.

## Copilot cloud agent 작업 검토하기

Copilot cloud agent에 할당된 작업의 현재 상태를 확인합니다.

1. [Copilot agents 페이지](https://github.com/copilot/agents)로 이동합니다.
2. 기본 창과 왼쪽 창에 표시되는 작업 목록을 살펴봅니다. 다음과 같이 Copilot에 할당한 작업이 표시됩니다.
   - 코드베이스 문서 업데이트
   - 게임 상세 페이지에 관련 게임 표시
   - 웹사이트에 고대비 모드 추가
3. 실행 중인 작업 하나를 선택하고 Copilot이 수행한 작업을 검토합니다. 다음 작업을 수행할 수 있습니다.
   - 리포지토리에서 코드 확인
   - Copilot 작업 환경 생성
   - MCP 서버 설정
   - 할당된 작업 완료를 위한 여러 단계 수행

> [!NOTE]
> 표시되는 정확한 단계는 Copilot의 작업 상태와 접근 방식에 따라 다릅니다.

4. 오른쪽에 나타나는 풀 리퀘스트(PR) 창도 살펴봅니다. 여기에서 PR과 변경된 파일을 확인할 수 있습니다.

## Cloud agent에 추가 지침 보내기

현재 진행 중인 작업을 확인했으니, 에이전트가 고대비 모드를 구현하는 동안 밝은 모드 토글도 추가하도록 요청합니다.

1. 고대비 모드 작업을 진행 중인 세션을 선택합니다. 정확한 제목은 Copilot이 선택한 이름과 작업 상태에 따라 달라집니다.

    ![제어 센터의 접근성 세션](../../../_images/ex6-accessibility-session.png)

2. 세션이 설정을 마치고 작업을 시작할 때까지 몇 분간 확인합니다. 다음과 비슷한 메시지가 표시되면 작업이 시작된 것입니다.
3. **Steer active session while Copilot is working** 대화 상자에 다음 프롬프트를 입력합니다.

    ```plaintext
    고대비 모드를 만드는 김에 밝은 모드도 추가해 주세요. 사용자가 원하는 표시 모드를 선택할 수 있도록 밝은 모드 토글도 제공해야 합니다.
    ```

    ![agents 페이지의 cloud agent 작업에서 활성 세션 안내 대화 상자를 강조한 스크린샷](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd>를 눌러 프롬프트를 보냅니다.
5. Copilot이 요청을 확인하고 작업 흐름에 반영하는지 살펴봅니다.

## Copilot이 작업하도록 두기

이전과 마찬가지로 Copilot이 업데이트된 작업을 시작합니다. 메시지를 보낸 시점에 진행 중이던 단계를 마친 다음 새로운 요청을 작업 흐름에 반영합니다.

이 작업도 몇 분 정도 걸리므로, 그동안 지금까지 배운 내용을 돌아볼 수 있습니다.

## 요약 및 다음 단계

Copilot cloud agent 작업을 모니터링하고 안내하는 중심 공간인 Copilot agents 페이지를 살펴봤습니다. 이 제어 센터에서 진행 중이거나 완료된 모든 작업을 추적하고 수행 내용을 검토하며, 진행 중인 세션의 방향을 조정할 수 있습니다.

이 연습에서는 다음을 수행했습니다.

- 제어 센터와 agents 페이지를 살펴보며 cloud agent 작업을 모니터링했습니다.
- 진행 중인 세션에 추가 기능을 요청했습니다.

Copilot이 접근성 기능 구현을 계속하는 동안, 다음으로 [Copilot이 만든 풀 리퀘스트를 반복 개선합니다][next-lesson].

## 리소스

- [Copilot agents 페이지][agents-page]
- [사용자 지정 에이전트 정보][custom-agents]

---

| [← 이전 레슨: 사용자 지정 에이전트][previous-lesson] | [다음 레슨: Copilot 작업 반복 개선 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
