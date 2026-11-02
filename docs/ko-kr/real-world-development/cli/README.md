---
slug: ko-kr/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI 모드와 사용자 지정 기능, MCP 도구, 풀 리퀘스트 자동화를 살펴보며 Tailspin Toys의 변경 사항 두 가지를 빌드하고 검증하고 전달합니다."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]**는 에이전트형 코딩 도우미인 GitHub Copilot을 터미널에서 사용할 수 있게 합니다. 코드베이스를 살펴보고 코드를 생성하고 명령을 실행하며 외부 도구에 연결하는 작업을 명령줄에서 처리하므로 그래픽 편집기로 전환하지 않고 작업 흐름을 이어 갈 수 있습니다.

이 워크숍은 하나로 이어지는 Tailspin Toys 작업 흐름을 따라갑니다.

1. GitHub Codespaces에서 프로젝트를 준비하고 Copilot CLI를 설치한 뒤 기본 사용법을 익힙니다.
2. 게임 카드에 별점을 추가하고 브라우저에서 결과를 검토한 뒤 첫 풀 리퀘스트(PR)를 직접 병합합니다.
3. 필터링 이슈에서 시작해 Plan 모드에서 접근 방식을 정하고 Autopilot 모드로 구현한 뒤 Interactive 모드에서 검토합니다.
4. 리포지토리 지침을 업데이트하고 필터링 작업에 적용합니다.
5. 기존 `quality-checks` 스킬을 사용자 지정하고 프로젝트 검사를 실행합니다.
6. Playwright Model Context Protocol(MCP) 서버를 추가하고 브라우저에서 필터링 동작을 살펴봅니다.
7. 품질 보증(QA) 사용자 지정 에이전트를 만들어 요구 사항, 테스트 범위, 검증 근거를 검토합니다.
8. 필터링 변경 사항 전체를 검토하고 Agent Merge로 필터링 PR을 관리합니다.
9. 컨텍스트, 모델, 공유, cloud agent에 선택적으로 위임하는 방법을 비롯해 유용한 슬래시 명령을 살펴봅니다.

워크숍에 집중할 수 있도록 PR 두 개를 만듭니다. 첫 번째는 별점 변경이며, 두 번째는 지침 업데이트, 스킬 업데이트, QA 프로필, 테스트를 포함한 필터링 변경입니다. 필터링과 품질 작업은 하나의 대화와 브랜치에서 진행하므로 각 도구를 익히면서 작업을 이어 갈 수 있습니다.

## 레슨

| 레슨 | 주제 | 설명 |
| ------ | ----- | ----------- |
| [0. 사전 준비][ex0] | 설정 | 리포지토리와 Codespace를 준비합니다 |
| [1. Copilot CLI 설치][ex1] | 설치 | Copilot CLI를 설치하고 인증한 뒤 기본 사용법을 익힙니다 |
| [2. 별점 추가: 빠른 성과][ex2] | 첫 변경 | 기존 별점과 값이 없는 경우의 대체 표시를 추가한 뒤 첫 PR을 병합합니다 |
| [3. 에이전트 모드: Plan 및 Autopilot][ex3] | 에이전트 모드 | 이슈에서 기능을 계획하고 Autopilot으로 구현한 뒤 Interactive 모드에서 검토합니다 |
| [4. 사용자 지정 지침으로 Copilot 안내][ex4] | 컨텍스트 | 지침을 살펴보고 업데이트한 뒤 필터링 작업에 적용합니다 |
| [5. quality-checks 스킬 사용자 지정 및 사용][ex5] | 반복 가능한 검사 | 기존 스킬을 살펴보고 결과 보고 형식을 바꾼 뒤 실행합니다 |
| [6. Playwright MCP로 기능 검증][ex6] | 브라우저 확인 | CLI에서 MCP를 구성하고 필터링 동작을 살펴봅니다 |
| [7. QA 에이전트 만들기 및 사용][ex7] | 요구 사항과 테스트 범위 | 전문 프로필을 만들고 선택한 뒤 최종 검증 근거를 수집합니다 |
| [8. 기능 PR 만들기 및 병합][ex8] | 검토 및 병합 | 변경 사항 전체를 검토하고 PR을 만든 뒤 Agent Merge를 사용합니다 |
| [9. GitHub Copilot CLI의 슬래시 명령][ex9] | CLI 기능 | 컨텍스트, 모델, 공유, cloud agent에 선택적으로 위임하는 방법을 살펴봅니다 |
| [10. 마무리 및 다음 단계][ex10] | 요약 | 작업 흐름, 재사용 가능한 사용자 지정 기능, 추가 리소스를 검토합니다 |
| [선택 사항: Foundry 통합][foundry] | 호스트된 에이전트 | 모델을 준비하고 카탈로그에 기반한 에이전트를 배포한 뒤 웹사이트에 연결합니다 |

## 사전 준비

이 워크숍에 참여하기 전에 다음을 준비합니다.

- [ ] 활성 **Copilot Student, Pro, Pro+, Business 또는 Enterprise** 플랜이 있는 GitHub 계정
- [ ] 리포지토리와 Codespace를 만들 수 있는 권한
- [ ] 터미널 또는 명령줄 사용에 대한 기본 지식

> [!TIP]
> 유료 플랜이 없습니까? 인증된 학생은 [GitHub Education][student-plan]을 통해 GitHub Copilot을 무료로 사용할 수 있습니다. **Copilot Student** 플랜에는 이 워크숍에서 사용하는 에이전트, MCP, 코드 검토, Copilot CLI 기능이 포함되어 있습니다.

> [!NOTE]
> Copilot Business 또는 Copilot Enterprise를 사용하는 경우 관리자가 Copilot CLI를 활성화했는지 확인합니다.

## 시작하기

**[사전 준비부터 시작 →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
