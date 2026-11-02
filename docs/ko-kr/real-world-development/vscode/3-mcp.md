---
title: "연습 3 - Playwright MCP 서버로 기능 테스트하기"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 이전 레슨: 에이전트 모드][previous-lesson] · [다음 레슨: 사용자 지정 에이전트 →][next-lesson]

에이전트 모드로 필터링 기능을 만들었습니다. 풀 리퀘스트를 열기 전에 브라우저에서 제대로 작동하는지 확인합니다. 앱을 직접 조작하는 대신 **Playwright MCP 서버**를 연결해 Copilot이 실제 브라우저에서 기능을 테스트하도록 합니다. 그런 다음 브랜치를 게시하고 PR을 엽니다.

이 연습에서는 다음을 수행합니다.

- Model Context Protocol(MCP)이 무엇이며 MCP 서버가 Copilot에 도구를 추가하는 방식을 알아봅니다.
- 작업 공간에 Playwright MCP 서버를 추가합니다.
- Copilot에 브라우저에서 필터링 기능을 수동으로 테스트하도록 요청합니다.
- 브랜치를 게시하고 필터링 작업의 풀 리퀘스트를 엽니다.

## Model Context Protocol(MCP)이란?

에이전트 모드는 편집기 외부의 도구를 사용할 수 있을 때 더욱 강력해집니다. Model Context Protocol(MCP)은 Copilot이 외부 도구 및 서비스와 통신하는 표준입니다.

![컨텍스트, LLM, MCP 서버 및 VS Code 확장이 제공하는 도구를 포함해 에이전트 모드가 상호 작용하는 방식을 보여 주는 다이어그램](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol(MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)은 AI 에이전트가 외부 도구 및 서비스와 통신할 수 있도록 합니다. MCP를 사용하면 리소스를 통해 최신 정보를 확인하고 도구를 사용해 사용자를 대신하여 작업을 수행할 수 있습니다.

에이전트는 MCP 서버를 통해 도구와 리소스에 접근합니다. MCP 서버는 에이전트와 외부 도구 및 서비스(기존 API나 NPM 패키지 같은 로컬 도구 포함) 사이의 연결 역할을 합니다. 각 MCP 서버에서 에이전트가 사용할 수 있는 도구와 리소스를 제공합니다.

널리 사용되는 MCP 서버의 예시는 다음과 같습니다.

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub 리포지토리, 이슈, 풀 리퀘스트를 관리하는 API를 제공합니다. AI 에이전트가 새 리포지토리를 만들고 기존 리포지토리를 업데이트하고 이슈 및 풀 리퀘스트를 관리할 수 있습니다.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright를 사용해 브라우저를 자동화합니다. AI 에이전트가 웹 페이지를 열고 양식을 작성하고 버튼을 선택할 수 있습니다.

다양한 MCP 서버가 있으며, GitHub는 생태계의 검색과 기여를 돕기 위해 [MCP 레지스트리](https://github.com/mcp)를 운영합니다.

> [!CAUTION]
> MCP 서버도 프로젝트의 다른 종속성과 마찬가지로 취급합니다. 사용하기 전에 소스 코드를 검토하고 게시자를 확인하며 보안에 미치는 영향을 고려합니다. 신뢰할 수 있는 MCP 서버만 사용하고 민감한 리소스나 작업에 대한 액세스를 허용할 때 주의합니다.

## MCP 구성 검토하기

`.vscode/mcp.json` 파일은 이 VS Code 작업 공간에서 사용할 수 있는 MCP 서버를 구성합니다.

1. Codespace에서 `.vscode/mcp.json`을 엽니다.
2. 다음과 같이 `github` 서버가 미리 구성되어 있는지 확인합니다.

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

`github` 항목은 프로젝트 템플릿에 포함되어 있으므로 이전 연습에서 Copilot이 이슈 백로그를 읽을 수 있었습니다. 이 서버는 [원격 GitHub MCP 서버][remote-github-mcp-server]를 사용하므로 로컬에 설치할 필요가 없습니다. VS Code가 HTTP로 연결하며 Copilot이 도구를 처음 사용할 때 GitHub로 인증합니다.

## Playwright MCP 서버 추가하기

이제 두 번째 서버를 추가합니다. [Playwright MCP 서버][playwright-mcp-server]는 Copilot에 브라우저를 제공해 기능을 테스트할 수 있게 합니다.

1. `.vscode/mcp.json`에서 `github`와 함께 `playwright` 항목을 추가해 다음과 같이 구성합니다.

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 파일을 저장합니다.

원격 `github` 서버와 달리 `playwright`는 VS Code가 `npx @playwright/mcp@latest`를 실행해 시작하는 **로컬** 서버입니다. `--headless` 플래그를 사용하면 Playwright가 화면에 창을 표시하지 않고 브라우저를 실행합니다. Codespace에는 브라우저를 표시할 데스크톱이 없으므로 이 설정이 필요합니다.

> [!NOTE]
> Tailspin Toys 프로젝트는 엔드투엔드 테스트에 이미 Playwright를 사용하므로 필요한 브라우저도 설치되어 있을 가능성이 높습니다. Copilot이 브라우저가 없다고 보고하면 `npx playwright install chromium`을 실행하고 다시 시도하도록 요청합니다.

## 서버 시작 및 신뢰하기

VS Code는 Copilot이 서버 도구를 처음 사용하는 시점에 MCP 서버를 **필요에 따라** 시작하고 서버를 신뢰하는지 확인합니다.

1. `.vscode/mcp.json`에서 `playwright` 항목 위에 표시되는 **Start** 작업을 선택해 서버를 시작하고 구성이 올바른지 확인합니다.
2. VS Code에서 서버를 신뢰할지 묻는 메시지가 표시되면 구성을 검토하고 신뢰를 선택해 서버를 시작합니다.

수동으로 시작하는 단계는 선택 사항입니다. 건너뛰면 다음 단계에서 Copilot이 도구를 처음 사용할 때 VS Code가 서버를 시작하고 신뢰 여부를 확인합니다.

## 필터링 기능 테스트하기

[Playwright MCP 서버][playwright-mcp-server]는 Copilot이 조작할 수 있는 실제 브라우저를 제공합니다. 에이전트가 페이지를 열고 이동하고 필터를 적용하고 결과를 읽은 뒤 요약할 수 있습니다. 대화를 벗어나지 않고 기능이 예상대로 동작하는지 빠르게 확인할 수 있습니다.

Playwright MCP 서버는 스크린샷 대신 페이지의 [접근성 트리][playwright-mcp-server]를 사용합니다. 따라서 에이전트는 보조 기술과 마찬가지로 버튼, 링크, 목록 항목 등의 레이블이 지정된 구조화 요소를 살펴봅니다. 기능을 빠르게 확인하면서 접근성도 간단히 점검할 수 있습니다.

서버를 연결하고 앱을 실행한 뒤 Copilot에 필터링 기능을 테스트하도록 요청합니다.

```text
Playwright MCP 서버를 사용해 실행 중인 앱(http://localhost:4321)에서 브라우저를 열고 새 게임 필터링 기능을 확인해 주세요.

1. 게임 페이지로 이동해 표시된 게임 수를 확인합니다.
2. 카테고리 필터를 적용하고 해당 카테고리의 게임만 표시되는지 확인합니다.
3. 카테고리 필터를 해제한 다음 퍼블리셔 필터를 적용하고 해당 퍼블리셔의 게임만 표시되는지 확인합니다.
4. 카테고리와 퍼블리셔 필터를 함께 적용하고 결과에 두 조건이 모두 반영되는지 확인합니다.

각 단계에서 확인한 내용을 보고하고 예상대로 동작하지 않는 항목을 알려 주세요.
```

Copilot은 Playwright MCP 서버로 브라우저를 시작하고 각 단계를 수행한 뒤 결과를 보고합니다. 보고서를 이슈의 수락 기준과 비교합니다. 문제가 있다면 풀 리퀘스트를 열기 전에 후속 질문을 하거나 코드를 수정하도록 요청합니다.

> [!NOTE]
> 이 테스트를 실행하려면 앱이 `http://localhost:4321`에서 실행 중이어야 합니다. 개발 서버를 중지했다면 프롬프트를 보내기 전에 다시 시작합니다. Copilot이 Playwright MCP 서버를 처음 사용할 때 브라우저를 다운로드해야 할 수 있습니다. 브라우저가 없다고 보고하면 `npx playwright install chromium`을 실행하고 다시 시도하도록 요청합니다.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 브랜치를 게시하고 풀 리퀘스트 만들기

기능이 제대로 작동하는지 확인했으니 팀에서 검토할 풀 리퀘스트(PR)를 엽니다. 먼저 `filtering-vscode` 브랜치를 게시합니다.

1. Codespace의 **Source Control** 패널에서 Copilot이 변경한 내용을 검토합니다.
2. **+** 아이콘을 선택해 변경 사항을 스테이징합니다.
3. **Sparkle** 버튼을 사용해 커밋 메시지를 생성합니다.

   ![Copilot이 변경한 내용을 보여 주는 Source Control 패널](../../../_images/ex3-source-control-changes.png)

4. **Publish**를 선택해 브랜치를 리포지토리에 푸시합니다.

## 풀 리퀘스트 만들기

github.com 또는 GitHub CLI를 비롯해 여러 방법으로 풀 리퀘스트를 만들 수 있습니다. Copilot에 관련 이슈를 찾아 현재 브랜치에서 이슈와 연결된 PR을 만들도록 요청합니다.

1. Copilot Chat 패널에서 **New Chat**을 선택해 새 세션을 시작합니다.
2. Copilot이 GitHub 도구를 사용할 수 있도록 에이전트 드롭다운에서 **Agent**가 선택되었는지 확인합니다.
3. Copilot에 PR을 만들도록 요청합니다.

   ```plaintext
   리포지토리에서 카테고리와 퍼블리셔로 필터링하는 이슈를 찾아 주세요. 현재 브랜치에서 새 풀 리퀘스트를 만들고 올바른 이슈와 연결해 주세요.
   ```

4. Copilot이 정보를 수집하고 작업을 수행할 수 있도록 메시지가 표시되면 **Continue**를 선택합니다. GitHub 도구를 처음 사용할 때 GitHub 인증을 요청할 수 있으므로 안내를 따릅니다.
5. Copilot이 이슈를 검색하고 올바른 이슈를 찾아 PR을 만드는 과정을 살펴봅니다.
6. Copilot이 생성한 링크를 선택해 풀 리퀘스트를 검토하되, **아직 병합하지 마세요**.

## 요약 및 다음 단계

이 연습에서는 다음을 수행했습니다.

- Model Context Protocol(MCP)이 무엇이며 MCP 서버가 Copilot에 도구를 추가하는 방식을 알아봤습니다.
- Playwright MCP 서버를 작업 공간에 추가했습니다.
- Playwright MCP를 사용해 브라우저에서 필터링 기능을 직접 테스트했습니다.
- 브랜치를 게시하고 필터링 작업의 풀 리퀘스트를 열었습니다.

MCP 서버를 사용해 기능을 *테스트*했습니다. 하지만 MCP는 Copilot을 확장하는 여러 방법 중 하나입니다. 다음으로 [사용자 지정 에이전트][next-lesson]를 만들어 접근성 검토와 같은 작업을 간소화합니다.

## 리소스

- [MCP란 무엇이며 왜 주목받고 있나요?][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code의 MCP 서버][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
