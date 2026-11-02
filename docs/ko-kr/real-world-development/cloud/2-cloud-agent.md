---
title: "연습 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 이전 레슨: 사용자 지정 지침][previous-lesson] |
|:--|

기술 부채로 어려움을 겪지 않는 조직은 거의 없습니다. 해결되지 않은 보안 문제, 업데이트가 필요한 레거시 코드, 구현할 시간이 없어 백로그에 남은 기능 요청 등이 그 예입니다. GitHub Copilot cloud agent는 코드를 업데이트하고 기능을 추가하는 등의 작업을 자율적으로 수행합니다. 작업을 마치면 사람이 검토할 수 있도록 초안 PR을 엽니다. 이를 통해 지루한 작업을 위임하고 개발 프로세스를 빠르게 진행하여 더 큰 과제에 집중할 수 있습니다.

이 연습에서는 Copilot cloud agent를 다음과 같이 활용합니다.

- 코드 생성 환경을 사용자 지정합니다.
- 작업이 안전하게 수행되는지 확인합니다.
- 범위가 명확한 이슈의 중요성을 살펴봅니다.
- 이슈를 Copilot에 할당합니다.

## 시나리오

Tailspin Toys에는 해결할 기술 부채가 있습니다. 초기 웹사이트를 만든 계약 업체가 문서를 충분히 작성하지 않았습니다. 먼저 애플리케이션에서 내보내는 모든 함수에 TSDoc 문서 주석을 추가하려고 합니다.

또한 디자인 팀은 게임 검색 환경을 개선하려고 합니다. 각 게임 상세 페이지에 관련 게임, 즉 같은 카테고리에 속한 다른 게임을 표시하려고 합니다. 세련된 구현이 필요한 것은 아니며, 사용자 경험(UX) 테스트에 사용할 수 있는 기능을 원합니다. 현재는 우선순위가 더 높은 다른 이슈가 있어 이 작업이 보류되어 있습니다.

이처럼 우선순위가 밀리기 쉬운 작업은 Copilot cloud agent에 맡기기 좋습니다. 개발자는 다른 작업에 집중하고, Copilot cloud agent는 비동기적으로 작업을 수행합니다. 나중에 돌아와 Copilot이 만든 결과를 검토하고 요구 사항에 맞는지 확인하면 됩니다.

## GitHub Copilot cloud agent 소개

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)는 사람 개발자와 비슷하게 백그라운드에서 작업을 수행할 수 있습니다. 사람 개발자에게 하듯 [GitHub 이슈를 Copilot에 할당][assign-issue]하는 방식으로 작업을 요청할 수 있습니다. 작업을 할당하면 Copilot은 진행 상황을 추적하기 위한 초안 풀 리퀘스트를 만들고 환경을 설정한 뒤 작업을 시작합니다. 작업이 진행 중일 때나 완료된 뒤에 Copilot 세션을 살펴볼 수 있습니다. 결과를 검토할 준비가 되면 풀 리퀘스트에서 사용자를 태그합니다.

## 범위가 명확한 지침의 중요성

GitHub Copilot에는 마법 같은 기능이 없습니다. 생각하는 과정을 건너뛸 수는 없습니다. 겉보기에는 간단한 작업도 자세히 살펴보면 복잡할 수 있으므로 [Copilot cloud agent에 할당하는 작업의 범위를 신중하게 정합니다][cloud-agent-best-practices]. AI 페어 프로그래머처럼 단계별로 접근하고 배우고 실험하며 조정합니다. 생성형 AI를 사용하더라도 소프트웨어 개발의 기본은 달라지지 않습니다.

## 이 리포지토리의 사용자 지정 지침

이전 연습에서 사용자 지정 지침과 Copilot을 안내하는 방식을 살펴봤습니다. 사용자 지정 지침 연습을 완료했다면 `.github/copilot-instructions.md`를 사용해 본 적이 있을 것입니다. 아직 살펴보지 않았다면 Copilot cloud agent에 작업을 할당하기 전에 이 리포지토리에 포함된 지침 파일을 읽어 봅니다. 나중에 영향을 확인할 수 있도록 읽기만 합니다.

GitHub 웹 UI 또는 실행 중인 Codespace에서 다음 파일을 엽니다.

- `.github/copilot-instructions.md` - TypeScript 규칙과 TSDoc 문서 주석 요구 사항이 포함된 **Code standards** 섹션을 검토합니다.
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter가 `**/*.test.ts` 파일에 지침을 적용하는 방식을 살펴봅니다.

다음 섹션에서 `코드에 문서가 부족합니다` 이슈를 cloud agent에 할당할 때 풀 리퀘스트에 TSDoc 문서 주석, TypeScript 규칙, 파일 머리 주석이 반영되는지 확인합니다. 이후 연습에서 풀 리퀘스트를 검토할 때 다시 살펴봅니다.

## Copilot cloud agent 개발 환경 설정하기

코드를 작성하려면 일반적으로 적절한 환경을 마련하고 초기 상태를 확인하기 위한 설정 스크립트를 실행해야 합니다. Copilot은 소프트웨어 엔지니어와 비슷한 방식으로 작업하므로 Copilot에 작업을 할당할 때도 마찬가지입니다.

Cloud agent는 작업 환경으로 [GitHub Actions][github-actions]를 사용합니다. 작업을 시작하기 전에 실행할 [특수 설정 워크플로][setup-workflow]를 `.github/workflows/copilot-setup-steps.yml` 파일에 구성하여 필요한 개발 도구와 종속성을 제공할 수 있습니다. 이 워크숍이 원활하게 진행되도록 해당 설정이 미리 준비되어 있습니다. 단일 Astro 앱에 필요한 Node.js, 프로젝트 종속성, 엔드투엔드 테스트용 브라우저 바이너리, 마이그레이션 및 시드 작업을 마친 SQLite 데이터베이스를 Copilot이 사용할 수 있도록 합니다.

```yaml
name: "Copilot Setup Steps"

# 리포지토리의 Actions 탭에서 설정 단계를 직접 실행할 수 있습니다.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 아래 단계에 필요한 최소 권한만 지정합니다. Copilot은 자체 토큰을 사용합니다.
    permissions:
      # 종속성 설치 등을 위해 설정 단계에서 리포지토리를 복제하려면 contents: read 권한이 필요합니다. 설정 단계에서 리포지토리를 복제하지 않으면 단계가 끝난 뒤 Copilot이 자동으로 복제합니다.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 프런트엔드 및 앱 설정 - Node.js(앱 전체가 Astro + Drizzle/libSQL로 구성됨)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 로컬 SQLite 데이터베이스를 마이그레이션하고 시드하여 빌드 및 테스트용 데이터를 준비합니다.
      - name: Set up the database
        run: npm run db:setup
```

다른 GitHub 워크플로 파일과 비슷하지만 몇 가지 중요한 특징이 있습니다.

- `copilot-setup-steps`라는 단일 작업이 있습니다. Copilot이 풀 리퀘스트에서 작업을 시작하기 전에 이 작업이 GitHub Actions에서 실행됩니다.
- `workflow_dispatch` 트리거를 사용하므로 리포지토리의 **Actions** 탭에서 워크플로를 직접 실행할 수 있습니다. Copilot이 작업을 시작할 때까지 기다리지 않고 워크플로가 성공적으로 실행되는지 확인할 수 있습니다.

## 문서 추가하기

문서가 중요하다는 것은 모두 알지만, 대부분의 프로젝트에는 오래된 정보가 있거나 문서가 전혀 없습니다. 이런 기술 부채는 해결되지 않은 채 남아 생산성을 낮추고 코드베이스를 유지 관리하거나 새 개발자를 온보딩하기 어렵게 만듭니다. Copilot은 문서 작성에도 유용하므로 이 작업을 Copilot cloud agent에 맡길 수 있습니다. Copilot은 백그라운드에서 필요한 문서를 작성합니다. 이후 연습에서 결과를 검토합니다.

1. 새 브라우저 탭에서 github.com의 리포지토리로 이동합니다.
2. **Issues** 탭을 선택합니다.
3. **New issue**를 선택하여 새 이슈 대화 상자를 엽니다.
4. **Blank issue**를 선택합니다.
5. **Title**에 `코드에 문서가 부족합니다`를 입력합니다.
6. **Description**에 다음을 입력합니다.

    ```plaintext
    우리 조직에서는 함수와 메서드에 도움이 되는 경우 TSDoc 문서 주석을 포함하도록 요구합니다. 안타깝게도 최근 업데이트에서 이 표준이 지켜지지 않았습니다. 동작을 명확히 하는 경우 문서 주석이 포함되도록 기존 코드를 업데이트해야 합니다.
    ```

7. **Create**를 선택하여 이슈를 만듭니다.
8. 오른쪽에서 **Assign to Copilot**을 선택하여 할당 대화 상자를 엽니다.

    ![이슈에 Copilot을 할당하는 화면](../../../_images/shared-assign-copilot.png)

9. **Assign**을 선택합니다.

    ![Copilot에 이슈를 할당한 세부 정보](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** 탭을 선택합니다.
11. 새 풀 리퀘스트(PR)를 엽니다. 제목은 `[WIP]: 문서 누락 수정`과 비슷할 수 있습니다. 목록에 새 PR이 없으면 잠시 기다린 뒤 브라우저 창을 새로 고칩니다.
12. 몇 분 뒤 Copilot이 할 일 목록을 만든 것을 확인합니다.

> [!NOTE]
> Copilot의 할 일 목록이 PR에 나타나기까지 몇 분 정도 걸릴 수 있습니다. Copilot이 작업 환경을 만들고, 앞서 설명한 워크플로를 실행하고, 프로젝트를 분석하고, 문제를 해결할 방법을 찾고 있습니다.

13. 목록과 Copilot이 수행할 작업을 검토합니다.
14. 풀 리퀘스트 타임라인을 아래로 스크롤하여 Copilot이 이슈 작업을 시작했다는 업데이트를 확인합니다.
15. **View session** 버튼을 선택합니다.

    ![Copilot 세션 보기](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 업데이트된 표시를 보려면 창을 새로 고쳐야 할 수 있습니다.

16. 실시간 세션을 스크롤하며 Copilot이 문제를 해결하는 과정을 살펴봅니다. 코드와 상태를 확인하고 적절한 계획을 세운 뒤 코드를 작성하는 과정을 확인할 수 있습니다.

이 작업은 몇 분 정도 걸립니다. Copilot cloud agent의 주요 장점은 비동기적으로 작업하여 개발자가 다른 일에 집중할 수 있게 하는 것입니다. 이 기능을 활용해 또 다른 작업을 Copilot cloud agent에 할당한 다음, 애플리케이션의 기능을 추가하는 작업을 진행합니다.

## 게임 상세 페이지에 관련 게임 섹션 추가하기

GitHub Copilot cloud agent의 장점 중 하나는 작업을 나눌 수 있다는 점입니다. 사용자는 한 가지 작업에 집중하고 Copilot은 다른 작업을 수행할 수 있습니다. 디자인 팀이 요청한 관련 게임 섹션을 추가하는 데 시간이 오래 걸리지 않더라도, 그 시간에 다른 작업을 할 수 있습니다. 이 작업도 Copilot cloud agent에 할당합니다.

1. github.com의 리포지토리로 돌아갑니다.
2. **Issues** 탭을 선택합니다.
3. **New issue**를 선택하여 새 이슈 대화 상자를 엽니다.
4. **Blank issue**를 선택합니다.
5. **Title**에 `게임 상세 페이지에 관련 게임 표시`를 입력합니다.
6. **Description**에 다음을 입력합니다.

    ```markdown
    사용자가 더 많은 게임을 발견할 수 있도록 각 게임 상세 페이지에 관련 게임을 표시하려고 합니다. 디자인 팀은 사용자 경험을 살펴보고 인수 테스트를 수행하려고 합니다. 요구 사항은 다음과 같습니다.

    - `src/lib/`에 지정한 게임을 제외하고 같은 카테고리의 다른 게임을 반환하는 데이터 접근 도우미를 추가합니다.
    - 해당 도우미를 사용하는 "관련 게임" 섹션을 게임 상세 페이지에 표시합니다.
    - 관련 게임이 없는 경우를 처리합니다.
    - 새 도우미에 대한 단위 테스트를 작성합니다.
    - PR을 만들기 전에 모든 테스트가 통과하는지 확인합니다.
    ```

7. **Create**를 선택하여 이슈를 만듭니다.
8. 오른쪽에서 **Assign to Copilot**을 선택하여 할당 대화 상자를 엽니다.

    ![이슈에 Copilot을 할당하는 화면](../../../_images/shared-assign-copilot.png)

9. **Assign**을 선택합니다.

잠시 후 이슈의 첫 번째 댓글에 Copilot이 작업 중임을 나타내는 👀 표시가 나타납니다.

![눈 이모지로 이슈 작업 중임을 표시하는 Copilot](../../../_images/ex4-issue-eyes-emoji.png)

이제 Copilot이 두 번째 요청을 처리하고 있습니다. Copilot cloud agent는 소프트웨어 엔지니어처럼 작업하므로 계속 지켜볼 필요는 없습니다. 작업이 완료된 후 결과를 검토하면 됩니다. 이제 사용자 지정 에이전트를 살펴보고 사용합니다.

## 요약 및 다음 단계

[GitHub Copilot cloud agent][copilot-agents]를 살펴봤습니다. cloud agent에 이슈를 할당하면 비동기적으로 작업을 수행합니다. Copilot에 기술 부채 해결, 새 기능 작성, 프레임워크 간 코드 마이그레이션 등을 요청할 수 있습니다.

이 연습에서는 다음을 살펴봤습니다.

- 코드 생성 환경을 사용자 지정하는 방법
- 작업이 안전하게 수행되는지 확인하는 방법
- 범위가 명확한 이슈의 중요성
- Copilot에 이슈를 할당하는 방법

Cloud agent가 백그라운드에서 작업하는 동안 사용자 지정 에이전트를 만들고 사용합니다. [Copilot cloud agent는 MCP 서버][cloud-agent-mcp]도 사용할 수 있으며, 앞선 모듈에서 살펴본 사용자 지정 지침을 함께 활용합니다.

## 리소스

- [Copilot cloud agent 정보][copilot-agents]
- [GitHub 이슈를 Copilot에 할당하기][assign-issue]
- [Copilot cloud agent 설정 워크플로 모범 사례][cloud-agent-best-practices]

| [다음 레슨: 사용자 지정 에이전트 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
