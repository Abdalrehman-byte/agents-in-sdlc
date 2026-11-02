---
title: "연습 2 - Copilot 에이전트 모드로 새 기능 추가하기"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

애플리케이션을 조금만 변경해도 여러 파일을 수정하고 테스트를 실행해야 하는 경우가 많습니다. 개발자는 관련 파일을 찾고 변경하고 테스트를 실행하고 오류를 수정하고 빠뜨린 파일을 찾아 다시 변경하는 등의 작업을 반복합니다.

이럴 때 Copilot 에이전트 모드를 사용할 수 있습니다.

Copilot 에이전트 모드는 IDE에서 더 자율적으로 작업하도록 설계되었습니다. 개발자처럼 기존 프로젝트 구조를 살펴보고 필요한 변경을 적용하고 테스트를 실행하며 발견된 문제를 자동으로 수정합니다. 에이전트 모드를 사용해 사이트에 기능을 추가하는 방법을 알아봅니다.

> [!NOTE]
> 이름은 비슷하지만 에이전트 모드와 cloud agent는 서로 다른 방식으로 작동합니다. 에이전트 모드는 IDE에서 작업하고 빠르게 피드백을 주고받습니다. Cloud agent는 이슈와 풀 리퀘스트로 상호 작용하며 팀원처럼 비동기적으로 작업합니다.

이 연습에서는 다음을 수행합니다.

- Copilot 에이전트 모드가 프로젝트를 살펴보고 관련 파일을 찾아 변경을 조율하는 방법을 알아봅니다.
- GitHub Copilot 에이전트 모드로 UI와 데이터 계층 전반에 기능을 구현합니다.
- 코드베이스에 병합하기 전에 Copilot 에이전트 모드가 생성한 변경 사항과 테스트를 검토합니다.

## 시나리오

게임 목록이 늘어나면서 사용자가 카테고리와 퍼블리셔별로 게임을 필터링할 수 있어야 합니다. 이전 연습에서 퍼블리셔 도우미를 추가했으며, 이제 Copilot 에이전트 모드로 나머지 데이터 계층, UI, 테스트 작업을 완료합니다.

## Tailspin Toys 웹사이트 실행하기

변경 사항을 적용하기 전에 Tailspin Toys 웹사이트를 살펴보고 현재 기능을 확인합니다.

이 웹사이트는 개발자 테마의 보드게임 크라우드 펀딩 플랫폼입니다. 사용자가 게임 목록과 상세 정보를 볼 수 있습니다. 단일 Astro 앱으로 구성되어 있으며 빌드 시 페이지를 정적 HTML로 렌더링합니다. 페이지는 frontmatter에서 Drizzle ORM을 통해 로컬 SQLite 데이터베이스를 직접 조회합니다. 별도 백엔드 API나 클라이언트 UI 프레임워크는 없습니다. 재사용 가능한 데이터 접근 도우미는 `src/lib/`에 있고, 상호 작용 기능은 표준 DOM API를 사용하는 작은 범위의 Astro `<script>`로 구현합니다.

### 웹사이트 시작하기

`npm run dev` 명령으로 단일 Astro 개발 서버를 시작할 수 있습니다. 다음 단계를 따라 GitHub Codespace에서 실행합니다.

1. Codespace로 돌아갑니다. 이전 연습에서 `main`으로 돌아왔어야 합니다.
2. Mac에서는 <kbd>Control</kbd>+<kbd>\`</kbd>를, Windows/Linux에서는 <kbd>Ctrl</kbd>+<kbd>\`</kbd>를 눌러 Codespace에서 새 터미널을 엽니다.
3. 필터링 작업을 위한 새 브랜치를 만들고 전환합니다.

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 다음 명령을 실행해 웹사이트를 시작합니다.

   ```bash
   npm run dev
   ```

   Astro 개발 서버가 준비되면 다음과 비슷한 URL이 표시됩니다.

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321`을 브라우저 창에서 열라는 대화 상자가 표시되면 **x**를 선택해 닫습니다.

5. 터미널에서 Astro 서버 주소 `http://localhost:4321`을 선택할 때 <kbd>Command</kbd>(Mac) 또는 <kbd>Ctrl</kbd>(Windows/Linux)을 누르고 있습니다.

> [!NOTE]
> Codespace 터미널에서 localhost URL을 선택하면 `https://<your-codespace-name>-4321.app.github.dev/`로 자동 전환됩니다. 이 주소는 웹 서버를 실행 중인 Codespace로 연결되는 비공개 터널입니다.

### 웹사이트 살펴보기

웹사이트가 실행되면 기능을 살펴봅니다. 주요 기능은 다음과 같습니다.

- **홈페이지**: 보드게임 목록과 제목, 이미지, 설명을 표시합니다.
- **게임 상세 페이지**: 게임을 선택하면 제목, 설명, 퍼블리셔, 카테고리 등의 추가 정보를 볼 수 있습니다.

## Copilot으로 백로그 살펴보기

> [!TIP]
> **Copilot Chat 열기**
>
> 아래 연습을 시작하기 전에 Codespace로 돌아가 Copilot Chat 패널을 열고 **New Chat**을 선택하여 새 대화를 시작합니다. 모드와 모델 선택은 연습마다 다를 수 있습니다. 필요한 단계에서 안내합니다.

웹사이트의 초기 구현은 정상적으로 작동하지만 새로운 기능을 추가하려고 합니다. 먼저 백로그를 살펴봅니다. 템플릿에서 프로젝트를 만들 때 GitHub 이슈 백로그도 자동으로 생성되었습니다. GitHub Copilot에 이슈 목록을 보여 달라고 요청합니다.

1. Chat 보기의 에이전트 드롭다운에서 **Agent**를 선택합니다. 에이전트 모드는 여러 파일에 걸쳐 변경을 계획하고 구현하고 터미널 명령과 도구를 실행할 수 있습니다.

   ![Chat 보기의 에이전트 선택기를 보여 주는 스크린샷](../../../_images/shared-chat-mode-selector.png)

2. 사용 가능한 모델 목록에서 **Claude Sonnet 4.5**를 선택합니다.

> [!CAUTION]
> 워크숍 작성자가 특정 모델을 선호한다는 의미는 아닙니다. 이 워크숍을 만들 때 Claude Sonnet 4.5를 사용했기 때문에 안내에도 해당 모델을 포함했습니다. 안내를 일관되게 제공하려는 목적이지만 대규모 언어 모델은 확률적으로 작동하므로 코드 제안이 설명과 다를 수 있습니다. 이는 정상적인 동작입니다.

> [!NOTE]
> 확률적인 특성으로 인해 Copilot이 다른 MCP 명령을 사용할 수 있지만 작업은 완료할 수 있어야 합니다.

3. 다음 프롬프트를 보내 Copilot에 이슈 백로그를 보여 달라고 요청합니다.

   ```plaintext
   내 GitHub 리포지토리의 백로그 항목을 보여 주세요. 사용자에게 가장 유용한 항목을 기준으로 우선순위를 정하는 데 도움을 주세요.
   ```

4. **Continue**를 선택해 모든 이슈를 나열하는 명령을 실행합니다.
5. 생성된 이슈 목록을 검토합니다.

Copilot이 사용자에게 가장 유용하다고 판단한 작업을 기준으로 우선순위까지 지정하는 것을 확인할 수 있습니다.

## 지침 파일 검토하기

Copilot에 코드를 생성하도록 요청하기 전에 작업에 필요한 컨텍스트를 제공할 지침 파일을 검토합니다. 웹사이트 기능을 추가하는 방법을 설명하는 [UI 지침 파일](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)을 사용합니다.

1. Codespace에서 `.github/instructions/ui.instructions.md`로 이동합니다.
2. 기능을 추가할 때 사용하는 전반적인 지침을 확인합니다.
   - 아키텍처 개요
   - 컴포넌트 설계, 테스트 가능성, 접근성 원칙
   - Astro, Tailwind CSS, Drizzle 등 파일 유형별 지침 링크

> [!TIP]
> 지침 파일에서는 다른 지침 파일과 프로젝트 파일을 모두 참조할 수 있습니다. 경로는 지침 파일 위치를 기준으로 합니다. 이 방법을 사용하면 지침을 재사용하고 더 작은 파일로 나누고 예제와 템플릿을 제공할 수 있습니다.

## 필터링 기능 구현하기

필터링 기능을 완성하려면 애플리케이션을 세 군데 이상 수정해야 합니다.

- 데이터 계층(`src/lib/`)에서 필터링 로직을 추가하거나 개선합니다.
- 필터링 동작을 테스트하는 테스트를 추가하거나 업데이트합니다.
- 게임 목록 페이지에 필터링 UI를 추가합니다.

또한 코드베이스에 병합하기 전에 테스트를 실행해 통과하는지 확인해야 합니다. Copilot 에이전트 모드로 이러한 작업을 수행할 수 있습니다.

1. 현재 Copilot 대화를 이어 가거나 **New Chat**을 선택해 새 대화를 시작합니다.
2. **Add Context**, **Instructions**, **ui**를 선택해 지침 파일을 추가합니다.

   ![UI 지침 파일을 선택하는 예시를 보여 주는 스크린샷](../../../_images/ex3-select-instructions-file.png)

3. Chat 보기의 에이전트 드롭다운에서 **Agent**가 선택되어 있는지 확인합니다.

   ![Chat 보기의 에이전트 선택기를 보여 주는 스크린샷](../../../_images/shared-chat-mode-selector.png)

4. 모델로 **Claude Sonnet 4.5**가 선택되어 있는지 확인합니다.
5. 백로그의 관련 이슈를 기준으로 기능을 구현하도록 Copilot에 다음을 요청합니다.

   ```plaintext
   백로그의 관련 GitHub 이슈 요구 사항에 따라 사이트에 퍼블리셔 및 카테고리 필터링을 추가해 주세요. 이전 연습에서 퍼블리셔 도우미를 만들었으므로 기존 작업을 유지하고 필요하면 개선하면서 데이터 계층, UI, 테스트를 완성해 주세요. 모든 테스트가 통과하는지 확인해 주세요. 서버가 이미 실행 중이므로 다시 시작하지 않아도 됩니다.
   ```

6. Copilot이 프로젝트를 살펴보고 기능과 관련된 파일을 찾는 과정을 확인합니다. 데이터 계층 도우미, UI, 테스트 파일을 찾아 변경하고 테스트를 실행합니다.

   ![Copilot이 프로젝트 파일을 살펴보는 스크린샷](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot은 프로젝트를 살펴보고 파일을 수정하고 테스트를 실행합니다. 코드베이스와 작업 복잡도에 따라 몇 분 정도 걸릴 수 있습니다. 작업 중 코드 편집기에 **Keep**과 **Undo** 버튼이 표시될 수 있습니다. Copilot이 끝나면 모든 변경 사항에 **Keep** 또는 **Undo**가 표시되므로 작업 중에 선택할 필요는 없습니다.

7. Copilot이 요청하면 **Continue**를 선택해 테스트를 실행합니다.

   ![Copilot Chat에서 테스트 실행을 확인하는 대화 상자](../../../_images/ex3-agent-mode-run-tests.png)

8. 작업 중에 잠시 멈추거나 테스트가 실패할 수 있습니다. Copilot은 오류가 없다고 판단할 때까지 코드 생성과 테스트 실행을 반복합니다.

   ![Copilot 에이전트 모드의 전체 Chat 세션을 보여 주는 스크린샷](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 생성된 코드에 문제가 없는지 살펴봅니다.

> [!CAUTION]
> Copilot 또는 다른 AI 도구가 생성한 코드는 항상 검토해야 합니다.

10. 웹사이트가 실행 중인 브라우저로 돌아가 새 기능을 살펴봅니다.
11. 기능이 제대로 작동하고 코드를 검토했으면 Copilot Chat 창에서 **Keep**을 선택합니다.

## 요약 및 다음 단계

GitHub Copilot 에이전트 모드로 Tailspin Toys 웹사이트에 기능을 추가하는 방법을 살펴봤습니다. 이 연습에서는 다음을 수행했습니다.

- GitHub Copilot 에이전트 모드로 UI와 데이터 계층 전반에 기능을 구현했습니다.
- Copilot 에이전트 모드가 프로젝트를 살펴보고 관련 파일을 찾아 변경을 조율하는 방법을 알아봤습니다.
- 코드베이스에 병합하기 전에 Copilot 에이전트 모드가 생성한 변경 사항과 테스트를 검토했습니다.

다음으로 [Playwright MCP 서버로 기능을 테스트][next-lesson]하고 풀 리퀘스트를 엽니다.

### 보너스 탐색 연습 - 페이지 매김 구현하기

게임 목록이 커지면 페이지 매김 기능이 필요할 수 있습니다. 이 연습에서 배운 내용을 사용해 Copilot에 사이트의 페이지 매김 기능을 구현하도록 요청합니다. 다음 항목을 고려합니다.

- 기존 지침 파일을 포함해 모범 사례를 따릅니다.
- 사용자가 페이지 크기를 선택하도록 할지, 페이지 크기를 고정할지 결정합니다.
- 원하는 구현에 필요한 지침을 프롬프트에 포함합니다.
- Copilot에 반복해서 변경을 요청하고 컨텍스트를 제공할 수 있습니다. 이는 Copilot과 함께 작업하는 일반적인 과정입니다.

## 리소스

- [Copilot의 ask, edit, agent 모드: 각각의 역할과 사용 시점][choose-mode]
- [VS Code의 에이전트 모드][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
