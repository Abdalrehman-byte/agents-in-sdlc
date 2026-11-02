---
title: "레슨 4 - 사용자 지정 지침으로 Copilot 안내하기"
description: "리포지토리 지침을 살펴보고 문서 표준을 추가한 뒤 필터링 코드에 적용합니다."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

생성형 AI를 사용할 때 컨텍스트가 중요합니다. 특정 방식으로 작업해야 한다면 Copilot이 해당 지침을 사용할 수 있어야 합니다. [지침 파일][instruction-files]은 원하는 코드뿐 아니라 코드 구성 방식도 설명합니다. 필터링 기능을 만든 뒤 Copilot이 사용한 지침을 살펴보고 문서 표준을 추가해 코드에 적용합니다.

이 레슨에서는 다음을 수행합니다.

- 리포지토리 지침 및 경로 범위 지침 파일이 에이전트에 전달되는 방식을 살펴봅니다.
- 코딩 표준을 지키도록 지침 파일을 업데이트합니다.
- 지침 파일이 코드에 미치는 영향을 확인합니다.

## 시나리오

Tailspin Toys에는 개발 관행을 위한 지침과 요구 사항이 있습니다.

- 주석은 코드 내용을 반복하지 않고 의도나 명확하지 않은 결정을 설명해야 합니다.
- `db/`와 `src/lib/`에서 내보내는 함수에는 목적, 매개 변수, 반환 값이 포함된 TSDoc/JSDoc을 작성해야 합니다. `db`를 인수로 전달할 수 있다면 문서에 이를 포함해야 합니다.
- 재사용 가능한 Astro 컴포넌트에는 `Props` 계약을 설명해야 하며, 관련 코드가 바뀌면 주석도 최신 상태로 유지해야 합니다.
- 기존 서식 및 린트 지침을 유지해야 합니다.

지침 파일을 사용해 Copilot이 이러한 관행에 맞게 작업하도록 안내합니다.

## 지침 파일

사용자 지정 지침은 Copilot에 컨텍스트와 선호 사항을 제공하여 코딩 스타일과 요구 사항을 더 잘 이해하도록 합니다. 코딩 규칙, 라이브러리, 포함하려는 주석 유형을 지정할 수 있습니다. 리포지토리 전체 또는 특정 파일 유형에 적용되는 작업별 지침 파일을 만들 수 있습니다.

지침 파일에는 두 가지 유형이 있습니다.

- `.github/copilot-instructions.md`는 리포지토리의 **모든** 요청에 Copilot이 사용하는 단일 지침 파일입니다. 대부분의 요청에 관련된 프로젝트 수준 정보를 작성합니다.
- `.github/instructions/*.instructions.md` 파일은 특정 언어, 파일 유형, 작업에 대한 지침을 제공합니다.

> [!NOTE]
> 지침 파일 형식과 지원 범위는 환경에 따라 다를 수 있습니다. 특정 형식에 의존하기 전에 [사용자 지정 지침 지원 참조][custom-instructions-support]를 확인합니다.

## 이 프로젝트의 사용자 지정 지침 파일 살펴보기

시작을 돕기 위해 스타터 프로젝트에 지침 파일 모음이 포함되어 있습니다. 변경 전에 기존 파일을 살펴보고 지침의 영향을 확인합니다.

1. Codespace로 돌아갑니다.
2. Codespaces 편집기에서 터미널이 아니라 `.github/copilot-instructions.md`를 엽니다.
3. 프로젝트에 대한 간단한 설명과 코딩 지침을 살펴봅니다. 이 지침은 리포지토리에서 Copilot과 상호 작용할 때마다 적용됩니다.
4. `.github/instructions` 폴더를 열어 파일을 살펴봅니다. Astro 파일, Drizzle 데이터 계층, 테스트 등에 대한 지침이 있습니다.
5. `.github/instructions/unit-tests.instructions.md`를 엽니다. 맨 위의 `applyTo` 필드는 지침을 적용할 파일을 결정하는 glob을 지정합니다.
6. `.github/instructions/drizzle.instructions.md`를 열고 다른 지침 파일 및 기존 프로젝트 파일을 가리키는 링크를 살펴봅니다. 이를 활용하면 큰 지침 모음을 작고 재사용 가능한 파일로 나누고 Copilot이 참고할 예제를 지정할 수 있습니다.

## 팀 지침에 맞게 지침 파일 업데이트하기

이미 포함된 파일은 좋은 출발점이지만 보완할 부분이 있습니다. 새 TypeScript 코드에 TSDoc 주석을 추가하도록 핵심 `copilot-instructions.md` 파일을 수정합니다.

1. `.github/copilot-instructions.md`에서 35행 근처에 있는 **Code formatting guidance** 섹션을 찾습니다.
2. 마지막 항목으로 다음을 추가합니다.

   ```markdown
   - 새 TypeScript 코드에는 문서화를 위한 TSDoc 주석을 작성합니다.
   ```

파일이 자동으로 저장됩니다.

## 업데이트한 지침 적용하기

Copilot CLI는 대화를 시작할 때 리포지토리 지침을 불러옵니다. 지침을 저장한 뒤 필터링 대화를 종료하고 다시 시작하면 기능 컨텍스트를 유지하면서 새 지침을 불러올 수 있습니다.

1. 필터링 대화가 아직 열려 있으면 Copilot CLI에서 `/exit`를 입력해 종료합니다. Copilot CLI는 대화를 자동으로 저장합니다.
2. `game-filters-cli` 브랜치의 리포지토리 루트에서 저장된 대화를 다시 시작합니다.

   ```bash
   copilot --resume
   ```

   세션 선택 화면이 표시되면 필터링 대화를 선택하고 `game-filters-cli` 브랜치가 체크아웃되어 있는지 확인합니다.
3. 다음 프롬프트로 Copilot에 업데이트된 지침을 적용하도록 요청합니다.

   ```plaintext
   방금 지침과 코드 작성 지침을 업데이트했습니다. 생성한 코드에 새 지침을 적용해 주시겠어요?
   ```

4. `/diff`를 입력하고 변경된 TypeScript 파일을 확인합니다. 새로 생성된 TSDoc 주석이 코드 동작을 정확히 설명하는지 살펴봅니다.

## 요약 및 다음 단계

Copilot이 지침 파일을 적용하는 방식을 살펴보고 새 프로젝트 표준을 추가했습니다. 이 레슨에서는 다음을 수행했습니다.

- 리포지토리 지침 및 경로 범위 지침 파일이 에이전트에 전달되는 방식을 살펴봤습니다.
- 코딩 표준을 지키도록 지침 파일을 업데이트했습니다.
- 지침 파일이 코드에 미치는 영향을 확인했습니다.

다음으로 [재사용 가능한 quality-checks 스킬을 사용자 지정하고 실행][next-lesson]하여 린트와 테스트를 일관되게 실행합니다.

## 리소스

- [GitHub Copilot 사용자 지정 지침 파일][instruction-files]
- [사용자 지정 지침 지원][custom-instructions-support]
- [사용자 지정 지침 작성 모범 사례][instructions-best-practices]
- [Awesome Copilot — 지침 파일 및 기타 리소스 모음][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
