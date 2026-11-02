---
title: "연습 3 - 사용자 지정 에이전트"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 사용자 지정 에이전트란?

GitHub Copilot의 [사용자 지정 에이전트][custom-agents-concept]는 개발 워크플로의 특정 작업이나 영역에 맞춘 전문 AI 도우미를 만들 수 있게 합니다. 리포지토리의 `.github/agents` 폴더에 Markdown 파일로 에이전트를 정의하면, Copilot이 특정 유형의 작업을 더 효과적으로 수행하도록 집중된 지침, 모범 사례, 코딩 패턴, 도메인 지식을 제공할 수 있습니다. 팀은 접근성 규정을 적용하는 에이전트, [WCAG][wcag]를 따르는 에이전트, 보안 코딩 관행을 적용하는 에이전트, 일관된 테스트 패턴을 유지하는 에이전트처럼 전문 지식을 재사용 가능한 에이전트로 정리할 수 있습니다.

사용자 지정 에이전트는 프로젝트의 `.github/agents` 폴더 또는 전역 `~/.copilot/agents` 폴더에 Markdown 파일로 정의합니다. 각 파일에는 최소한 `name`과 `description`이 포함된 YAML frontmatter가 있고, 그 뒤에 에이전트의 동작 방식, 전문 지식, 지침을 정의하는 Markdown 프롬프트가 이어집니다.

### 사용자 지정 에이전트와 에이전트 스킬 비교

사용자 지정 에이전트와 [에이전트 스킬][agent-skills-concept]은 일부 역할이 겹칩니다. 둘 다 주로 Markdown 파일로 정의하며 AI에 작업 수행 방법을 알려 줍니다. 간단하게 구분하면 **사용자 지정 에이전트는 작업자이고 스킬은 도구**입니다.

사용자 지정 에이전트에는 자체 컨텍스트 창이 있으며, 작업을 수행할 때 스킬이나 다른 에이전트를 조율할 수 있습니다. 이 실습의 접근성 사용자 지정 에이전트는 접근성 지침에 따라 사이트를 검토하고 업데이트합니다. 작업의 일부로 풀 리퀘스트 워크플로 스킬이나 테스트를 실행하고 관리하는 스킬을 호출할 수 있습니다.

> [!NOTE]
> 사용자 지정 에이전트를 작성하는 하나의 정답은 없습니다. AI를 사용하는 다른 작업과 마찬가지로 환경과 시나리오에 맞게 테스트하고 반복 개선합니다.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

사용자 지정 에이전트로 다음을 살펴봅니다.

- 사용자 지정 에이전트를 정의하는 방법
- 사용자 지정 에이전트에 작업을 할당하는 방법

## 시나리오

Tailspin Toys는 시각 능력이나 선호도에 관계없이 모든 사용자가 크라우드 펀딩 플랫폼을 이용할 수 있도록 노력하고 있습니다. 최근 사용자 의견에 따르면 텍스트와 배경 색상의 대비가 충분하지 않아 일부 사용자가 현재 어두운 테마를 읽기 어렵다고 합니다. 이에 디자인 팀은 사용자가 켜고 끌 수 있는 고대비 모드 구현을 요청했습니다.

접근성은 중요하므로 이 기능을 가능한 한 빨리 구현하려고 합니다. 사용자 지정 에이전트를 활용해 기능을 생성합니다.

## 접근성 사용자 지정 에이전트 검토하기

접근성 작업을 위한 사용자 지정 에이전트가 이미 준비되어 있습니다. 내용을 검토하여 Copilot을 어떻게 안내하는지 확인합니다.

Codespace로 돌아가 접근성 사용자 지정 에이전트 파일을 검토합니다.

1. `.github/agents/accessibility.md`를 엽니다.
2. `name` 및 `description` 필드가 있는 YAML frontmatter를 확인합니다.

> [!CAUTION]
> 사용자 지정 에이전트에는 `name` 및 `description` frontmatter가 필요합니다.

3. 이어지는 섹션을 살펴보고 다음 내용을 확인합니다.
   - 접근성 있는 웹사이트를 위한 코드 생성 시 핵심 책임
   - 접근성 모범 사례
   - HTML, CSS, JavaScript 코드 예제
   - 흔히 발생하는 문제와 실수

## 이슈 만들고 Copilot에 할당하기

제어 센터(Mission control)는 환경의 모든 에이전트와 작업하는 중심 공간입니다. Copilot cloud agent에 작업을 할당하고 작업을 모니터링하며 추가 지침을 보낼 수 있습니다. 먼저 접근성 에이전트에 고대비 모드 구현 작업을 할당합니다.

1. 리포지토리로 이동합니다.
2. **Issues** 탭을 선택합니다.
3. **New issue**를 선택하여 새 이슈 대화 상자를 엽니다.
4. **Blank issue**를 선택합니다.
5. **Title**에 `웹사이트에 고대비 모드 추가`를 입력합니다.
6. **Description**에 다음을 입력합니다.

    ```plaintext
    사이트에 고대비 모드를 추가해야 합니다. 사용자가 설정할 수 있는 고대비 토글이 있어야 하며, 브라우저의 로컬 스토리지에 설정을 저장해야 합니다.
    ```

7. **Create**를 선택하여 이슈를 만듭니다.
8. 오른쪽에서 **Assign to Copilot**을 선택하여 할당 대화 상자를 엽니다.
9. 목록에서 **Accessibility agent**를 선택합니다.

    ![고대비 모드 이슈에 접근성 에이전트를 할당하는 화면](../../../_images/ex5-select-custom-agent.png)

10. **Assign**을 선택합니다.
11. Copilot이 백그라운드에서 작업을 시작합니다.

## 요약 및 다음 단계

GitHub Copilot의 [사용자 지정 에이전트][custom-agents]는 특정 작업과 영역을 위한 전문 AI 도우미입니다. 사용자 지정 에이전트로 팀의 전문 지식과 표준을 재사용 가능한 형태로 정의해 Copilot이 특정 유형의 작업을 더 효과적으로 수행하도록 안내할 수 있습니다.

이 연습에서는 다음을 살펴봤습니다.

- 사용자 지정 에이전트가 정의되는 방식
- 사용자 지정 에이전트에 작업을 할당하는 방법

Copilot이 고대비 모드를 구현하는 동안 다음으로 [에이전트 세션을 모니터링하고 안내합니다][next-lesson].

## 리소스

- [사용자 지정 에이전트 정보][custom-agents]
- [조직에서 사용자 지정 에이전트 사용 준비하기][org-custom-agents]
- [엔터프라이즈에서 사용자 지정 에이전트 사용 준비하기][enterprise-custom-agents]

---

| [← 이전 레슨: GitHub Copilot cloud agent][previous-lesson] | [다음 레슨: 에이전트 모니터링 및 관리 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
