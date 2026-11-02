---
title: "실제 개발"
slug: ko-kr/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

현실적인 시나리오와 완성된 애플리케이션, 공유 백로그를 바탕으로 소프트웨어 개발 수명 주기 전반에서 GitHub Copilot을 살펴봅니다. 반복적으로 변경을 계획하고 코드를 작성하고 동작을 테스트하며 풀 리퀘스트를 검토하고 에이전트로 작업을 관리합니다.

## 시나리오

개발자 테마의 보드게임 크라우드 펀딩을 제공하는 가상의 회사 Tailspin Toys에서 새 개발자로 일한다고 가정합니다. 팀의 백로그는 GitHub 이슈로 등록되어 있어 바로 작업을 시작할 수 있습니다. 백로그에는 필터링 및 페이지 매김과 같은 기능 작업과 접근성 및 코딩 표준과 같은 품질 개선 작업이 포함되어 있습니다.

## 작업 환경 선택하기

GitHub Copilot은 다양한 작업 환경에서 사용할 수 있습니다. 원하는 개발 방식에 맞는 환경을 선택합니다. 각 워크숍은 자체 설정 절차로 시작하며 동일한 Tailspin Toys 시나리오를 사용합니다.

### [GitHub Copilot CLI][cli]

터미널에서 에이전트형 도우미를 사용합니다. Plan 및 Autopilot 모드, 지침, 스킬, 사용자 지정 에이전트, Playwright MCP, Agent Merge, 실용적인 슬래시 명령을 살펴봅니다.

### [GitHub Copilot app][app]

병렬 에이전트 세션을 실행하고 세션 모드를 전환하며 캔버스에서 협업하고 데스크톱 앱에서 GitHub 이슈와 풀 리퀘스트를 관리합니다.

### [GitHub Copilot cloud agent][cloud]

GitHub 이슈를 비동기 에이전트에 할당하고 작업을 안내하며 진행 상황을 모니터링하고 에이전트가 여는 풀 리퀘스트를 검토합니다.

### [Visual Studio Code][vscode]

Visual Studio Code와 GitHub Codespaces에서 GitHub Copilot을 사용합니다. 편집기를 벗어나지 않고 Copilot Chat 에이전트 모드, MCP 서버, 사용자 지정 에이전트를 활용합니다.

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
