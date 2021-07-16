---
title: 노션을 깃허브 블로그로 옮기기
excerpt: 노션(Notion)을 깃허브 블로그(Github io)로 쉽게 옮기는 법을 알아보자.
categories:
  - posts
tags:
  - Notion
  - Github
---
# 노션을 깃허브 블로그로 옮기기

노션 글을 깃허브로 쉽게 옮기고자 py파일을 만들었습니다.

관련 파일은 다음 깃허브에서 다운받을 수 있습니다.

다른 언어보다 python이 수정에 용이해 python으로 제작하였습니다.

[https://github.com/JinseongP/notion2githubpages](https://github.com/JinseongP/notion2githubpages)

다음 깃허브에서 shell로 노션을 깃허브로 옮기는 프로그램을 가이드라인으로 잡았고([shell implementation](https://github.com/uoneway/notion-to-github-pages)) 다음 글을 참조해 예외사항을 수정했습니다([guideline](https://swieeft.github.io/2020/03/02/NotionToGithubioPorting.html)). Notion을 Tistory로 옮기고자 하는 분들은 다음 깃허브를 참조하시면 됩니다([notion2tistory](https://www.notion.so/Notion2Tistory-f46185df1db14f8eb571d366b66c5e9c)).

사용법은 다음과 같습니다.

1. [노션 페이지] 노션 페이지에 들어가 'ㆍㆍㆍ' 버튼(우상단)을 클릭합니다.

![Untitled.png]({{site.url}}/assets/images/2021-7-16-노션을 깃허브 블로그로 옮기기/Untitled.png){:.center-image}

1. [노션 페이지] 'Export' 버튼을 누르고 'Export format'으로 Markdown & CVS 을 선택하고 'Include content' 을 everything로 설정합니다.
2. github io repo의 root 폴더에 다운받은 zip파일들(여러 개 가능)을 옮깁니다. (파일 이름 변경 금지)
3. notion2githubpages.py를 같은 root folder에 복사 후 'python notion2githubpages.py'를 입력합니다.
4. Title(제목), Excerpt(발췌-제목 아래에 표시되는 내용), Category(카테고리)와 Tags(태그, '/'로 구분해서 여러 개 입력 가능)을 입력합니다.
5. 자동적으로 md 파일은 posts_folder_path='_posts/'에 저장되고 이미지는 images_folder_path='assets/images/'에 저장됩니다.
