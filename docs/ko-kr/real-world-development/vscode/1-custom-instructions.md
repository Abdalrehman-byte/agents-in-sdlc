---
title: "연습 1 - 사용자 지정 지침(VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 이전 레슨: 사전 준비][previous-lesson] · [다음 레슨: 에이전트 모드 →][next-lesson]

생성형 AI를 사용할 때 컨텍스트가 중요합니다. 특정 방식으로 작업해야 하거나 Copilot이 알아야 할 배경 정보가 있다면 해당 정보를 사용할 수 있도록 해야 합니다. Copilot과 특정 컨텍스트를 공유하는 방법은 여러 가지가 있습니다. 그중 [지침 파일][instruction-files]은 코드 생성 방법을 안내하는 대표적인 방법입니다.

이 연습에서는 다음을 수행합니다.

- 리포지토리 사용자 지정 지침과 경로 범위 지침 파일을 통해 프로젝트 컨텍스트, 코딩 지침, 문서 표준이 Copilot에 전달되는 방식을 살펴봅니다.
- 현재 지침을 적용해 필터링을 위한 첫 데이터 기능인 퍼블리셔 도우미를 생성합니다.
- `.github/copilot-instructions.md`에 새로운 리포지토리 전체 표준을 추가합니다.
- 동일한 프롬프트를 다시 실행해 생성된 코드에 새 표준이 적용되는지 확인합니다.
- 지침 업데이트와 필터링 기능을 `main`에 커밋하여 다음 연습에서 업데이트된 지침을 사용할 수 있도록 합니다.

> [!CAUTION]
> 생성된 코드가 설정한 표준과 다를 수 있습니다. Copilot은 비결정적으로 작동합니다. 이 연습의 목적은 문자를 하나하나 일치시키는 것이 아니라 지침을 업데이트한 뒤 동작이 어떻게 달라지는지 확인하는 것입니다.

## 지침 파일

### 시나리오

Tailspin Toys에는 개발 관행에 관한 표준과 요구 사항이 있습니다.

- 데이터 계층에는 항상 단위 테스트가 필요합니다.
- UI는 어두운 모드와 현대적인 느낌을 사용해야 합니다.
- 코드에는 TSDoc 문서 주석을 추가해야 합니다.
- 각 파일의 맨 위에는 파일의 역할을 설명하는 주석 블록을 추가해야 합니다.

지침 파일을 사용해 Copilot이 이러한 관행에 따라 작업하는 데 필요한 정보를 제공합니다.

### 사용자 지정 지침

사용자 지정 지침은 Copilot에 컨텍스트와 선호 사항을 제공하여 코딩 스타일과 요구 사항을 더 잘 이해하도록 합니다. 코딩 규칙, 라이브러리, 포함하려는 주석 유형을 지정할 수 있습니다. 리포지토리 전체 또는 특정 파일 유형에 적용되는 작업별 지침 파일을 만들 수 있습니다.

지침 파일에는 다음 두 가지 유형이 있습니다.

- `.github/copilot-instructions.md`는 리포지토리의 **모든** 요청에 Copilot이 사용하는 단일 지침 파일입니다. 대부분의 Copilot Chat 또는 CLI 요청과 관련된 프로젝트 수준 컨텍스트를 작성합니다. 기술 스택, 개발 중인 프로젝트 개요, 모범 사례, 전역 지침 등을 포함할 수 있습니다.
- `.github/instructions/*.instructions.md` 파일은 특정 작업이나 파일 유형에 대한 지침을 제공합니다. TypeScript나 Astro 같은 특정 언어나 UI 컴포넌트 또는 새 단위 테스트 작성 같은 작업에 대한 지침을 만들 수 있습니다.

> [!NOTE]
> IDE에서는 지침 파일을 Copilot Chat의 코드 생성에만 사용하며, 코드 완성이나 다음 편집 제안에는 사용하지 않습니다.
>
> Copilot Chat, Copilot CLI, Copilot cloud agent는 코드 생성 시 리포지토리 수준 지침과 `applyTo` frontmatter가 있는 `*.instructions.md` 파일을 함께 사용합니다.
>
> Copilot은 AGENTS.md 및 CLAUDE.md 파일을 비롯한 다른 형식의 지침 파일도 [지원합니다][custom-instructions-support].

### 지침 파일 관리 모범 사례

지침 파일을 만드는 방법에 관한 자세한 설명은 이 워크숍의 범위를 벗어납니다. 샘플 프로젝트에는 일반적인 접근 방식을 보여 주는 예제가 포함되어 있습니다. 큰 틀에서는 다음과 같이 관리합니다.

- `copilot-instructions.md`에는 개발 중인 내용, 프로젝트 구조, 전역 코딩 표준 등 프로젝트 수준의 지침을 작성합니다.
- `*.instructions.md` 파일에는 단위 테스트, Astro 컴포넌트, 데이터 계층과 같은 파일 유형별 또는 특정 작업 지침을 작성합니다.
- 자연어로 지침을 명확하게 작성하고, 좋은 코드와 피해야 할 코드 예제를 제공합니다.

지침 파일을 만드는 한 가지 정답이나 AI를 사용하는 한 가지 정답은 없습니다. 직접 실험하며 프로젝트에 맞는 방법을 찾습니다.

> [!TIP]
> GitHub Copilot을 사용하는 모든 프로젝트에는 체계적인 지침 파일 모음이 필요합니다. 이 프로젝트에는 [UI 업데이트][ui-instructions] 및 [Astro][astro-instructions]를 비롯한 여러 작업에 대한 지침 파일이 있습니다.
>
> Copilot에 지침 파일 생성을 요청할 수도 있습니다. 환경마다 제공 방법이 다릅니다. 예를 들어 VS Code에서는 **Configure Chat → Generate Agent Instructions**, Copilot CLI에서는 `/init`을 사용할 수 있습니다. 사용 중인 환경의 레슨에서 관련 방법을 안내합니다.
>
> 시작 템플릿을 찾고 있다면 지침 파일, 사용자 지정 에이전트, 기타 리소스가 있는 [awesome-copilot][awesome-copilot] 리포지토리를 살펴봅니다.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 이 프로젝트의 사용자 지정 지침 파일 살펴보기

이 리포지토리에는 핵심 `copilot-instructions.md` 파일 하나와 다양한 작업을 위한 `*.instructions.md` 파일 모음이 있습니다. 편집기 또는 GitHub 웹 UI에서 파일을 엽니다.

1. `.github/copilot-instructions.md`를 엽니다.
2. 프로젝트에 대한 간단한 설명과 **Agent notes**, **Code standards**, **Scripts**, **Repository Structure** 같은 섹션을 살펴봅니다. **Code standards** 아래의 **GitHub Actions Workflows** 지침도 확인합니다. 이 지침은 Copilot과 상호 작용할 때마다 적용됩니다.
3. `.github/instructions` 폴더를 열어 파일을 살펴봅니다. Astro 파일, Drizzle 데이터 계층, 테스트 등에 대한 지침이 있습니다.
4. `.github/instructions/unit-tests.instructions.md`를 엽니다. 맨 위의 `applyTo` 필드는 리포지토리 루트에 상대적인 glob을 지정합니다. 예를 들어 `**/*.test.ts`와 일치하는 TypeScript 테스트 파일에 지침이 적용됩니다.
5. 단위 테스트 작성에 관한 프로젝트별 지침을 확인합니다.
6. 마지막으로 `.github/instructions/drizzle.instructions.md`를 열고 맨 아래로 이동합니다. `unit-tests.instructions.md` 같은 다른 지침 파일과 프로젝트 파일을 가리키는 링크를 확인합니다. 이를 사용해 큰 지침을 작고 재사용 가능한 파일로 나누고 Copilot이 참고할 예제를 지정할 수 있습니다. 이 파일의 경로는 리포지토리 루트가 아니라 지침 파일 위치를 기준으로 합니다.

> [!NOTE]
> `copilot-instructions.md`의 **Code formatting requirements** 섹션에는 프로젝트 코딩 표준이 있지만, 아직 코드 문서화에 대한 요구 사항은 없습니다. 다음 단계에서 TSDoc 문서 주석과 파일 머리 주석에 관한 규칙을 추가합니다.

## 변경 사항을 위한 브랜치 만들기

변경 사항을 별도 브랜치에서 관리합니다.

1. 이전 연습에서 사용한 Codespace로 돌아갑니다.
2. Mac에서는 <kbd>Control</kbd>+<kbd>\`</kbd>를, Windows/Linux에서는 <kbd>Ctrl</kbd>+<kbd>\`</kbd>를 눌러 새 터미널을 엽니다.
3. 새 브랜치를 만들고 전환합니다.

   ```bash
   git checkout -b custom-instructions
   ```

## 지침 업데이트 *전* Copilot Chat 사용하기

> [!TIP]
> **Copilot Chat 열기**
>
> 아래 연습을 시작하기 전에 Codespace로 돌아가 Copilot Chat 패널을 열고 **New Chat**을 선택하여 새 대화를 시작합니다. 모드와 모델 선택은 연습마다 다를 수 있습니다. 관련 단계에서 안내합니다.

사용자 지정 지침의 영향을 확인하려면 먼저 현재 지침이 적용된 상태에서 프롬프트를 보냅니다. 다음 단계에서 지침을 업데이트한 뒤 동일한 프롬프트를 다시 보내 결과를 비교합니다.

1. 이전 연습에서 열었던 편집기 탭을 닫아 Copilot이 필요한 컨텍스트만 사용하도록 합니다.
2. Copilot이 도우미를 작성할 위치를 알 수 있도록 `src/lib/publishers.ts`를 엽니다.
3. Chat 보기의 에이전트 드롭다운에서 **Agent**를 선택하여 Copilot이 파일을 수정할 수 있도록 합니다.

   ![Chat 보기의 에이전트 선택기를 보여 주는 스크린샷](../../../_images/shared-chat-mode-selector.png)

4. 다음 프롬프트를 입력합니다.

   ```plaintext
   src/lib/publishers.ts에 이름과 ID를 포함한 모든 퍼블리셔 목록을 반환하는 데이터 접근 도우미를 만들거나 업데이트해 주세요. 파일에 변경 사항을 적용해 주세요.
   ```

5. Copilot이 프로젝트를 살펴보고 도우미 파일과 테스트 등을 업데이트합니다.
6. 제안된 도우미가 첫 번째 인수로 `db` 클라이언트를 받고 형식이 지정된 퍼블리셔 배열을 반환하는지 살펴봅니다. 이는 `src/lib/*.ts`에 적용되는 `.github/instructions/drizzle.instructions.md`의 데이터 계층 규칙에 따른 것입니다.
7. 제안된 코드에 TSDoc 문서 주석과 파일 머리 주석이 **없는지** 확인합니다.

> [!CAUTION]
> Copilot은 확률적으로 작동하므로 지침이 없어도 문서 주석을 추가할 수 있습니다. 이 경우에도 지침을 업데이트한 뒤 출력의 일관성이 어떻게 개선되는지 살펴보면 됩니다.

## 새로운 리포지토리 표준 추가하기

`.github/copilot-instructions.md`는 Copilot에 프로젝트 수준 정보를 제공합니다. 코드 제안을 개선하기 위해 리포지토리 코딩 표준에 문서 주석 규칙을 추가합니다.

1. `.github/copilot-instructions.md`를 다시 엽니다.
2. 27행 근처의 **Code formatting requirements** 섹션을 찾습니다. 코드 생성 도우미에 문서 주석이 없었던 이유는 아직 이 규칙이 없기 때문입니다.
3. 기존 표준 바로 아래에 다음 Markdown을 추가하여 파일 머리 주석과 TSDoc 문서 주석을 요청합니다.

   ```markdown
   - 내보내는 모든 함수에는 목적, 매개 변수, 반환 값을 설명하는 TSDoc 주석을 작성합니다.
   - 가져오기 구문이나 코드 앞에 파일의 목적을 설명하는 주석 블록을 추가합니다.
   ```

4. `copilot-instructions.md`를 저장합니다.

> [!TIP]
> 앞선 레슨에서 살펴봤듯이 리포지토리 수준 파일(`.github/copilot-instructions.md`)에는 전역 지침을, `*.instructions.md` 파일에는 특정 언어, 파일 유형, 작업 지침을 작성할 수 있습니다. 문서 주석 규칙과 같은 프로젝트 전체 표준은 리포지토리 수준 파일에 두는 것이 적절합니다.

## 프롬프트를 다시 실행하고 변경 사항 살펴보기

1. Copilot Chat으로 돌아가 **New Chat**을 선택해 대화 컨텍스트를 비웁니다.
2. `src/lib/publishers.ts`를 다시 선택해 Copilot이 올바른 파일을 대상으로 하도록 합니다.
3. 이전과 **동일한 프롬프트**를 입력합니다.

   ```plaintext
   src/lib/publishers.ts에 이름과 ID를 포함한 모든 퍼블리셔 목록을 반환하는 데이터 접근 도우미를 만들거나 업데이트해 주세요. 파일에 변경 사항을 적용해 주세요.
   ```

4. 제안된 파일 맨 위에 다음과 비슷한 주석 블록이 추가되었는지 확인합니다.

   ```typescript
   /**
    * Tailspin Toys 크라우드 펀딩 플랫폼의 퍼블리셔 데이터 접근 도우미입니다.
    * 데이터베이스에서 퍼블리셔 정보를 가져오는 기능을 제공합니다.
    */
   ```

5. 제안된 함수에 다음과 비슷한 TSDoc 주석이 추가되었는지 확인합니다.

   ```typescript
   /**
    * ID와 이름을 포함한 모든 퍼블리셔 목록을 반환합니다.
    *
    * @param db - Drizzle 데이터베이스 클라이언트입니다.
    * @returns 퍼블리셔 객체 배열을 반환하는 Promise입니다.
    */
   ```

새 프로젝트 표준을 추가하여 Copilot을 안내하고 다음 연습에서 사용할 실제 코드에 적용했습니다.

## 지침 변경과 브랜치 푸시하기

지침 파일도 다른 리포지토리 자산과 마찬가지로 버전 관리합니다. 변경 사항을 커밋하고 브랜치를 푸시합니다.

1. Mac에서는 <kbd>Control</kbd>+<kbd>\`</kbd>를, Windows/Linux에서는 <kbd>Ctrl</kbd>+<kbd>\`</kbd>를 눌러 Codespace에서 새 터미널 창을 엽니다.
2. 다음 명령을 실행해 지침 변경과 도우미 파일이 있는지 확인합니다.

   ```bash
   git status
   ```

3. 터미널에서 지침 변경, 도우미 코드, Copilot이 수정한 테스트 등 관련 파일을 스테이징하고 커밋합니다.

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status에 테스트 같은 지원 변경 사항이 표시되면 해당 파일도 추가합니다.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 브랜치를 리포지토리에 푸시합니다.

   ```bash
   git push -u origin custom-instructions
   ```

## 풀 리퀘스트 만들고 병합하기

브랜치를 푸시했으니 지침 표준 이슈와 연결된 풀 리퀘스트를 만듭니다. 직접 만들 수도 있지만 이미 연결된 GitHub 도구를 사용해 Copilot에 이슈를 찾고 이슈를 해결하는 PR을 만든 뒤 병합하도록 요청합니다.

> [!NOTE]
> 이 연습은 Copilot이 사용자를 대신해 GitHub 작업을 수행하는 첫 번째 단계입니다. 프로젝트 템플릿에 GitHub 도구가 이미 연결되어 있어 별도로 설정할 필요가 없습니다. Copilot이 처음으로 도구를 사용하면 VS Code에서 GitHub 로그인을 요청할 수 있습니다. 안내에 따라 로그인합니다. 이후 연습에서 Model Context Protocol을 살펴봅니다.

1. Codespace에서 Copilot Chat을 엽니다.
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>(Mac) 또는 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>(Windows/Linux)를 눌러 Copilot Chat 보기를 열고 에이전트 선택기에서 **Agent**가 선택되었는지 확인합니다.
3. Copilot에 지침 파일 업데이트와 필터링 작업을 위한 퍼블리셔 도우미를 설명하는 PR을 현재 브랜치에서 만들고 해당 이슈를 해결하도록 요청합니다.

   ```
   지침 파일 업데이트와 관련된 이슈를 찾아 주세요. 현재 브랜치에서 새 PR을 만들고 해당 이슈를 해결한다고 명시해 주세요. 다음 필터링 작업을 위한 퍼블리셔 도우미 기반을 추가한 내용도 포함해 주세요.
   ```

4. Copilot이 이슈를 찾고 PR을 만드는 과정을 살펴봅니다.
5. Copilot이 GitHub 작업을 대신 수행하도록 **Allow**를 요청하면 명령을 검토하고 허용할지 결정합니다.
6. PR이 만들어지면 Copilot에 병합한 뒤 로컬 브랜치를 `main`으로 전환하도록 요청합니다.

   ```
   PR을 main에 병합해 주세요. 그런 다음 로컬에서 main으로 돌아가 최신 코드를 가져와 최신 상태로 맞춰 주세요.
   ```

7. Copilot이 GitHub 작업과 셸 명령을 수행하도록 **Allow**를 요청하면 명령을 검토하고 허용할지 결정합니다.

GitHub Copilot의 도움으로 풀 리퀘스트를 만들고 병합했습니다.

## 요약 및 다음 단계

이 프로젝트에서 Copilot이 지침 파일을 활용하는 방식을 살펴본 뒤 VS Code에서 Copilot Chat을 사용해 다음을 수행했습니다.

- 현재 지침이 적용된 코드 생성 프롬프트를 보내 결과를 살펴봤습니다.
- `.github/copilot-instructions.md`에 새로운 리포지토리 전체 표준을 추가했습니다.
- 동일한 프롬프트를 다시 실행해 새 표준이 코드에 반영되는지 확인했습니다.
- 지침 업데이트와 퍼블리셔 도우미를 `main`에 커밋하여 다음 연습에서 사용할 수 있도록 했습니다.

다음으로 [에이전트 모드][next-lesson]에서 지침을 사용하여 코드베이스 전반에 걸친 기능을 추가합니다.

## 리소스

- [GitHub Copilot 사용자 지정 지침 파일][instruction-files]
- [사용자 지정 지침 작성 모범 사례][instructions-best-practices]
- [Copilot을 위한 더 나은 사용자 지정 지침 작성 팁 5가지][copilot-instructions-five-tips]
- [GitHub Copilot 개인 사용자 지정 지침][personal-instructions]
- [Awesome Copilot — 지침 파일 및 기타 리소스 모음][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
