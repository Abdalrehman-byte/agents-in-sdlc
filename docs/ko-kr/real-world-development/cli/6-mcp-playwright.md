---
title: "레슨 6 - Playwright MCP로 기능 검증하기"
description: "Copilot CLI에서 Playwright MCP를 확인하거나 구성하고 브라우저에서 필터링 기능을 살펴봅니다."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

앞서 살펴봤듯이 코드를 작성하는 일만으로는 충분하지 않습니다. 데이터와 외부 서비스를 다루고 Copilot에서 자동화 작업을 사용할 수 있도록 해야 합니다. MCP 서버를 사용하면 Copilot CLI에 기본 제공되는 기능을 넘어 도구와 서비스를 추가할 수 있습니다.

이 레슨에서는 다음을 수행합니다.

- Model Context Protocol(MCP)이 무엇이며 Copilot CLI에서 어떻게 사용하는지 알아봅니다.
- 아직 사용할 수 없는 경우 Playwright MCP 서버를 추가합니다.
- 에이전트가 브라우저를 조작해 필터링 기능을 살펴보도록 요청합니다.

## 시나리오

단위 테스트와 엔드투엔드 테스트도 중요하지만, UI 업데이트를 검증하려면 실제로 UI와 상호 작용해야 합니다. Copilot이 사용자처럼 작업 중인 웹사이트를 사용하도록 하여 변경 사항이 예상대로 작동하는지 더 확실히 확인할 수 있습니다.

## Model Context Protocol(MCP)이란?

[Model Context Protocol(MCP)][mcp-blog-post]은 AI 에이전트가 외부 도구 및 서비스와 통신할 수 있도록 합니다. MCP를 사용하면 최신 정보를 확인하고 사용자를 대신해 작업을 수행할 수 있습니다.

에이전트는 MCP 서버를 통해 도구와 리소스에 접근합니다. MCP 서버는 에이전트와 외부 도구 및 서비스 사이의 연결 역할을 합니다. 각 MCP 서버에서 에이전트가 사용할 수 있는 도구와 리소스가 제공됩니다.

널리 사용되는 MCP 서버의 예시는 다음과 같습니다.

- **[GitHub MCP Server][github-mcp]**: GitHub 리포지토리, 이슈, 풀 리퀘스트를 관리하는 API에 접근할 수 있습니다.
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright를 사용한 브라우저 자동화 기능을 제공합니다.

다양한 MCP 서버가 있으며, GitHub는 생태계의 검색 및 기여를 돕기 위해 [MCP 레지스트리][mcp-registry]를 운영합니다.

> [!CAUTION]
> MCP 서버도 프로젝트의 다른 종속성과 마찬가지로 취급합니다. 사용하기 전에 소스 코드를 검토하고 게시자를 확인하며 보안에 미치는 영향을 고려합니다.

## Playwright MCP 서버 추가하기

Playwright MCP 서버를 아직 사용할 수 없다면 추가하여 Copilot이 사용자처럼 사이트와 상호 작용하도록 합니다.

> [!NOTE]
> Copilot CLI에서 Playwright MCP를 이미 사용할 수 있다면 아래 설정 단계는 건너뛰고 다음 섹션으로 진행합니다.

1. Codespace로 돌아갑니다.
2. Copilot CLI에서 다음 명령을 입력해 MCP 서버 추가 대화 상자를 엽니다.

   ```plaintext
   /mcp add
   ```

3. 이름으로 `playwright`를 입력하고 <kbd>Tab</kbd>을 누릅니다.
4. <kbd>Enter</kbd>를 눌러 서버 유형으로 **STDIO**를 확인한 다음 <kbd>Tab</kbd>을 누릅니다.
5. **Command** 대화 상자에 다음을 붙여 넣습니다.

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>(Mac) 또는 <kbd>Ctrl</kbd>+<kbd>S</kbd>(Windows/Linux)를 눌러 새 MCP 서버를 저장합니다.
7. <kbd>Esc</kbd>를 눌러 MCP 대화 상자를 닫습니다.

## Playwright를 사용해 Copilot이 기능을 살펴보도록 요청하기

앞서 기능이 제대로 작동하는지 직접 확인했습니다. 이제 Copilot이 사용할 수 있는 Playwright MCP 서버로 같은 검증을 수행합니다.

1. 다음 프롬프트로 Copilot에 Playwright MCP 서버를 사용해 기능을 검증하도록 요청합니다.

   ```plaintext
   앱을 시작하고 Playwright MCP로 이슈와 계획에 따라 필터링 기능을 확인해 주세요. 변경하지 말고 무엇이 작동하고 작동하지 않는지 알려 주세요. 작업이 끝나면 시작한 서버를 중지해 주세요.
   ```

> [!NOTE]
> 일반적으로 Copilot이 알아서 MCP 서버를 사용하므로 서버 이름을 직접 요청할 필요는 없습니다. 하지만 사용할 서버를 알고 있다면 명시적으로 알려 주는 편이 좋습니다. 일관성이 높아지고 토큰을 조금 절약할 수 있습니다.

2. Copilot이 브라우저에서 기능을 확인하는 여러 단계를 표시하는지 살펴봅니다.
3. 보고서를 읽고 모든 동작이 예상과 일치하는지 확인합니다.

Copilot은 서버를 시작하고 Playwright로 사이트를 조작한 다음 서버를 중지하고 결과를 보고합니다.

## 요약 및 다음 단계

이 레슨에서는 다음을 수행했습니다.

- Model Context Protocol(MCP)이 무엇이며 MCP 서버가 Copilot에 도구를 추가하는 방식을 알아봤습니다.
- Playwright MCP 서버를 사용할 수 있는지 확인하고, 필요한 경우 추가했습니다.
- 에이전트가 브라우저를 조작해 필터링 기능을 살펴보도록 요청했습니다.

사용 가능한 Playwright MCP 서버로 기능을 *테스트*했습니다. 하지만 MCP는 Copilot을 확장하는 여러 방법 중 하나입니다. 다음으로 [사용자 지정 에이전트][next-lesson]를 만들어 접근성 검토와 같은 작업을 간소화합니다.

## 리소스

- [MCP란 무엇이며 왜 주목받고 있나요?][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI에 MCP 서버 추가하기][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
