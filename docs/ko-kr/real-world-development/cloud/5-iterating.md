---
title: "연습 5: GitHub Copilot 작업 반복 개선"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 이전 레슨: 에이전트 관리][previous-lesson] |
|:--|

## 작업 검토하기

이 실습에서는 GitHub Copilot과 함께 사용자 경험과 기능을 개선하는 여러 작업을 수행했습니다. Copilot에 코드 문서를 추가하고 디자인 팀을 위해 관련 게임 기능을 만들고, 고대비 및 밝은 모드 토글을 포함한 접근성 기능을 구현하도록 요청했습니다. Copilot의 작업을 살펴보고 필요하다면 피드백을 제공해 개선합니다. 그런 다음 나머지 팀원에게 검토를 요청합니다.

### 시나리오

생성형 AI를 사용하더라도 소프트웨어 설계와 DevOps의 기본은 바뀌지 않습니다. 생성된 코드를 검토하고 일반적인 DevOps 프로세스를 따라야 합니다. 이를 염두에 두고 팀에서 검토를 시작하기 전에 GitHub Copilot이 문서, 관련 게임 기능, 접근성 기능을 어떻게 만들었는지 검토합니다.

## GitHub Copilot cloud agent 보안

Copilot cloud agent는 감독 없이 비동기적으로 작업하므로 안전성을 보장하기 위한 보안 제한이 있습니다.

- Copilot은 리포지토리에 대한 읽기 권한만 가집니다. 코드 작성에 사용하는 브랜치에만 쓰기 권한이 있습니다.
- Cloud agent는 GitHub Actions에서 별도의 임시 작업 환경을 만듭니다.
- GitHub Actions 워크플로를 실행하려면 사람이 먼저 승인해야 합니다.
- MCP 서버를 포함한 [외부 리소스 액세스는 기본적으로 제한됩니다](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall).

## 생성된 문서 검토하기

먼저 GitHub Copilot cloud agent가 코드를 문서화한 첫 번째 풀 리퀘스트(PR)를 검토합니다. github.com의 일반적인 PR 인터페이스를 사용합니다.

> [!NOTE]
> PR을 살펴볼 때 GitHub Copilot이 방화벽에 의해 차단되었다는 경고가 표시될 수 있습니다. MCP 서버 호출을 포함한 외부 리소스 액세스가 기본적으로 제한되므로 정상적인 동작입니다. 원한다면 [Copilot cloud agent의 방화벽을 사용자 지정하거나 비활성화할 수 있습니다][agent-firewall].

1. github.com의 리포지토리로 돌아갑니다.
2. **Pull Requests**를 선택하여 PR 목록을 엽니다.
3. 제목이 **누락된 문서 추가** 또는 이와 비슷한 풀 리퀘스트를 엽니다.

> [!NOTE]
> Copilot이 아직 작업 중이면 PR에 **[WIP]** 표시가 있습니다. Copilot이 작업을 완료할 때까지 기다립니다. 몇 분 정도 걸릴 수 있으므로 잠시 쉬거나 지금까지 배운 내용을 돌아봐도 좋습니다.

4. PR이 준비되면 **Files changed** 탭을 선택하여 변경 사항을 검토합니다.

    ![Files changed 탭](../../../_images/shared-pr-files-changed.png)

5. 새로 추가된 TSDoc 문서 주석과 기타 문서를 포함해 코드 변경 사항을 살펴봅니다. 정확한 변경 사항은 다를 수 있습니다.

   검토할 때 TSDoc 문서 주석, TypeScript 규칙, 파일 머리 주석을 확인합니다. 이는 연습 2에서 살펴본 사용자 지정 지침 파일의 영향을 받은 내용입니다.

6. 업데이트를 검토하고 문제가 없으면 **Conversation** 탭으로 돌아가 아래로 스크롤합니다.
7. 승인을 기다리는 워크플로가 있다는 표시가 있는지 확인합니다.
8. 승인을 기다리는 워크플로가 있다면 **Approve and run workflows**를 선택합니다.

   ![워크플로 승인 및 실행](../../../_images/shared-approve-workflows.png)

9. 풀 리퀘스트 검사 섹션에서 워크플로가 실행 대기열에 추가되는 것을 확인합니다. 문제가 없다면 단일 Astro 앱에 대한 프로젝트 검사가 통과합니다. 완료까지 몇 분 정도 걸릴 수 있습니다.

## GitHub Copilot에 변경 요청하기

Copilot과 풀 리퀘스트에서 협업할 때 댓글로 Copilot을 태그할 수 있습니다. 팀원에게 요청하는 것처럼 PR 댓글이나 코드의 인라인 댓글에서 요청할 수 있습니다. Copilot은 댓글을 확인하고 변경 사항을 처리하기 위한 새 세션을 시작합니다. 결과가 비결정적이므로 특정 문구를 지정할 수는 없습니다. 다음과 같은 변경을 요청할 수 있습니다.

- 각 코드 파일 맨 위에 파일의 역할을 설명하는 주석을 추가합니다.
- TypeScript 및 Astro 파일에 TSDoc 문서 주석을 추가합니다.
- Astro 앱 구조를 설명하는 README를 만듭니다.

1. 위 아이디어 중 하나를 사용하거나 코드베이스에 추가하고 싶은 문서에 대해 다른 요청을 작성하고, **@copilot**을 태그해 댓글을 추가합니다.
2. **View Session**을 선택하여 Copilot의 작업을 확인합니다. Copilot이 업데이트를 위해 새 세션을 시작합니다.
3. **Back to pull request**를 선택해 PR로 돌아갈 수 있습니다.

    ![풀 리퀘스트로 돌아가기](../../../_images/ex7-back-to-pr.png)

4. Copilot이 변경을 마치면 PR에 새 커밋이 표시됩니다.
5. **Files changed** 탭을 선택하여 변경 사항을 검토합니다.

원하는 결과가 나올 때까지 계속 반복해도 좋습니다. 만족스러우면 초안 PR을 검토 준비 상태로 변경한 뒤 `main` 브랜치에 병합할 수 있습니다.

![검토 준비 상태로 변경하는 PR](../../../_images/ex7-ready-for-review.png)

## 관련 게임 기능 검토하기

게임 상세 페이지에 관련 게임을 표시하는 이슈를 해결하기 위해 Copilot이 만든 PR을 검토합니다.

1. GitHub.com의 리포지토리로 돌아갑니다.
2. **Pull Requests** 탭을 선택합니다.
3. 제목이 **게임 상세 페이지에 관련 게임 표시** 또는 이와 비슷한 PR을 선택합니다.
4. **Files changed** 탭을 선택하여 Copilot이 작성한 코드를 검토합니다.
5. 업데이트를 검토하고 문제가 없으면 **Conversation** 탭으로 돌아가 아래로 스크롤합니다.
6. 승인을 기다리는 워크플로가 있다는 표시가 있는지 확인합니다.
7. 승인을 기다리는 워크플로가 있다면 **Approve and run workflows**를 선택합니다.

   ![워크플로 승인 및 실행](../../../_images/shared-approve-workflows.png)

8. 풀 리퀘스트 검사 섹션에서 워크플로가 실행 대기열에 추가되는 것을 확인합니다. 문제가 없다면 단일 Astro 앱에 대한 프로젝트 검사가 통과합니다. 완료까지 몇 분 정도 걸릴 수 있습니다.
9. **선택 사항:** Codespace에서 해당 브랜치로 전환하여 관련 게임 기능을 직접 테스트할 수 있습니다. Codespace로 이동해 터미널을 열고 다음 명령을 실행합니다. `<branch-name>`을 Copilot이 만든 브랜치 이름(예: **copilot/fix-8**)으로 바꿉니다.

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot이 관련 게임 기능을 만들었습니다. 이전처럼 Copilot cloud agent에 업데이트를 요청하며 반복 작업할 수 있습니다. 예를 들어 표시할 관련 게임 수를 조정하거나, 사용자 지정 지침을 업데이트하기 **전에** 이슈에 할당된 작업에 파일 머리 주석과 TSDoc 문서 주석을 추가하도록 요청할 수 있습니다. **Conversation** 탭에 댓글을 추가하면 Copilot이 확인하고 새 세션을 시작합니다.

## 접근성 기능 검토하기

마지막으로 접근성 사용자 지정 에이전트를 사용해 구현한 기능을 검토합니다. PR에는 연습 3에서 요청한 고대비 모드와 연습 4에서 제어 센터를 통해 추가로 요청한 밝은 모드가 모두 포함되어야 합니다.

1. GitHub.com의 리포지토리로 돌아갑니다.
2. **Pull Requests** 탭을 선택합니다.
3. 제목이 **웹사이트에 고대비 모드 추가** 또는 이와 비슷한 PR을 선택합니다.

> [!NOTE]
> Copilot이 아직 작업 중이면 PR에 **[WIP]** 표시가 있습니다. Copilot이 작업을 완료할 때까지 기다립니다. 몇 분 정도 걸릴 수 있습니다.

4. **Files changed** 탭을 선택하여 Copilot이 작성한 코드를 검토합니다.
5. 구현 내용을 확인하며 특히 다음을 살펴봅니다.
   - 모드를 전환하는 토글 UI 컴포넌트
   - 사용자 기본 설정을 유지하기 위한 로컬 스토리지 사용
   - 고대비 및 밝은 모드를 위한 CSS 또는 스타일 변경
   - 접근성 속성(ARIA 레이블, 키보드 탐색 등)
   - 모드 전환을 관리하는 JavaScript/TypeScript 코드

6. 업데이트를 검토하고 문제가 없으면 **Conversation** 탭으로 돌아가 아래로 스크롤합니다.
7. 승인을 기다리는 워크플로가 있다는 표시가 있는지 확인합니다.
8. 승인을 기다리는 워크플로가 있다면 **Approve and run workflows**를 선택합니다.

   ![워크플로 승인 및 실행](../../../_images/shared-approve-workflows.png)

9. 풀 리퀘스트 검사 섹션에서 워크플로가 실행 대기열에 추가되는 것을 확인합니다. 문제가 없다면 단일 Astro 앱에 대한 프로젝트 검사가 통과합니다. 완료까지 몇 분 정도 걸릴 수 있습니다.
10. **선택 사항:** Codespace에서 해당 브랜치로 전환하여 접근성 기능을 직접 테스트할 수 있습니다. Codespace로 이동해 터미널을 열고 Copilot이 만든 브랜치 이름으로 `<branch-name>`을 바꾼 뒤 다음을 실행합니다.

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    그런 다음 애플리케이션을 시작하고 브라우저에서 고대비 및 밝은 모드 토글을 테스트하여 페이지를 새로 고친 뒤에도 설정이 유지되는지 확인합니다.

접근성 사용자 지정 에이전트는 접근성 모범 사례에 따라 기능을 구현하도록 Copilot을 안내합니다. 접근성과 관련해 우려 사항이나 개선점이 있다면 앞선 PR과 마찬가지로 댓글에서 **@copilot**을 태그해 업데이트를 요청할 수 있습니다.

## 선택 연습 - 위임 계속하기

실제 백로그 작업을 cloud agent에 맡기고 다른 작업에 집중할 수 있습니다. 이 습관을 익히려면 리포지토리에 이슈를 몇 개 더 등록하고 Copilot에 할당합니다. 다음과 같은 작업을 요청할 수 있습니다.

- 게임 상세 페이지에 후원자 관심 등록 양식을 추가합니다.
- 게임 목록 페이지에 페이지 매김을 구현합니다.
- 데이터 접근 도우미에 입력 검증과 오류 처리를 추가합니다.

## 요약

Cloud agent 과정을 완료했습니다. 이 레슨에서는 다음을 수행했습니다.

- **이 리포지토리에 포함된 사용자 지정 지침 파일을 검토했습니다.** 이후 cloud agent 결과에 미친 영향을 확인할 수 있습니다.
- **GitHub Copilot cloud agent에 이슈를 할당했습니다.** 작업 환경을 설정하고 비동기적으로 계획과 구현을 진행하는 과정을 살펴봤습니다.
- **접근성 사용자 지정 에이전트를 만들고 사용했습니다.** 고대비와 밝은 모드 토글을 추가했습니다.
- **Copilot agents 페이지를 제어 센터로 사용했습니다.** 작업이 진행되는 동안 접근성 세션을 모니터링하고 안내했습니다.
- **cloud agent가 만든 풀 리퀘스트를 검토하고 반복 개선했습니다.** `@copilot`을 태그해 변경을 요청하고 워크플로를 승인했습니다.

## 검토 및 다음 단계

Cloud agent 과정을 완료했습니다. 계속 학습하려면 다른 환경을 다루는 다음 워크숍을 살펴봅니다.

- 🖥️ **[VS Code 과정](../../vscode/)** — IDE에서 Copilot Chat 에이전트 모드와 MCP 통합을 살펴봅니다.
- 💻 **[CLI 과정](../../cli/)** — Plan 및 Autopilot 모드, 에이전트 스킬, 사용자 지정 QA 에이전트, Playwright MCP, Agent Merge, 선택적 `/delegate`를 사용해 터미널에서 변경 사항 두 개를 검토하고 전달합니다.

본인의 리포지토리에서 다음 작업을 시도할 수도 있습니다.

- 리팩터링이나 테스트 범위 확장 이슈를 cloud agent에 할당합니다.
- 다른 작업 분야를 위한 사용자 지정 에이전트를 만듭니다.
- 다른 기술 스택에 맞게 `copilot-setup-steps.yml`을 구성합니다.

## 리소스

- [GitHub Copilot][github-copilot]
- [Copilot agent 정보][copilot-agents]
- [GitHub 이슈를 Copilot에 할당하기][assign-issue]
- [Copilot cloud agent 설정 워크플로 모범 사례][cloud-agent-best-practices]
- [Copilot cloud agent 방화벽 구성][agent-firewall]

---

| [← 이전 레슨: 에이전트 관리][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
