---
title: "레슨 5 - quality-checks 스킬 사용자 지정 및 사용하기"
description: "기존 quality-checks 스킬을 살펴보고 결과 보고 형식을 사용자 지정한 뒤 필터링 기능을 검증하는 데 사용합니다."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

코드 작성은 개발 작업의 일부일 뿐입니다. 코드를 직접 검증하고 지침 파일로 코딩 표준을 적용했습니다. 이제 테스트, 린트, 지속적 통합(CI)의 다른 부분을 살펴봅니다.

이런 작업에는 **에이전트 스킬**이 적합합니다. 스킬을 사용하면 Copilot이 작업을 올바르게 실행하는 방법을 알 수 있습니다.

이 레슨에서는 다음을 수행합니다.

- 기존 `quality-checks` 스킬을 살펴봅니다.
- 결과 보고 형식을 사용자 지정합니다.
- 스킬을 새로 불러오고 실행합니다.

## 시나리오

Tailspin Toys는 단위 테스트, 린트, 타입 검사를 위해 `quality-checks` 스킬을 사용합니다. 팀은 결과를 더 쉽게 읽을 수 있도록 보고 형식을 개선하려고 합니다.

## 지침, 스크립트, 리소스

에이전트 스킬은 재사용할 수 있는 작업 지침, 실행 가능한 스크립트, 지원 리소스를 패키지로 제공합니다. 스킬은 기본적으로 이름이 같은 폴더에 있는 `SKILL.md` 파일로 구성됩니다. Markdown 파일에는 스킬의 이름과 설명, 역할을 소개하고 사용할 시점을 안내하는 frontmatter와 지침이 포함됩니다. 폴더에 스크립트와 기타 리소스를 넣을 수도 있습니다.

> [!NOTE]
> 스킬에 추가 파일이나 폴더가 꼭 필요한 것은 아닙니다. Tailspin Toys의 `quality-checks` 스킬은 프로젝트에서 기존 명령을 사용하므로 `SKILL.md` 파일만 포함합니다.

스킬을 프로젝트의 `.github/skills` 폴더에 저장하면 팀에서 공유하고 재사용할 수 있는 리포지토리 자산이 됩니다. 사용자 스킬 폴더인 `~/.copilot/skills`에 저장할 수도 있습니다.

## 스킬 살펴보기

Tailspin Toys 팀이 단위 테스트, 린트, 타입 검사를 실행하기 위해 만든 `quality-checks` 스킬을 살펴봅니다.

1. Codespace로 돌아갑니다. Codespaces 편집기에서 `.github/skills/quality-checks/SKILL.md`를 엽니다.
2. 맨 위의 `name` 및 `description`을 읽습니다. 설명은 Copilot이 스킬을 언제 사용할지 판단하는 데 도움을 줍니다.
3. 지침을 읽고 Copilot이 테스트와 린트 작업을 진행하도록 안내하는 방식을 살펴봅니다.
4. 아직 **결과 보고 형식** 섹션이 없다는 점을 확인합니다.

## 변경 전에 스킬 실행하기

Copilot CLI에서 스킬을 직접 호출하거나 자연어로 요청할 수 있습니다. Copilot에 세 가지 검사를 실행하도록 요청합니다.

1. Interactive 모드의 필터링 대화로 돌아갑니다.
2. 다음 프롬프트를 입력합니다.

   ```plaintext
   quality-checks 스킬을 실행해 단위 테스트, 린트, 타입 검사를 수행해 주세요.
   ```

3. 마지막에 표시되는 보고서를 살펴봅니다.

## 결과 보고 형식 사용자 지정하기

무엇을 실행했는지, 성공했는지, 도구에서 어떤 결과를 받았는지 알려 주는 보고서를 작성하도록 스킬을 업데이트합니다.

1. `.github/skills/quality-checks/SKILL.md`로 돌아갑니다.
2. 파일 끝에 다음 섹션을 추가합니다.

   ```markdown
   ## 결과 보고 형식

   작업이 끝나면 실행한 각 명령과 통과, 실패, 차단 여부를 보고합니다. 도구에서 보고한 경우에만 테스트 수, 실행 시간, 오류, 경고, 기타 측정값을 포함합니다. 실패 또는 차단된 항목에 대한 다음 조치를 제시하고, 건너뛰거나 완료되지 않은 검사를 통과한 것으로 설명하지 않습니다.
   ```

3. 파일이 자동으로 저장됩니다.

## 업데이트한 스킬 실행하기

변경 사항이 실제 실행 결과에 반영되는지 확인합니다. Copilot CLI는 대화를 다시 시작하지 않아도 편집된 스킬을 새로 불러올 수 있습니다.

1. 다음을 입력합니다.

   ```plaintext
   /skills reload
   ```

2. 이전과 동일한 프롬프트를 입력합니다.

   ```plaintext
   quality-checks 스킬을 실행해 단위 테스트, 린트, 타입 검사를 수행해 주세요.
   ```

3. 마지막에 표시되는 보고서를 살펴보고 첫 번째 보고서와 비교합니다.

## 요약 및 다음 단계

기존 에이전트 스킬을 사용자 지정하고 사용했습니다. 이 레슨에서는 다음을 수행했습니다.

- 단위 테스트, 린트, 타입 검사를 위한 `quality-checks` 스킬을 살펴봤습니다.
- 결과 보고 형식을 사용자 지정했습니다.
- 스킬을 새로 불러오고 실행했습니다.

이 변경 사항은 필터링 기능 PR에 포함됩니다. 다음으로 [Playwright MCP 서버][next-lesson]를 사용해 Copilot이 사이트와 상호 작용하도록 합니다.

## 추가 스킬 예제

다음 커뮤니티 예제는 참고 자료이며 추가 실습은 아닙니다.

- [Agent Skills 사양][skill-spec]
- [기여 워크플로: `make-repo-contribution`][contribution-example]
- [요구 사항 문서: `prd`][prd-example]
- [다이어그램 및 내보내기 스크립트: `drawio`][drawio-example]
- [브라우저 테스트: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
