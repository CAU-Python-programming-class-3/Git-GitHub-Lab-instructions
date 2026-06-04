# 3주차 수업 환경 설정

> **소요 시간**: 약 20~25분  
> **진행 방식**: 교수님과 함께 라이브로 진행  
> **목표**: 2인 1조 협업 환경 구성 + 수업용 실습 지시서 수령

---

## 사전 확인 — 조 편성

수업 시작 직후 아래 기준으로 조를 편성합니다.

```
2인 1조 완성 → Track 1으로 진행
혼자인 경우  → Track 2로 진행 (교수님이 팀원 역할)
홀수 남을 경우 → 3인 1조 구성
```

---

## Track 1 — 2인 1조

### [조원 A] Step 1. repo 생성

본인 GitHub 계정에서 새 repo를 아래 설정으로 만드세요.

```
Repository name: gradebook-week3
Visibility: Public
Add a README file: 체크 안 함
.gitignore: Python 선택
License: 없음
```

---

### [조원 A] Step 2. 2주차 파일 복사 후 초기 commit

Sourcetree에서 gradebook-week3 repo를 로컬에 클론하세요.

```
Sourcetree + 버튼 → Clone
→ URL: github.com/본인ID/gradebook-week3
→ 로컬 경로: Documents/gradebook-week3
```

클론 후 아래 파일을 gradebook-week3 폴더에 복사하세요.

**2주차 실습을 완료한 경우 — git-practice 폴더에서 복사**
```
복사할 파일:
  grades.py
  converter.py
  file_io.py
  README.md
```

**2주차 실습을 완료하지 못한 경우 — starter 파일 사용**
```
① 먼저 공통 Step 8 (class remote + pull)을 진행하세요.
② pull 후 생성된 starter/ 폴더의 파일 4개를
   gradebook-week3 폴더 루트에 복사하세요.
   grades.py / converter.py / file_io.py / README.md
```

Sourcetree에서 파일들을 staging하고 commit하세요.

```
commit 메시지: "Import week2 gradebook files"
push
```

---

### [조원 A] Step 3. 조원 B를 Collaborator로 초대

GitHub 웹에서 gradebook-week3 repo를 열고 아래 순서로 진행하세요.

```
① 상단 탭에서 Settings 클릭
② 좌측 메뉴에서 Collaborators 클릭
   (또는 Access → Collaborators)
③ "Add people" 버튼 클릭
④ 조원 B의 GitHub 계정명 또는 이메일 입력
⑤ 검색 결과에서 조원 B 선택
⑥ "Add [조원B 계정명] to this repository" 클릭
```

조원 B에게 GitHub 가입 이메일로 초대장이 발송됩니다.

---

### [조원 A] Step 4. 교수님을 Collaborator로 초대

Step 3과 동일한 방법으로 교수님 GitHub 계정을 초대하세요.

```
교수님 GitHub 계정명: [교수님이 수업 중 안내]
```

> 💡 교수님이 Collaborator로 등록되어야  
> PR 리뷰어로 지정하고 approve를 받을 수 있습니다.

---

### [조원 B] Step 5. Collaborator 초대 수락

GitHub 가입 이메일을 확인하고 초대를 수락하세요.

```
방법 A — 이메일:
  수신된 초대 이메일 → "Accept invitation" 버튼 클릭

방법 B — GitHub 웹:
  github.com 접속 → 우측 상단 알림(🔔) 클릭
  → 초대 알림 확인 → "Accept invitation" 클릭
```

수락 후 gradebook-week3 repo가 본인 GitHub에서 보이는지 확인하세요.

---

### [조원 B] Step 6. repo 클론

Sourcetree에서 gradebook-week3 repo를 로컬에 클론하세요.

```
Sourcetree + 버튼 → Clone
→ URL: github.com/조원A계정/gradebook-week3
→ 로컬 경로: Documents/gradebook-week3
```

클론 후 grades.py, converter.py, README.md가 있는지 확인하세요.

---

### [조원 A] Step 7. Branch Ruleset 설정

main 브랜치를 보호해서 PR 없이는 merge가 불가능하도록 설정합니다.

```
① GitHub에서 gradebook-week3 → Settings
② 좌측 메뉴 Rules → Rulesets 클릭
③ "New ruleset" → "New branch ruleset" 클릭
④ Ruleset Name: protect-main 입력
⑤ Enforcement status: Active 선택
⑥ Target branches → "Add target" → "Include by pattern"
   → main 입력
⑦ Rules 섹션에서 아래 항목 체크:
   ☑ Require a pull request before merging
      → Required approvals: 1
⑧ "Create" 버튼 클릭
```

아래 두 항목은 기본값으로 이미 체크되어 있습니다. 그대로 두세요.

```
☑ Restrict deletions
   → main 브랜치를 실수로 삭제하는 것을 방지

☑ Block force pushes
   → git push --force로 히스토리를 강제 덮어쓰는 것을 방지
```

> 💡 이 두 설정이 "main은 항상 안정적인 상태"를  
> 유지하는 핵심 보호 장치입니다.

설정 완료 후 main 브랜치에 직접 push하면 거부됩니다.

---

### 양쪽 확인 — Track 1 환경 설정 완료 기준

```
조원 A:
  ☑ gradebook-week3 repo 생성 완료
  ☑ 초기 파일 commit + push 완료
  ☑ 조원 B + 교수님 collaborator 초대 완료
  ☑ Branch Ruleset 설정 완료

조원 B:
  ☑ 초대 수락 완료
  ☑ 로컬 클론 완료
  ☑ 파일 3개 확인 완료
```

---

## Track 2 — 1인

### Step 1. repo 생성

Track 1의 [조원 A] Step 1과 동일하게 진행하세요.

```
Repository name: gradebook-week3
Visibility: Public
.gitignore: Python 선택
```

### Step 2. 2주차 파일 복사 후 초기 commit

Track 1의 [조원 A] Step 2와 동일하게 진행하세요.

### Step 3. 교수님을 Collaborator로 초대

Track 1의 [조원 A] Step 4와 동일하게 진행하세요.

> 💡 Track 2는 교수님이 팀원 역할을 겸합니다.  
> PR 리뷰와 approve를 교수님에게 요청하게 됩니다.

### Step 4. Branch Ruleset 설정

Track 1의 [조원 A] Step 7과 동일하게 진행하세요.

---

## 공통 — Step 8. class remote 추가 및 실습 지시서 수령

모든 조가 아래 과정을 진행하세요.

Terminal(Mac) 또는 PowerShell(Windows)에서
gradebook-week3 폴더로 이동하세요.

```bash
cd Documents/gradebook-week3
```

class remote를 추가하고 실습 지시서를 받아오세요.

```bash
git remote add class https://github.com/[org이름]/Git-GitHub-Lab-instructions.git
git remote -v
git pull class main
```

`INSTRUCTIONS_lecture3.md`가 로컬에 생겼는지 확인하세요.

```bash
# Mac
ls

# Windows
dir
```

VS Code에서 파일을 열어 내용을 확인하세요.

---

## 환경 설정 완료 최종 확인

```
Track 1:
  ☑ gradebook-week3 repo에 두 계정 모두 접근 가능
  ☑ 교수님 collaborator 등록 완료
  ☑ Branch Ruleset 설정 완료
  ☑ INSTRUCTIONS_lecture3.md 수령 완료

Track 2:
  ☑ gradebook-week3 repo 생성 완료
  ☑ 교수님 collaborator 등록 완료
  ☑ Branch Ruleset 설정 완료
  ☑ INSTRUCTIONS_lecture3.md 수령 완료
```

이제 실습 지시서(INSTRUCTIONS_lecture3.md)를 따라 진행합니다.

---

## Track 3 — 3인 1조

### [조원 A] Step 1. repo 생성

Track 1의 [조원 A] Step 1과 동일하게 진행하세요.

```
Repository name: gradebook-week3
Visibility: Public
.gitignore: Python 선택
```

---

### [조원 A] Step 2. 2주차 파일 복사 후 초기 commit

Track 1의 [조원 A] Step 2와 동일하게 진행하세요.

```
복사할 파일: grades.py / converter.py / README.md
commit 메시지: "Import week2 gradebook files"
push
```

---

### [조원 A] Step 3. 조원 B, C 및 교수님 Collaborator 초대

GitHub 웹에서 gradebook-week3 → Settings → Collaborators → Add people

```
초대 대상:
  조원 B GitHub 계정
  조원 C GitHub 계정
  교수님 GitHub 계정
```

---

### [조원 B, C] Step 4. 초대 수락 및 repo 클론

이메일 또는 GitHub 알림에서 초대를 수락하세요.

```
방법 A — 이메일: "Accept invitation" 버튼 클릭
방법 B — GitHub 웹: 알림(🔔) → "Accept invitation" 클릭
```

Sourcetree에서 repo를 로컬에 클론하세요.

```
Sourcetree + 버튼 → Clone
→ URL: github.com/조원A계정/gradebook-week3
→ 로컬 경로: Documents/gradebook-week3
```

---

### [조원 A] Step 5. Branch Ruleset 설정

Track 1의 [조원 A] Step 7과 동일하게 진행하세요.

```
Ruleset Name: protect-main
Target: main
☑ Require a pull request before merging
   → Required approvals: 1
☑ Restrict deletions (기본값 유지)
☑ Block force pushes (기본값 유지)
```

---

### PR 리뷰 역할 분배

3인 모두 개발자이면서 서로의 PR 리뷰어가 됩니다.

```
조원 A의 PR → 조원 B 또는 C가 리뷰 + 교수님 최종 approve
조원 B의 PR → 조원 A 또는 C가 리뷰 + 교수님 최종 approve
조원 C의 PR → 조원 A 또는 B가 리뷰 + 교수님 최종 approve
```

---

### 확인 — Track 3 환경 설정 완료 기준

```
조원 A:
  ☑ gradebook-week3 repo 생성 완료
  ☑ 초기 파일 commit + push 완료
  ☑ 조원 B, C + 교수님 collaborator 초대 완료
  ☑ Branch Ruleset 설정 완료

조원 B, C:
  ☑ 초대 수락 완료
  ☑ 로컬 클론 완료
  ☑ 파일 3개 확인 완료
```

공통 Step 8 (class remote + 실습 지시서 수령)을 이어서 진행하세요.
