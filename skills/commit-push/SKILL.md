---
name: commit-push
description: "Stage, commit, or push the current work when the user requests it, including requests mixed into other instructions. Also use for commit message suggestions; not for read-only commit history inspection."
---

# Commit Push

## 요청과 범위
- 현재 브랜치, `git status`와 diff로 이번 작업 범위를 확인한다.
- 사용자가 `제목만`이나 메시지 추천만 요청하면 §메시지 템플릿으로 제목 후보를 내고 끝낸다.
- 사용자가 지정한 브랜치와 현재 브랜치가 다르면 변경 전에 묻는다.

## 실행
- 이번 작업 변경만 스테이징한다. 같은 파일에 섞인 다른 변경이나 이미 스테이징된 무관한 변경은 커밋에 포함하지 않는다.
- 스테이징만 요청했으면 거기서 끝내고, 커밋 요청이면 §메시지 템플릿으로 커밋한다.
- 푸시 요청이면 현재 브랜치의 upstream으로 푸시한다. upstream이 없거나 푸시가 거절되면 이유를 보고하고 멈춘다.

## 메시지 템플릿
```text
<제목: 달라지는 동작을 짧은 명사형으로 요약>

<본문: 이유가 제목에 다 드러나지 않을 때만. diff만으로 알 수 없는 이유·제약·버린 대안을 한 줄에 한 문장씩>
```
- `Co-Authored-By` 같은 attribution 줄은 붙이지 않는다.

## 보고
- 수행한 작업, 커밋했다면 SHA와 제목, 푸시했다면 대상 브랜치, 포함하지 않은 변경을 적는다.
