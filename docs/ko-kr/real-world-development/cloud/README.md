---
slug: ko-kr/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[**GitHub Copilot cloud agent**](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)는 클라우드에서 비동기적으로 작업합니다. GitHub에서 작업을 할당하면 cloud agent가 백그라운드에서 리포지토리를 살펴보고 변경한 다음 풀 리퀘스트를 엽니다. 그동안 사용자는 다른 작업을 진행할 수 있습니다.

이 연습에서는 cloud agent가 따를 사용자 지정 지침을 추가한 다음 GitHub 이슈를 할당하여 작업을 구현하도록 합니다. 사용자 지정 에이전트를 검토하고 사용하여 접근 방식을 조정하고, 에이전트 대시보드에서 세션을 모니터링하고 안내한 뒤, 에이전트가 만든 풀 리퀘스트를 검토하고 반복해서 개선합니다.

## 연습

| 연습 | 주제 | 설명 |
|----------|-------|-------------|
| [0. 사전 준비][ex0] | 설정 | 리포지토리를 만들고 codespace를 준비합니다 |
| [1. 사용자 지정 지침][ex1] | 컨텍스트 | cloud agent가 따를 사용자 지정 지침을 추가합니다 |
| [2. Cloud agent][ex2] | 비동기 에이전트 | 이슈를 GitHub Copilot cloud agent에 할당합니다 |
| [3. 사용자 지정 에이전트][ex3] | 특화된 에이전트 | 사용자 지정 에이전트를 검토하고 사용합니다 |
| [4. 에이전트 관리][ex4] | 모니터링 | 에이전트 세션을 모니터링하고 안내합니다 |
| [5. 반복 개선][ex5] | 검토 | PR을 검토하고 Copilot의 작업을 반복 개선한 뒤 다음 단계를 선택합니다 |

## 사전 준비

이 워크숍에 참여하기 전에 다음을 준비합니다.

- [ ] 활성 **Copilot Student, Pro, Pro+, Business 또는 Enterprise** 플랜이 있는 GitHub 계정
- [ ] 계정 또는 조직에서 GitHub Copilot cloud agent가 활성화되어 있어야 합니다.

> [!TIP]
> 유료 플랜이 없습니까? 인증된 학생은 [GitHub Education][callout-student-plan-education]을 통해 GitHub Copilot을 무료로 사용할 수 있습니다. **Copilot Student** 플랜에는 이 워크숍에서 사용하는 agent, MCP, 코드 검토, Copilot CLI 기능이 포함되어 있으므로 모든 실습 과정을 완료할 수 있습니다.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent는 **Copilot Student, Pro, Pro+, Business, Enterprise** 플랜에서 사용할 수 있습니다. Copilot Free 플랜에서는 사용할 수 없습니다. Copilot Business 또는 Enterprise를 사용하는 경우 관리자가 조직에서 cloud agent를 활성화해야 합니다.

> [!NOTE]
> 이 과정에서는 MCP를 다루지 않습니다. Copilot에서 MCP 서버를 사용하는 방법을 알아보려면 [CLI 과정][cli] 또는 [VS Code 과정][vscode]을 참조합니다.

## 시작하기

**[연습 0: 사전 준비부터 시작 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
