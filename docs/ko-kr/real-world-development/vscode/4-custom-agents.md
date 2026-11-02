---
title: "연습 4 - 사용자 지정 에이전트"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 사용자 지정 에이전트란?

GitHub Copilot의 [사용자 지정 에이전트][custom-agents-concept]는 개발 워크플로의 특정 작업이나 영역에 맞춘 전문 AI 도우미를 만들 수 있게 합니다. 리포지토리의 `.github/agents` 폴더에 Markdown 파일로 에이전트를 정의하면 Copilot이 특정 작업을 더 효과적으로 수행하도록 집중된 지침, 모범 사례, 코딩 패턴, 도메인 지식을 제공할 수 있습니다. 팀은 접근성 규정을 적용하는 에이전트, [WCAG][wcag]를 따르는 에이전트, 보안 코딩 관행을 적용하는 에이전트, 일관된 테스트 패턴을 유지하는 에이전트처럼 전문 지식을 재사용 가능한 에이전트로 정리할 수 있습니다.

사용자 지정 에이전트는 프로젝트의 `.github/agents` 폴더 또는 전역 `~/.copilot/agents` 폴더에 Markdown 파일로 정의합니다. 각 파일에는 최소한 `name`과 `description`이 포함된 YAML frontmatter가 있고, 그 뒤에 에이전트의 동작 방식, 전문 지식, 지침을 정의하는 Markdown 프롬프트가 이어집니다.

### 사용자 지정 에이전트와 에이전트 스킬 비교

사용자 지정 에이전트와 [에이전트 스킬][agent-skills-concept]은 일부 역할이 겹칩니다. 둘 다 주로 Markdown 파일로 정의하며 AI에 작업 수행 방법을 알려 줍니다. 간단하게 구분하면 **사용자 지정 에이전트는 작업자이고 스킬은 도구**입니다.

사용자 지정 에이전트에는 자체 컨텍스트 창이 있으며 작업을 수행할 때 스킬이나 다른 에이전트를 조율할 수 있습니다. 이 실습의 접근성 사용자 지정 에이전트는 접근성 지침에 따라 사이트를 검토하고 업데이트합니다. 작업의 일부로 풀 리퀘스트 워크플로 스킬이나 테스트를 실행하고 관리하는 스킬을 호출할 수 있습니다.

> [!NOTE]
> 사용자 지정 에이전트를 작성하는 한 가지 정답은 없습니다. AI를 사용하는 다른 작업과 마찬가지로 환경과 시나리오에 맞게 테스트하고 반복 개선합니다.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 시나리오

Tailspin Toys는 시각 능력이나 선호도에 관계없이 모든 사용자가 크라우드 펀딩 플랫폼을 이용할 수 있도록 노력하고 있습니다. 최근 사용자 의견에 따르면 텍스트와 배경 색상의 대비가 충분하지 않아 일부 사용자가 현재 어두운 테마를 읽기 어렵다고 합니다. 이에 디자인 팀은 사용자가 켜고 끌 수 있는 고대비 모드를 구현해 달라고 요청했습니다.

접근성은 중요하므로 이 기능을 가능한 한 빨리 구현하려고 합니다. 사용자 지정 에이전트를 사용해 기능을 생성합니다.

이 연습에서는 다음을 수행합니다.

- 기존 접근성 사용자 지정 에이전트를 검토합니다.
- Copilot Chat에서 접근성 에이전트를 사용해 고대비 모드를 구현합니다.

## 접근성 사용자 지정 에이전트 검토하기

접근성 작업을 위한 사용자 지정 에이전트가 이미 준비되어 있습니다. 내용을 검토하여 Copilot을 어떻게 안내하는지 확인합니다.

Codespace로 돌아가 터미널을 열고 `main`에서 접근성 작업을 위한 새 브랜치를 만듭니다. 필터링 PR은 연습 3에서 별도로 관리합니다.

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md`를 엽니다.
2. `name` 및 `description` 필드가 있는 YAML frontmatter를 확인합니다.

> [!CAUTION]
> 사용자 지정 에이전트에는 `name` 및 `description` frontmatter가 필요합니다.

3. 이어지는 섹션을 살펴보고 다음 내용을 확인합니다.
   - 접근성 있는 웹사이트를 위한 코드 생성 시 핵심 책임
   - 접근성 모범 사례
   - HTML, CSS, JavaScript 코드 예제
   - 흔히 발생하는 문제와 실수

## Copilot Chat에서 사용자 지정 에이전트 사용하기

VS Code의 Copilot Chat 보기 하단에 있는 에이전트 드롭다운에는 `.github/agents`에 정의된 모든 사용자 지정 에이전트가 표시됩니다. 사용자 지정 에이전트를 선택하면 해당 에이전트의 지침과 도구를 사용하는 채팅 세션을 시작할 수 있습니다.

> [!TIP]
> **Copilot Chat 열기**
>
> 아래 연습을 시작하기 전에 Codespace로 돌아가 Copilot Chat 패널을 열고 **New Chat**을 선택해 새 대화를 시작합니다. 모드와 모델 선택은 연습마다 다를 수 있습니다. 필요한 단계에서 안내합니다.

1. Chat 보기의 에이전트 드롭다운에서 **Agent**를 선택합니다. 이미 선택되어 있다면 그대로 둡니다.

   ![Chat 보기에서 에이전트 선택기를 보여 주는 스크린샷](../../../_images/shared-chat-mode-selector.png)

2. Chat 보기 아래의 에이전트 드롭다운을 선택합니다. 기본적으로 활성 에이전트인 **default**가 표시됩니다.
3. 사용 가능한 에이전트 목록에서 **Accessibility agent**를 선택합니다.
4. 접근성 에이전트에 다음 프롬프트를 보냅니다.

   ```plaintext
   사이트에 고대비 모드를 추가해 주세요. 사용자가 켜고 끌 수 있는 고대비 토글이 있어야 하며, 브라우저의 로컬 스토리지(local storage)를 사용해 페이지를 다시 로드한 뒤에도 설정이 유지되어야 합니다.
   ```

5. Copilot Chat이 코드베이스를 살펴보고 변경 사항을 제안한 뒤 프로젝트 파일에 적용합니다. 각 편집 내용은 파일 경로 및 검토할 수 있는 diff와 함께 채팅에 표시됩니다.
6. 편집이 적용되면 **Files changed** 표시에서 에이전트가 수정한 파일 목록이 갱신됩니다.

> [!NOTE]
> 이 작업은 몇 분 정도 걸릴 수 있습니다. Copilot이 실제로 리포지토리를 수정하며 Astro 컴포넌트, CSS, 관련 테스트 등을 편집합니다.

다음 연습에서는 진행 중인 작업을 검토하고 방향을 조정합니다.

## 요약 및 다음 단계

GitHub Copilot의 [사용자 지정 에이전트][custom-agents]는 특정 작업과 영역을 위한 전문 AI 도우미입니다. 사용자 지정 에이전트를 사용하면 팀의 전문 지식과 표준을 재사용 가능한 에이전트로 정리하여 Copilot이 더 효과적으로 작업하도록 안내할 수 있습니다.

이 레슨에서는 다음을 살펴봤습니다.

- 사용자 지정 에이전트가 정의되는 방식
- Copilot Chat 에이전트 모드에서 사용자 지정 에이전트를 사용하는 방법

다음으로 [에이전트 작업을 모니터링하고 안내합니다][next-lesson]. 변경 사항을 검토하며 동일한 세션에 밝은 모드 토글을 추가하도록 요청합니다.

## 리소스

- [사용자 지정 에이전트 정보][custom-agents]
- [IDE에서 사용자 지정 에이전트 만들기][creating-custom-agents-ide]
- [VS Code의 사용자 지정 에이전트][custom-agents-vscode]
- [사용자 지정 에이전트 구성][custom-agents-config]
- [awesome-copilot의 사용자 지정 에이전트][awesome-copilot-agents]

---

| [← 이전 레슨: Playwright MCP 서버로 기능 테스트][previous-lesson] | [다음 레슨: 에이전트 모니터링 및 관리 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
