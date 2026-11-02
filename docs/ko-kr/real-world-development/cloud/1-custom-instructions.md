---
title: "연습 1 - 사용자 지정 지침(Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 이전 레슨: 사전 준비][previous-lesson] · [다음 레슨: Copilot cloud agent →][next-lesson]

생성형 AI를 사용할 때는 컨텍스트가 중요합니다. 특정 방식으로 작업해야 하거나 Copilot이 알아야 할 배경 정보가 있다면, 해당 컨텍스트를 Copilot이 사용할 수 있도록 해야 합니다. [지침 파일][instruction-files]은 Copilot에 지침을 제공하여 원하는 작업뿐 아니라 작업 방식도 이해하도록 돕습니다.

이 연습에서는 다음을 수행합니다.

- 리포지토리 사용자 지정 지침과 경로 범위 지침 파일을 통해 프로젝트 컨텍스트, 코딩 지침, 문서 표준이 Copilot에 전달되는 방식을 살펴봅니다.
- `.github/copilot-instructions.md`에 새로운 리포지토리 전체 표준을 추가합니다.

> [!NOTE]
> VS Code 및 CLI 과정과 달리 여기서는 전후 결과를 비교하는 프롬프트를 실행하지 않습니다. Copilot cloud agent는 GitHub 이슈를 비동기적으로 처리하므로 실시간으로 나란히 비교하기 어렵습니다. 이 과정의 뒷부분에서 cloud agent가 만든 풀 리퀘스트를 검토할 때 지침 파일의 영향을 확인합니다.

## 지침 파일

### 시나리오

Tailspin Toys에는 개발 관행에 관한 표준과 요구 사항이 있습니다.

- 데이터 계층에는 항상 단위 테스트가 필요합니다.
- UI는 어두운 모드와 현대적인 느낌을 사용해야 합니다.
- 코드에는 TSDoc 문서 주석을 추가해야 합니다.
- 각 파일의 맨 위에는 파일의 역할을 설명하는 주석 블록을 추가해야 합니다.

지침 파일을 사용해 Copilot이 이러한 관행에 따라 작업하는 데 필요한 정보를 제공합니다.

### 사용자 지정 지침

사용자 지정 지침은 Copilot에 컨텍스트와 선호 사항을 제공하여 코딩 스타일과 요구 사항을 더 잘 이해하도록 합니다. 코딩 규칙, 라이브러리, 포함하려는 주석 유형을 지정할 수 있습니다. 리포지토리 전체 또는 특정 파일 유형과 작업에 적용되는 지침을 만들 수 있습니다.

지침 파일에는 다음 두 가지 유형이 있습니다.

- `.github/copilot-instructions.md`는 리포지토리의 **모든** 요청에 Copilot이 사용하는 단일 지침 파일입니다. 대부분의 요청에 공통으로 적용되는 프로젝트 수준의 컨텍스트와 지침을 작성합니다.
- `.github/instructions/*.instructions.md` 파일은 특정 작업이나 파일 유형에 대한 지침을 제공합니다. TypeScript나 Astro 같은 특정 언어 또는 UI 컴포넌트나 단위 테스트 작성과 같은 작업에 대한 지침을 만들 수 있습니다.

> [!NOTE]
> IDE에서는 지침 파일을 Copilot Chat의 코드 생성에만 사용하며, 코드 완성이나 다음 편집 제안에는 사용하지 않습니다.
>
> Copilot Chat, Copilot CLI, Copilot cloud agent는 코드 생성 시 리포지토리 수준 지침과 `applyTo` frontmatter가 있는 `*.instructions.md` 파일을 함께 사용합니다.
>
> Copilot은 AGENTS.md 및 CLAUDE.md 파일을 비롯한 다른 형식의 지침 파일도 [지원합니다][custom-instructions-support].

### 지침 파일 관리 모범 사례

지침 파일을 만드는 방법에 대한 자세한 설명은 이 워크숍의 범위를 벗어납니다. 샘플 프로젝트에는 일반적인 접근 방식을 보여 주는 예제가 포함되어 있습니다. 큰 틀에서는 다음과 같이 관리합니다.

- `copilot-instructions.md`에는 프로젝트 설명, 프로젝트 구조, 전역 코딩 표준처럼 프로젝트 수준의 지침을 작성합니다.
- `*.instructions.md` 파일에는 단위 테스트, Astro 컴포넌트, 데이터 계층과 같은 파일 유형별 지침이나 특정 작업 지침을 작성합니다.
- 자연어를 사용하고, 지침을 명확하게 작성하며, 좋은 코드와 피해야 할 코드 예제를 제공합니다.

지침 파일을 만드는 한 가지 정답이나 AI를 사용하는 한 가지 정답은 없습니다. 직접 실험하며 프로젝트에 맞는 방법을 찾습니다.

> [!TIP]
> GitHub Copilot을 사용하는 프로젝트에는 체계적인 지침 파일 모음이 필요합니다. 프로젝트 파일을 살펴보면 [UI 업데이트][ui-instructions] 및 [Astro][astro-instructions]를 비롯한 여러 작업에 대한 지침을 확인할 수 있습니다.
>
> Copilot에 지침 파일 생성을 요청할 수도 있습니다. 각 환경에서 사용하는 방법은 다릅니다. 예를 들어 VS Code에서는 **Configure Chat → Generate Agent Instructions**, Copilot CLI에서는 `/init`을 사용할 수 있습니다. 해당 환경의 레슨에서 관련 방법을 안내합니다.
>
> 시작 템플릿을 찾고 있다면 지침 파일, 사용자 지정 에이전트, 기타 리소스가 있는 [awesome-copilot][awesome-copilot] 리포지토리를 살펴봅니다.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 이 프로젝트의 사용자 지정 지침 파일 살펴보기

이 리포지토리에는 핵심 `copilot-instructions.md` 파일 하나와 다양한 작업을 위한 `*.instructions.md` 파일 모음이 있습니다. 편집기 또는 GitHub 웹 UI에서 파일을 엽니다.

1. `.github/copilot-instructions.md`를 엽니다.
2. 프로젝트에 대한 간단한 설명과 **Agent notes**, **Code standards**, **Scripts**, **Repository Structure** 등의 섹션을 살펴봅니다. **Code standards** 아래의 **GitHub Actions Workflows** 지침도 확인합니다. 이 지침은 Copilot과 상호 작용할 때마다 적용됩니다.
3. `.github/instructions` 폴더를 열어 파일을 살펴봅니다. Astro 파일, Drizzle 데이터 계층, 테스트 등에 대한 지침이 있습니다.
4. `.github/instructions/unit-tests.instructions.md`를 엽니다. 맨 위의 `applyTo` 필드는 리포지토리 루트에 상대적인 glob을 지정합니다. 여기서는 `**/*.test.ts`와 일치하는 TypeScript 테스트 파일에 지침이 적용됩니다.
5. 단위 테스트 작성에 관한 프로젝트별 지침을 확인합니다.
6. 마지막으로 `.github/instructions/drizzle.instructions.md`를 열고 맨 아래로 이동합니다. `unit-tests.instructions.md` 같은 다른 지침 파일과 프로젝트 파일을 가리키는 링크를 확인합니다. 이를 활용하면 큰 지침을 작고 재사용 가능한 파일로 나누고, Copilot이 참고할 예제를 지정할 수 있습니다. 이 파일의 경로는 리포지토리 루트가 아니라 지침 파일 위치를 기준으로 합니다.

> [!NOTE]
> `copilot-instructions.md`의 **Code formatting requirements** 섹션에는 프로젝트 코딩 표준이 있지만, 아직 코드 문서화에 대한 요구 사항은 없습니다. 다음 단계에서 TSDoc 문서 주석과 파일 머리 주석에 관한 규칙을 추가합니다.

## 새로운 리포지토리 표준 추가하기

다음 단계에서는 이 연습에서 수행할 유일한 편집 작업으로 코드에 TSDoc 문서 주석과 파일 머리 주석을 추가하도록 프로젝트 전체 규칙을 지정합니다. 이후 단계에서 cloud agent에 이슈를 할당하면 이 지침을 따릅니다.

먼저 작업 브랜치를 준비합니다. cloud agent는 이슈가 대상으로 지정한 브랜치의 지침을 읽지만, 변경 사항은 별도로 커밋합니다.

1. Codespace 터미널에서 새 브랜치를 만들고 전환합니다.

   ```bash
   git checkout -b custom-instructions-cloud
   ```

앞서 설명했듯 `.github/copilot-instructions.md`는 Copilot에 프로젝트 수준 정보를 제공합니다. 코드 제안을 개선하도록 리포지토리 코딩 표준을 문서화합니다.

1. `.github/copilot-instructions.md`를 다시 엽니다.
2. 27행 근처의 **Code formatting requirements** 섹션을 찾습니다. 프로젝트의 코딩 표준이 있지만 코드 문서화 규칙은 아직 없다는 점을 확인합니다.
3. 기존 표준 바로 아래에 다음 Markdown을 추가하여 파일 머리 주석과 TSDoc 문서 주석을 요청합니다.

   ```markdown
   - 내보내는 모든 함수에는 목적, 매개 변수, 반환 값을 설명하는 TSDoc 주석을 작성합니다.
   - 가져오기 구문이나 코드를 작성하기 전에 파일의 목적을 설명하는 주석 블록을 추가합니다.
   ```

4. `copilot-instructions.md`를 저장합니다.

> [!TIP]
> 앞선 레슨에서 살펴봤듯이 지침 파일은 전역 지침을 위한 리포지토리 수준 파일(`.github/copilot-instructions.md`) 또는 특정 언어, 파일 유형, 작업을 위한 `*.instructions.md` 파일로 만들 수 있습니다. 문서 주석 규칙과 같은 프로젝트 전체 표준은 리포지토리 수준 파일에 작성하는 것이 적절합니다.

## 지침 업데이트 커밋, 푸시, 병합하기

Cloud agent는 이슈가 대상으로 지정한 브랜치의 지침 파일을 읽습니다. 다음 연습에서 이슈를 Copilot에 할당하면 `main`에서 브랜치를 만들기 때문에, cloud agent가 지침을 사용하려면 변경 사항을 `main`에 병합해야 합니다.

1. 스테이징하고 커밋합니다.

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 브랜치를 푸시합니다.

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com에서 `custom-instructions-cloud` 브랜치에서 `main` 브랜치로 풀 리퀘스트를 열고 병합합니다. 다음 연습에서 이슈를 할당하면 cloud agent가 이 지침을 사용합니다.

> [!TIP]
> 워크숍에서 `main` 브랜치로 직접 작업하려면 브랜치를 만들지 않고 건너뛰어도 됩니다. 실제 프로젝트에서 작업하는 방식을 보여 주기 위해 브랜치 단계를 포함했습니다.

## 요약 및 다음 단계

이 프로젝트에서 Copilot이 지침 파일을 활용하는 방식을 살펴보고 `.github/copilot-instructions.md`에 리포지토리 전체 표준을 추가했습니다. 이후 cloud agent가 생성하는 풀 리퀘스트에서 이 표준이 적용되는 모습을 확인합니다.

다음으로 [첫 이슈를 Copilot cloud agent에 할당합니다][next-lesson].

## 리소스

- [GitHub Copilot 사용자 지정 지침 파일][instruction-files]
- [지침 파일 작성 모범 사례][instructions-best-practices]
- [Copilot을 위한 더 나은 사용자 지정 지침 작성 팁 5가지][copilot-instructions-five-tips]
- [Awesome Copilot — 지침 파일 및 기타 리소스 모음][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
