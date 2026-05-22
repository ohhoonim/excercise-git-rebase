# Git rebase 활용 사례 연구

Github : [Exercise Git Rebase](https://github.com/ohhoonim/excercise-git-rebase.git)
https://github.com/ohhoonim/excercise-git-rebase.git

### 요약

1. **히스토리 단순화** – merge commit 없이 직선형 히스토리 유지
2. **기능 브랜치 최신화** – 최신 main 위로 재배치
3. **커밋 메시지 정리** – squash & reword로 의미 있는 커밋 단위 구성
4. **오픈소스 기여** – upstream main에 rebase하여 깔끔한 PR 준비
5. **리뷰 친화적인 로그** – 작은 커밋 합치기, 메시지 수정, 최신 main으로 재배치

---
## 1. **히스토리 단순화**

- `merge`는 브랜치가 합쳐질 때 *merge commit*을 남깁니다.
- 반면 `rebase`는 브랜치의 커밋들을 다른 브랜치 위로 "재배치"하여 **직선형 히스토리**를 만듭니다.
- 결과적으로 `git log`가 깔끔해지고, 프로젝트 진행 흐름을 이해하기 쉬워집니다.
- workflow 예시
    
    **상황:** `feature` 브랜치를 `main`에 합치려는 경우
    
    ```jsx
    # feature 브랜치에서 작업 중
    git checkout feature
    
    # main 최신 상태 가져오기
    git fetch origin
    
    # feature 브랜치를 main 위로 재배치
    git rebase origin/main
    
    # 충돌 해결 후 rebase 계속 진행
    git add .
    git rebase --continue
    
    # 최종적으로 main에 병합 (fast-forward)
    git checkout main
    git merge feature
    ```
    
     결과: merge commit 없이 직선형 히스토리 유지.
    

---

## 2. **기능 브랜치 정리**

- 기능 개발 중 `main` 브랜치가 업데이트되면, `merge` 대신 `rebase`를 사용해 최신 `main` 위로 기능 브랜치를 재배치합니다.
- 예시:
    
    ```bash
    git checkout feature
    git fetch origin
    git rebase origin/main
    ```
    
- 이렇게 하면 기능 브랜치가 최신 상태를 기반으로 하여 충돌 관리가 쉬워지고, 최종 병합 시 불필요한 merge commit이 사라집니다.
- workflow 예시
    
    **상황: 기능 개발 중 main 브랜치가 업데이트됨**브랜치를 `main`에 합치려는 경우
    
    ```bash
    git checkout feature
    git fetch origin
    git rebase origin/main
    ```
    
     기능 브랜치가 최신 main 기반으로 정리되어, 최종 병합 시 불필요한 merge commit 제거.
    

---

## 3. **커밋 메시지 정리 (Interactive Rebase)**

- `git rebase -i`를 사용하면 커밋을 **squash**하거나 **reword**할 수 있습니다.
- 활용:
    - 여러 작은 커밋을 하나로 합쳐 "의미 있는 단위"로 정리
    - 불필요한 커밋 제거
    - 커밋 메시지를 일관성 있게 수정
- 예시:
    
    ```bash
    git rebase -i HEAD~5
    → 최근 5개의 커밋을 정리 가능
    ```
    
- workflow 예시
    
    **상황:** 작은 커밋 여러 개를 하나로 합치고 싶을 때
    
    ```bash
    # 최근 5개 커밋 정리
    git rebase -i HEAD~5
    ```
    
    - 에디터에서 `pick` → `squash`로 변경
    - 메시지 수정 후 저장
    👉 의미 있는 단위로 커밋을 정리해 깔끔한 로그 생성.

---

## 4. **리뷰 친화적인 로그 만들기**

- 팀 코드 리뷰 시, `merge`로 인한 복잡한 히스토리보다 `rebase`로 정리된 직선형 로그가 훨씬 읽기 쉽습니다.
- 특히 PR(풀 리퀘스트)에서 "기능 단위"로 깔끔하게 정리된 커밋은 리뷰어가 변경 사항을 이해하기 쉽게 해줍니다.
- workflow 예시
    
    **상황:** PR 제출 전 커밋 정리
    
    ```bash
    # 기능 브랜치에서 커밋 정리
    git rebase -i HEAD~10
    
    # 불필요한 커밋 제거, 메시지 수정
    # 이후 최신 main 위로 rebase
    git fetch origin
    git rebase origin/main
    
    # 깔끔한 커밋 로그로 PR 생성
    git push origin feature --force
    ```
    
     결과: 리뷰어가 보기 좋은 직선형 히스토리와 정리된 커밋 메시지 제공.
    

---

## 5. **오픈소스 협업에서의 필수 패턴**

- 오픈소스 프로젝트에서는 기여자가 `merge` 대신 `rebase`를 활용해 커밋을 정리한 뒤 PR을 올리는 것이 일반적입니다.
- 이유: 프로젝트 메인 히스토리를 최대한 깔끔하게 유지하기 위해서.
- workflow 예시
    
    **상황:** 오픈소스 프로젝트에 PR 올리기 전
    
    ```bash
    # upstream 최신 가져오기
    git fetch upstream
    git rebase upstream/main
    
    # 충돌 해결 후 rebase 계속
    git add .
    git rebase --continue
    
    # 정리된 커밋을 내 fork에 반영
    git push origin feature --force
    ```
    
     결과: 오픈소스 프로젝트 메인 히스토리를 깔끔하게 유지.
    
