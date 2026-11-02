---
title: "레슨 0 - 사전 준비"
description: "Tailspin Toys의 복사본을 만들고 Copilot CLI 워크숍에 사용할 GitHub Codespace를 준비합니다."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI 레슨을 시작하기 전에 필요한 사항을 준비합니다. Tailspin Toys 리포지토리의 복사본을 만들고 [Codespace][codespaces]를 시작합니다. 다음 레슨에서는 통합 터미널을 사용해 Copilot CLI를 설치하고 실행합니다.

이 레슨에서는 다음을 수행합니다.

- 템플릿에서 Tailspin Toys 프로젝트의 복사본을 만듭니다.
- Codespace를 만들고 프로젝트가 준비되었는지 확인합니다.

## 실습용 리포지토리 설정하기

Tailspin Toys 프로젝트의 복사본을 사용해 작업합니다. 지금 [템플릿 리포지토리][tailspin-template]에서 복사본을 만듭니다. 새 리포지토리에는 실습에 필요한 파일이 모두 포함되어 있습니다.

1. 새 브라우저 창에서 [Tailspin Toys 템플릿][tailspin-template]으로 이동합니다.
2. **Use this template**를 선택한 다음 **Create a new repository**를 선택하여 리포지토리 복사본을 만듭니다.
3. GitHub 또는 Microsoft가 진행하는 행사에서 워크숍을 수행하는 경우 멘토의 안내를 따릅니다. 그렇지 않다면 GitHub Copilot을 사용할 수 있는 조직에 새 리포지토리를 만듭니다.
4. 나중에 참조할 수 있도록 생성한 리포지토리 경로(`organization-or-user-name/repository-name`)를 기록해 둡니다.

> [!NOTE]
> 템플릿에서 리포지토리를 만들면 GitHub 이슈 백로그가 자동으로 생성됩니다. 워크숍 전체에서 해당 이슈를 사용하므로 직접 등록할 필요가 없습니다.

새 워크숍 템플릿 복사본을 사용합니다. 여기에는 리포지토리 지침, 애플리케이션 코드, 테스트, `quality-checks` 스킬, 실습에 사용할 백로그가 포함되어 있습니다. 이전 복사본을 사용한다면 필요한 파일이 있는지 진행자에게 확인합니다.

## Codespace 만들기

이제 Codespace에서 워크숍을 진행합니다.

[GitHub Codespaces][codespaces]는 브라우저에서 코드를 작성하고 실행하고 디버그할 수 있는 클라우드 기반 개발 환경입니다. 여러 프로그래밍 언어, 확장, 도구를 지원하는 완전한 기능의 편집기를 제공합니다.

1. 방금 만든 리포지토리로 이동합니다.
2. **Code**를 선택합니다.
3. **Codespaces** 탭을 선택한 다음 **Create codespace on main**을 선택합니다.
4. Codespace 설정이 완료될 때까지 기다립니다. 템플릿에서 프로젝트 종속성, Playwright Chromium, 로컬 데이터베이스를 설치합니다.
5. **Do you trust the authors of the files in this folder?**라는 메시지가 표시되면 **Trust Folder & Continue**를 선택합니다.
6. 리포지토리 루트에서 터미널을 열고 애플리케이션을 시작합니다.

   ```bash
   npm run dev
   ```

7. Codespaces에서 포트 `4321`을 사용할 수 있다고 표시되면 **Open in Browser**를 선택하고 Tailspin Toys 사이트가 로드되는지 확인합니다.
8. 터미널로 돌아가 <kbd>Ctrl</kbd>+<kbd>C</kbd>를 눌러 개발 서버를 중지합니다.

## 요약 및 다음 단계

준비가 끝났습니다. 이 레슨에서는 템플릿에서 Tailspin Toys 프로젝트의 복사본을 만들고 Codespace를 준비했습니다.

다음으로 Codespace에 [GitHub Copilot CLI를 설치][next-lesson]하고 GitHub 계정으로 인증합니다.

## 리소스

- [GitHub Codespaces 개요][codespaces]
- [템플릿에서 리포지토리 만들기][template-repository]
- [Codespaces 시작하기][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
