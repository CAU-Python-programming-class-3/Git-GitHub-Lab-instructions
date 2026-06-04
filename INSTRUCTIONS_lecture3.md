# 3주차 실습 지시서 — PR & Merge Conflict

> **실습 환경**: gradebook-week3 repo (2인 1조 또는 1인)  
> **사용 툴**: GitHub 웹 + VS Code + CLI + Sourcetree  
> **목표**: PR 생성, conflict 해결, approve → merge 전체 흐름 체험

---

## 역할 설정

| 역할 | Track 1 | Track 2 |
|---|---|---|
| 조원 A | 실제 팀원 A | 본인 |
| 조원 B | 실제 팀원 B | 교수님 |
| 리뷰어 | 교수님 | 교수님 |

---

## 파트 1 — 브랜치 생성 및 작업

> 📌 gradebook-week3 repo에서 진행합니다.

### [조원 A] Step 1-1. feature/stats 브랜치 생성

```bash
cd Documents/gradebook-week3
git switch -c feature/stats
```

### [조원 A] Step 1-2. stats.py 작성 후 commit

VS Code에서 `stats.py` 파일을 새로 만들고 아래 코드를 작성하세요.

```python
# stats.py
# 점수 통계를 계산하는 모듈

import math


def calculate_variance(scores):
    """분산을 계산한다."""
    avg = sum(scores.values()) / len(scores)
    return sum((s - avg) ** 2 for s in scores.values()) / len(scores)


def calculate_stddev(scores):
    """표준편차를 계산한다."""
    return math.sqrt(calculate_variance(scores))


def print_stats(scores):
    """통계 정보를 출력한다."""
    print("\n" + "=" * 30)
    print("       통계 정보")
    print("=" * 30)
    print(f"  분산:     {calculate_variance(scores):>8.2f}")
    print(f"  표준편차: {calculate_stddev(scores):>8.2f}")
    print("=" * 30)
```

README.md `## 기능` 섹션에 아래 항목을 추가하세요.

```markdown
- 점수 통계 (분산, 표준편차)
```

staging하고 commit하세요.

```bash
git add stats.py README.md
git commit -m "Add stats module + update README"
git push -u origin feature/stats
```

---

### [조원 B] Step 1-3. feature/report 브랜치 생성

Sourcetree에서 gradebook-week3 탭을 열고 Pull을 실행하세요.

```
Sourcetree Pull 버튼 클릭
→ main 기준으로 최신 상태 확인
```

Sourcetree에서 feature/report 브랜치를 생성하세요.

```
Branch 버튼 클릭
→ New Branch: feature/report
→ Checkout New Branch 체크
→ Create Branch 클릭
```

> ⚠️ 반드시 main 브랜치에 있는 상태에서 생성하세요.
> 조원 A의 feature/stats와 같은 분기점에서 갈라집니다.

---

### [조원 B] Step 1-4. report.py 작성 후 commit

VS Code에서 `report.py` 파일을 새로 만들고 아래 코드를 작성하세요.

```python
# report.py
# 성적 리포트를 파일로 저장하는 모듈


def save_report(scores, average):
    """성적 리포트를 마크다운 파일로 저장한다."""
    with open("report.md", "w", encoding="utf-8") as f:
        f.write("# 성적 리포트\n\n")
        f.write("## 과목별 점수\n\n")
        for subject, score in scores.items():
            f.write(f"- {subject}: {score:.1f}점\n")
        f.write(f"\n## 평균\n\n{average:.1f}점\n")
    print("report.md가 저장되었습니다.")
```

README.md `## 기능` 섹션에 아래 항목을 추가하세요.

```markdown
- 성적 리포트 저장 (report.md)
```

Sourcetree에서 staging하고 commit하세요.

```
report.py, README.md → Staged로 이동
commit 메시지: "Add report module + update README"
Sourcetree Push 버튼 클릭
```

> 💡 **의도적으로 pull하지 않고 push합니다.**  
> 조원 A가 push한 README.md 변경사항을 반영하지 않은 채로  
> push하기 때문에 이후 PR merge 시 conflict가 발생합니다.  
> 이것이 오늘 실습에서 conflict를 경험하는 핵심 시나리오입니다.

---

## 파트 2 — PR 생성

### [조원 A] Step 2-1. feature/stats → main PR 생성

GitHub 웹에서 gradebook-week3 repo를 열고 아래 순서로 진행하세요.

```
① "Compare & pull request" 버튼 클릭
   (push 직후 상단에 자동으로 표시됨)
   또는
   Pull requests 탭 → New pull request 클릭
   base: main ← compare: feature/stats

② PR 제목 입력:
   "Add statistics module"

③ PR 본문 작성:
   ## 변경 사항
   - stats.py 추가: 분산, 표준편차 계산 기능
   - README.md 업데이트

④ Reviewers 항목에서 교수님 계정 추가

⑤ "Create pull request" 클릭
```

---

### [조원 B] Step 2-2. feature/report → main PR 생성

Step 2-1과 동일한 방법으로 PR을 생성하세요.

```
PR 제목: "Add report module"

PR 본문:
  ## 변경 사항
  - report.py 추가: 성적 리포트 저장 기능
  - README.md 업데이트

Reviewers: 교수님 계정 추가
```

GitHub에서 PR 페이지를 열고 conflict 여부를 확인하세요.

```
조원 A의 PR (feature/stats):
  "This branch has no conflicts with the base branch"
  → conflict 없음, 바로 merge 가능

조원 B의 PR (feature/report):
  "This branch has conflicts that must be resolved"
  → conflict 발생 (의도된 상황입니다)
  → 파트 3으로 이동
```

> 💡 조원 A PR을 먼저 merge하면 main README에  
> "통계" 항목이 추가됩니다. 조원 B는 이 내용 없이  
> README를 수정했으므로 conflict가 발생합니다.  
> 이것이 오늘 실습의 핵심 시나리오입니다.

---

## 파트 3 — Conflict 해결

> 📌 조원 B의 PR에서 conflict가 발생합니다. 이것은 **의도된 상황**입니다.  
> pull 없이 같은 파일을 수정하면 어떤 일이 생기는지,  
> 그리고 PR이 어떻게 이를 감지하고 해결을 유도하는지 경험합니다.

### Step 3-1. conflict 발생 확인

PR 페이지에서 conflict가 발생한 파일을 확인하세요.

```
Conflicts:
  README.md
```

### Step 3-2. PR Conversation에서 합의

PR Conversation 탭에서 상대방에게 코멘트를 남기세요.

```
조원 B → 조원 A에게:
"README ## 기능 섹션에서
 stats 항목 아래에 report 항목 추가하려고 해.
 순서: 통계 → 리포트 저장 괜찮아?"

조원 A → 조원 B에게:
"OK, 그 순서로 해줘"
```

> 💡 실무에서 PR Conversation이 팀원 간 합의 채널로  
> 사용되는 방식입니다.

### Step 3-3. 로컬에서 conflict 해결

conflict가 있는 브랜치(feature/report)에서 진행하세요.

```bash
git switch feature/report
git fetch origin
git merge origin/main
```

VS Code에서 conflict 마커를 확인하고 해결하세요.

```
<<<<<<< HEAD
- 성적 리포트 저장 (report.md)     ← B
=======
- 점수 통계 (분산, 표준편차)        ← A (main에 merge된 내용)
>>>>>>> origin/main
```

합의한 대로 두 내용을 모두 살려서 정리하세요.

```markdown
## 기능
- 과목별 점수 입력
- 평균 점수 계산
- 성적 결과 출력
- 최고점 / 최저점 과목 확인
- 점수 통계 (분산, 표준편차)    ← A 유지
- 성적 리포트 저장 (report.md)  ← B 추가
```

### Step 3-4. 해결 후 push

```bash
git add README.md
git commit -m "Resolve conflict: merge main into feature/report"
git push
```

GitHub PR 페이지를 확인하세요.

```
"This branch has no conflicts with the base branch"
→ conflict 해결 완료 ✅
→ PR이 자동으로 업데이트됨
```

---

## 파트 4 — 코드 리뷰 및 Approve

### [교수님] Step 4-1. PR 리뷰

각 조의 PR을 열고 아래 순서로 리뷰합니다.

```
① Files changed 탭 클릭
② 변경된 파일 내용 확인
③ 코드에 코멘트 달기 (필요한 경우)
   라인 옆 + 버튼 클릭 → 코멘트 입력
④ Review changes 버튼 클릭
   → Approve 선택
   → "Submit review" 클릭
```

### [조원 A, B] Step 4-2. Approve 후 merge

교수님 Approve 후 PR 페이지에서 merge를 진행하세요.

**조원 A의 PR (feature/stats) 먼저 merge:**

```
"Merge pull request" 버튼 클릭
→ "Confirm merge" 클릭
→ "Delete branch" 클릭 (브랜치 정리)
```

**조원 B의 PR (feature/report) 이어서 merge:**

```
(conflict가 해결된 상태라면)
"Merge pull request" 버튼 클릭
→ "Confirm merge" 클릭
→ "Delete branch" 클릭
```

---

## 파트 5 — 최종 동기화 및 확인

### 양쪽 모두 Step 5-1. 로컬 main 업데이트

```bash
git switch main
git pull
git log --oneline --graph
```

Sourcetree 그래프 확인:
```
●   Merge pull request: feature/report     ← main
|\
| ● Resolve conflict + Add report module   ← feature/report
|/
●   Merge pull request: feature/stats
|\
| ● Add stats module                       ← feature/stats
|/
●   Import week2 gradebook files
```

---

## 핵심 개념 정리

| 개념 | 설명 |
|---|---|
| PR (Pull Request) | 브랜치를 main에 merge하기 전 리뷰를 요청하는 GitHub 기능 |
| PR Conversation | 팀원 간 합의 채널. conflict 해결 방향 협의에 사용 |
| Files changed | PR에서 변경된 코드를 라인 단위로 확인하는 탭 |
| Approve | 리뷰어가 코드를 확인하고 merge를 승인하는 행위 |
| Branch Ruleset | PR + Approve 없이는 main merge 불가 설정 |
| Conflict 해결 원칙 | feature 브랜치에서 먼저 해결 → main은 항상 clean |
| PR 자동 업데이트 | 같은 브랜치에 push하면 PR이 자동으로 갱신됨 |

---

## Track 3 — 3인 1조 실습

> 📌 Track 1과 동일한 흐름이지만 브랜치가 3개로 늘어납니다.  
> 3인 모두 개발자이면서 서로의 PR 리뷰어 역할을 겸합니다.

### 역할 분배

| 역할 | 담당 브랜치 | 작업 내용 |
|---|---|---|
| 조원 A | feature/stats | stats.py — 분산, 표준편차 |
| 조원 B | feature/report | report.py — 리포트 저장 |
| 조원 C | feature/summary | summary.py — 성적 요약 출력 |

---

### [조원 C] Step C-1. feature/summary 브랜치 생성

Sourcetree에서 Pull 후 main 기준으로 브랜치를 생성하세요.

```
Branch 버튼 → New Branch: feature/summary
→ Checkout New Branch 체크 → Create Branch
```

> ⚠️ 반드시 main에 있는 상태에서 생성하세요.
> 조원 A, B와 같은 분기점에서 갈라집니다.

---

### [조원 C] Step C-2. summary.py 작성 후 commit → push

VS Code에서 `summary.py` 파일을 새로 만들고 아래 코드를 작성하세요.

```python
# summary.py
# 전체 성적 요약을 출력하는 모듈


def print_summary(scores, average):
    """과목별 점수와 평균을 한눈에 요약 출력한다."""
    highest = max(scores, key=scores.get)
    lowest  = min(scores, key=scores.get)

    print("\n" + "=" * 30)
    print("       성적 요약")
    print("=" * 30)
    print(f"  수강 과목 수:  {len(scores)}개")
    print(f"  평균 점수:    {average:.1f}점")
    print(f"  최고 과목:    {highest} ({scores[highest]:.1f}점)")
    print(f"  최저 과목:    {lowest} ({scores[lowest]:.1f}점)")
    print("=" * 30)
```

README.md `## 기능` 섹션에 아래 항목을 추가하세요.

```markdown
- 성적 요약 출력
```

Sourcetree에서 staging하고 commit하세요.

```
commit 메시지: "Add summary module + update README"
Sourcetree Push 버튼 클릭
```

> ⚠️ README conflict 가능성이 있습니다.  
> push 에러 발생 시 아래 순서로 해결하세요.
>
> ```bash
> git merge main
> # VS Code에서 conflict 해결
> git add README.md
> git commit -m "Resolve conflict in README"
> git push -u origin feature/summary
> ```

---

### [조원 C] Step C-3. PR 생성

GitHub 웹에서 feature/summary → main으로 PR을 생성하세요.

```
PR 제목: "Add summary module"

PR 본문:
  ## 변경 사항
  - summary.py 추가: 성적 요약 출력 기능
  - README.md 업데이트

Reviewers:
  조원 A 또는 B 중 1명 추가
  교수님 계정 추가
```

---

### [전체] PR 리뷰 및 순차 merge

3개의 PR을 순차적으로 merge합니다.

```
① 조원 A의 PR (feature/stats) 먼저 merge
   → 리뷰어: 조원 B 또는 C + 교수님 approve

② 조원 B의 PR (feature/report)
   → conflict 발생 가능
   → Conversation에서 합의 후 해결
   → 리뷰어 approve → merge

③ 조원 C의 PR (feature/summary)
   → conflict 발생 가능
   → Conversation에서 합의 후 해결
   → 리뷰어 approve → merge
```

> 💡 merge 순서가 뒤로 갈수록 conflict 가능성이 높아집니다.  
> 각자 PR Conversation에서 충분히 합의한 후 진행하세요.

---

### 최종 그래프 확인

```
●   Merge PR: feature/summary           ← main
|\
| ● Add summary module                  ← feature/summary (조원 C)
|/
●   Merge PR: feature/report
|\
| ● Add report module                   ← feature/report (조원 B)
|/
●   Merge PR: feature/stats
|\
| ● Add stats module                    ← feature/stats (조원 A)
|/
●   Import week2 gradebook files
```

```bash
git switch main
git pull
git log --oneline --graph
```
