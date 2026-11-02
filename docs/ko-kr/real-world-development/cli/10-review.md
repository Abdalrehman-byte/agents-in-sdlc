---
title: "레슨 10 - 마무리 및 다음 단계"
description: "Copilot CLI 작업 흐름, 풀 리퀘스트 두 개, 재사용 가능한 사용자 지정 기능, 추가 리소스를 돌아봅니다."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

하나로 이어지는 Tailspin Toys 작업 흐름에서 GitHub Copilot CLI를 사용했습니다. 이 레슨에서는 다음을 수행했습니다.

- Codespace를 준비하고 Copilot CLI를 설치해 프로젝트를 살펴본 뒤 미리 등록된 필터링 이슈를 찾았습니다.
- 별점을 추가하고 포워딩된 브라우저에서 결과를 확인한 뒤 첫 풀 리퀘스트(PR)를 직접 병합했습니다.
- 필터링 이슈에서 시작해 Plan 모드로 접근 방식을 정하고 Autopilot 모드로 구현한 뒤 Interactive 모드에서 검토했습니다.
- 사용자 지정 지침으로 에이전트를 안내하고 `quality-checks` 스킬을 사용자 지정해 단위 테스트, 린트, 타입 검사를 실행했습니다.
- Playwright Model Context Protocol(MCP) 서버를 추가하고 실제 브라우저에서 필터링 기능을 살펴봤습니다.
- 품질 보증(QA) 사용자 지정 에이전트를 만들고 선택하여 요구 사항, 테스트 범위, 스킬 결과, 브라우저 검증 근거를 확인했습니다.
- 필터링 변경 사항 전체를 검토하고 Agent Merge를 승인해 필터링 PR을 병합했습니다.
- 컨텍스트, 모델, 공유, cloud agent에 선택적으로 위임하기 위한 슬래시 명령을 살펴봤습니다.

## 전달한 결과

워크숍에서는 업데이트된 `main`에서 시작하는 브랜치 두 개를 사용해 PR 두 개를 완료합니다.

1. **별점:** 기존 `starRating`과 별점이 없는 상태를 게임 카드에 표시합니다.
2. **필터링 및 품질 작업 흐름:** 필터링을 구현하고 지침을 업데이트해 기능에 적용하고 `quality-checks` 보고서를 사용자 지정하고 QA 프로필을 만들고 관련 테스트를 추가합니다.

필터링을 계획하는 단계부터 PR을 여는 단계까지 동일한 대화와 브랜치를 사용했습니다. 워크숍을 간소화하기 위해 해당 작업을 하나의 PR에 포함했습니다.

## 다양한 방식으로 검증하기

자동 테스트, 브라우저에서 직접 확인, MCP를 사용한 Copilot의 브라우저 탐색 등 여러 방법으로 코드를 검증했습니다. `quality-checks` 스킬은 단위 테스트, 린트, 타입 검사를 실행하고 새 형식에 따라 결과를 보고했습니다. QA는 PR 전에 요구 사항과 테스트 범위를 검토하고 해당 결과를 함께 정리했습니다.

테스트는 실제로 부족한 범위를 보완해야 합니다. QA 실행에서 테스트를 추가하지 않아도 되는 경우가 있을 수 있습니다. 병합을 승인하기 전에 코드와 근거를 검토하고 변경 사항이 있으면 관련 근거를 다시 확인합니다.

## 모범 사례

Copilot에 제공하는 컨텍스트와 도구가 결과에 영향을 줍니다. 이 워크숍에서는 지침을 업데이트하고 스킬을 사용자 지정하고 QA 프로필을 만들고 MCP 서버를 구성했습니다. 대화 전반에서 이러한 사용자 지정 기능을 재사용하고 팀의 요구 사항에 맞게 조정합니다. 지침은 표준을 정하고, 스킬은 반복 작업을 안내하고, 사용자 지정 에이전트는 전문 역할을 정의하고, MCP 서버는 외부 도구를 연결합니다. 에이전트의 요약뿐 아니라 실제 변경 사항과 도구 결과를 검토합니다.

작업에 맞게 **모드와 모델**을 선택합니다. **Plan**은 구현 전에 접근 방식을 검토할 때, **Interactive**는 범위가 명확한 변경에서 과정을 함께 진행할 때, **Autopilot**은 범위가 명확한 작업을 맡길 때 사용합니다. 일반적인 편집에는 더 빠른 모델을, 복잡한 작업에는 더 강력한 모델을 선택합니다.

인프라만큼 컨텍스트도 중요합니다. 무엇을, 왜, 어떻게 만들지 명확하게 설명하면 결과가 달라집니다.

## 더 살펴보기

핵심 작업 흐름을 마쳤습니다. Copilot CLI의 추가 기능도 살펴볼 수 있습니다.

- `/review`를 사용해 코드 검토 에이전트에 변경 사항을 분석하도록 요청합니다.
- `/rubber-duck`을 사용해 문제를 논의하고 다른 관점을 얻습니다.
- `/fleet`으로 독립적인 하위 작업을 병렬로 조율합니다.
- `/worktree`로 별도 작업을 격리합니다.
- `/delegate`로 Copilot cloud agent에 작업을 보냅니다.

## 다음 단계

어떤 도구든 계속 사용해야 더 잘 활용할 수 있습니다. 프로덕션 코드나 취미 프로젝트, 또는 오래전부터 만들고 싶었던 작은 앱에서 도구를 사용해 봅니다. 팀과 배운 내용을 공유하고 서로의 경험에서 배웁니다. 문서도 계속 살펴봅니다.

터미널에서 Tailspin Toys를 확장하려면 선택 사항인 [Foundry Backer Concierge 시리즈][foundry]를 진행합니다. 다른 환경을 비교하려면 [VS Code 워크숍][vscode], [GitHub Copilot app 워크숍][app], [Copilot cloud agent 워크숍][cloud]을 살펴봅니다.

## 리소스

- [GitHub Copilot CLI 정보][about-cli]
- [Copilot CLI 명령 참조][cli-reference]
- [Copilot CLI 사용자 지정][customize-cli]
- [Copilot CLI로 풀 리퀘스트 관리][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
