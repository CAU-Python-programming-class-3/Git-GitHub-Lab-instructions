# 2주차 실습 지시서 — Branch & CLI 기초

> **실습 환경**: git-practice repo (팀원 A) + git-practice-2 repo (팀원 B 역할)  
> **사용 툴**: Sourcetree + Terminal(Mac) / PowerShell(Windows)  
> **목표**: 두 명이 각자 브랜치에서 작업하고 하나의 원격 repo로 통합하는 협업 흐름 체험

---

## 역할 설정

오늘 실습에서 두 로컬 repo는 각각 다른 팀원의 컴퓨터 역할을 합니다.

| 로컬 repo | 역할 | 주요 툴 | 담당 작업 |
|---|---|---|---|
| git-practice | 팀원 A (나) | **CLI 중심** | feature/grade-converter 브랜치 |
| git-practice-2 | 팀원 B (가상) | **Sourcetree 중심** | feature/file-io 브랜치 |

> 💡 같은 Git 동작을 CLI와 Sourcetree 두 가지 방법으로 모두 경험합니다.

---

## 파트 1 — CLI 입문: Sourcetree와 1:1 매핑

**목표**: 지난 주에 Sourcetree로 한 동작을 CLI로 확인하기

### Step 1-1. Terminal에서 git-practice 폴더로 이동

Mac:
```bash
cd Documents/git-practice
```

Windows:
```powershell
cd Documents\git-practice
```

### Step 1-2. 기본 CLI 명령어 실행

아래 명령어를 순서대로 입력하고 Sourcetree 화면과 비교하세요.

```bash
# 현재 상태 확인 → Sourcetree Unstaged 칸과 동일
git status

# 커밋 이력 확인 → Sourcetree 그래프와 동일
git log --oneline --graph
```

> 💡 `git log`에서 나가려면 `q`를 누르세요.

### Step 1-3. CLI로 파일 수정 → staging → commit → push

grades.py 맨 위에 아래 주석 한 줄을 추가하세요.

```python
# gradebook v1.1 — 2주차 실습 시작
SUBJECTS = ["국어", "영어", "수학", "과학탐구"]
```

저장 후 터미널에서 순서대로 실행하세요.

```bash
git status
git add grades.py
git status
git commit -m "Add version comment for week2"
git log --oneline --graph
git push
```

Sourcetree 그래프에서 새 노드가 생기고  
`main`과 `origin/main`이 일치하는지 확인하세요.

> 💡 Sourcetree와 터미널이 같은 결과를 보여줍니다.  
> 툴이 달라도 Git은 하나입니다.

---

## 파트 2 — 협업 브랜치 실습

**목표**: 두 팀원이 각자 브랜치에서 독립적으로 작업하고 하나의 원격 repo로 통합하기

> 📌 **핵심 원칙**: main은 항상 동작하는 안정적인 상태를 유지합니다.  
> 새 기능은 반드시 feature 브랜치에서 개발합니다.

---

### [팀원 A · CLI] Step 2-1. 현재 브랜치 확인

```bash
git branch
```

`* main` — 현재 main 브랜치에 있다는 표시입니다.

---

### [팀원 A · CLI] Step 2-2. feature/grade-converter 브랜치 생성

```bash
git switch -c feature/grade-converter
git branch
```

`* feature/grade-converter`로 바뀐 것을 확인하세요.

Sourcetree 그래프 확인:
- `feature/grade-converter`와 `main`이 **같은 노드**에 위치
- HEAD가 `feature/grade-converter`로 이동

> 💡 이 시점에서 두 브랜치는 같은 commit을 가리킵니다.  
> 아직 분기가 일어나지 않은 상태입니다.

---

### [팀원 A · CLI] Step 2-3. converter.py 작성 후 commit → push

VS Code에서 `converter.py` 파일을 새로 만들고 아래 코드를 작성하세요.

```python
# converter.py
# 점수를 학점으로 변환하는 모듈

GRADE_SCALE = [
    (95, "A+"), (90, "A"),  (85, "B+"),
    (80, "B"),  (75, "C+"), (70, "C"),
    (65, "D+"), (60, "D"),  (0,  "F")
]


def convert_to_grade(score):
    """점수를 학점 문자열로 변환한다."""
    for threshold, grade in GRADE_SCALE:
        if score >= threshold:
            return grade
    return "F"


def print_grades(scores):
    """각 과목의 학점을 출력한다."""
    print("\n" + "=" * 30)
    print("       학점 결과")
    print("=" * 30)
    for subject, score in scores.items():
        grade = convert_to_grade(score)
        print(f"  {subject:<10} {score:>5.1f}점  {grade}")
    print("=" * 30)
```

README.md `## 기능` 섹션에 아래 항목을 추가하세요.

```markdown
- 점수 → 학점 변환 (A+~F)
```

CLI로 staging, commit, push합니다.

```bash
git add converter.py README.md
git commit -m "Add grade converter + update README"
git push -u origin feature/grade-converter
```

> 💡 `-u origin feature/grade-converter`는 처음 push할 때  
> 원격 브랜치와 연결하는 옵션입니다. 이후에는 `git push`만으로 됩니다.

Sourcetree 그래프 확인:
- `feature/grade-converter`가 `main`보다 **앞서** 있는 상태
- `origin/feature/grade-converter`도 같은 노드에 표시됨
- GitHub 웹에서 브랜치가 생긴 것 확인

---

### [팀원 B · Sourcetree] Step 2-4. git-practice-2에서 pull → feature/file-io 브랜치 생성

Sourcetree에서 **git-practice-2 탭**으로 전환하세요.

**Sourcetree Pull 버튼**을 클릭해서 팀원 A가 push한 내용을 받아오세요.

Sourcetree 그래프 확인:
- `origin/feature/grade-converter`가 보이는지 확인

이제 Sourcetree에서 feature/file-io 브랜치를 생성합니다.

```
Sourcetree 상단 Branch 버튼 클릭
→ New Branch 이름 입력: feature/file-io
→ Checkout New Branch 체크 확인
→ Create Branch 클릭
```

Sourcetree 그래프 확인:
- `feature/file-io`(팀원 B)와 `origin/feature/grade-converter`(팀원 A)가
  **같은 분기점에서 갈라진** 모습
- HEAD가 `feature/file-io`로 이동한 것 확인

> 💡 팀원 B는 **main 기준으로** 브랜치를 생성합니다.  
> 팀원 A의 feature/grade-converter와 **같은 분기점**에서 갈라집니다.

---

### [팀원 B · Sourcetree] Step 2-5. file_io.py 작성 후 commit → push

VS Code에서 git-practice-2 폴더의 `file_io.py` 파일을 새로 만들고 아래 코드를 작성하세요.

```python
# file_io.py
# 점수를 파일로 저장하고 불러오는 모듈

import os

DATA_FILE = "data/scores.txt"


def save_scores(scores):
    """점수 딕셔너리를 파일에 저장한다."""
    os.makedirs("data", exist_ok=True)
    with open(DATA_FILE, "w", encoding="utf-8") as f:
        for subject, score in scores.items():
            f.write(f"{subject},{score}\n")
    print(f"점수가 {DATA_FILE}에 저장되었습니다.")


def load_scores():
    """파일에서 점수를 불러와 딕셔너리로 반환한다."""
    if not os.path.exists(DATA_FILE):
        print("저장된 점수 파일이 없습니다.")
        return {}
    scores = {}
    with open(DATA_FILE, "r", encoding="utf-8") as f:
        for line in f:
            subject, score = line.strip().split(",")
            scores[subject] = float(score)
    return scores
```

`.gitignore`에 data 폴더를 추가하세요.

```
data/
```

README.md `## 기능` 섹션에 아래 항목을 추가하세요.

```markdown
- 점수 파일 저장 / 불러오기
```

**Sourcetree에서** staging하고 commit합니다.

```
Unstaged 칸에서 file_io.py, README.md, .gitignore 모두 Staged로 이동
commit 메시지: "Add file save/load + update README"
Commit 버튼 클릭
```

**Sourcetree Push 버튼**으로 원격에 push합니다.

```
Push 버튼 클릭
→ feature/file-io 브랜치 선택 확인
→ Push 실행
```

Sourcetree 그래프 확인:
- `feature/grade-converter`(팀원 A)와 `feature/file-io`(팀원 B)가
  **같은 분기점에서 각각 다른 노드에 위치**
- GitHub 웹에서 두 브랜치가 모두 올라온 것 확인

---

### [팀원 A · Sourcetree] Step 2-6. 두 브랜치를 main에 순차 merge

git-practice 탭으로 전환하고 CLI로 최신 상태를 확인합니다.

```bash
git switch main
git pull
git branch -a
```

`origin/feature/grade-converter`와 `origin/feature/file-io`가 모두 보이는지 확인하세요.

**이제 Sourcetree에서 merge합니다.**

**feature/grade-converter를 먼저 merge:**

```
Sourcetree 그래프에서 feature/grade-converter 노드 우클릭
→ "Merge feature/grade-converter into current branch" 클릭
→ OK
```

Sourcetree 그래프 확인:
- **Fast-forward merge** 발생
- main 포인터가 `feature/grade-converter` 노드로 이동
- merge commit 없이 일직선으로 이동

> 💡 **Fast-forward merge**: feature 브랜치가 분기된 이후  
> main에 새 commit이 없었으므로 포인터만 앞으로 이동합니다.

브랜치 삭제:
```
Sourcetree 좌측 브랜치 목록에서 feature/grade-converter 우클릭
→ Delete feature/grade-converter 클릭
```

Push:
```
Sourcetree Push 버튼 클릭
```

**feature/file-io를 main에 merge:**

```
Sourcetree 그래프에서 origin/feature/file-io 노드 우클릭
→ "Merge origin/feature/file-io into current branch" 클릭
→ OK
```

Sourcetree 그래프 확인:
- 이번에는 **merge commit이 생깁니다**
- 두 선이 합쳐지는 모습이 그래프에 나타납니다

> 💡 **Merge commit**: `feature/file-io`가 분기된 이후  
> main에 `feature/grade-converter` merge로 새 commit이 생겼기 때문에  
> 단순 포인터 이동이 아닌 merge commit이 만들어집니다.

브랜치 삭제 후 Push:
```
feature/file-io 우클릭 → Delete
Sourcetree Push 버튼 클릭
```

GitHub 웹에서 commit history를 확인하세요.

---

### [팀원 B · CLI] Step 2-7. git-practice-2에서 최종 pull

git-practice-2 폴더 터미널에서 CLI로 pull합니다.

```bash
cd Documents/git-practice-2
git switch main
git pull
git log --oneline --graph
```

Sourcetree 그래프 확인:
- git-practice와 동일한 그래프가 보이는지 확인
- `converter.py`와 `file_io.py`가 로컬에 생겼는지 확인

> 💡 팀원 B의 로컬이 팀원 A가 merge한 결과를 받아서  
> 두 사람의 작업이 하나로 통합된 상태가 됩니다.

---

## 파트 3 — 최종 그래프 확인

**git-practice Sourcetree 그래프가 아래와 유사한지 확인하세요.**

```
●   Merge branch 'feature/file-io'      ← main, origin/main
|\
| ● Add file save/load + update README  ← feature/file-io (팀원 B)
|/
● Add grade converter + update README   ← feature/grade-converter (팀원 A)
● Add version comment for week2
● Update README from GitHub web         ← 1주차
● Add highest/lowest score finder
...
```

CLI로도 동일하게 확인하세요.

```bash
git log --oneline --graph
```

---

## 핵심 개념 정리

| 개념 | CLI | Sourcetree |
|---|---|---|
| 브랜치 생성 + 전환 | `git switch -c 브랜치명` | Branch 버튼 → New Branch |
| 브랜치 목록 확인 | `git branch -a` | 좌측 브랜치 패널 |
| Staging | `git add 파일명` | Unstaged → Staged 이동 |
| Commit | `git commit -m "메시지"` | Commit 버튼 |
| Push (첫 번째) | `git push -u origin 브랜치명` | Push 버튼 → 브랜치 선택 |
| Merge | `git merge 브랜치명` | 노드 우클릭 → Merge |
| 브랜치 삭제 | `git branch -d 브랜치명` | 브랜치 우클릭 → Delete |
| Pull | `git pull` | Pull 버튼 |
| Fast-forward merge | 분기 후 main에 새 commit 없음 → 포인터만 이동 | 일직선 그래프 |
| Merge commit | 분기 후 양쪽에 새 commit 있음 → 새 commit 생성 | 두 선이 합쳐지는 그래프 |
