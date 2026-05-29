# 1주차 실습 지시서 — Git & GitHub 기초

> **프로젝트**: `gradebook` — 과목별 성적 관리 프로그램  
> **실습 환경**: GitHub Desktop · Sourcetree · VS Code  
> **목표**: Unstaged → Staged → Committed 흐름 체험, Push/Pull 실습, Hunk Staging

---

## 사전 확인

실습 시작 전 아래 항목을 확인하세요.

- [ ] `git-practice` 원격 repo가 본인 GitHub 계정에 생성되어 있다
- [ ] Sourcetree에서 `git-practice`가 로컬에 클론되어 있다
- [ ] Sourcetree에 GitHub 계정이 연결되어 있다
- [ ] VS Code가 설치되어 있다

---

## Step 1 — README.md 생성 및 첫 Commit

**목표**: Unstaged → Staged → Committed 3단계 첫 체험

### 1-1. README.md 파일 생성

VS Code에서 `git-practice` 폴더를 열고 `README.md` 파일을 새로 만드세요.  
아래 내용을 그대로 입력합니다.

```markdown
# gradebook

과목별 점수를 입력받아 성적을 관리하는 프로그램입니다.

## 기능
(추후 업데이트 예정)

## 사용법
python grades.py
```

파일을 저장합니다.

### 1-2. Sourcetree에서 확인

Sourcetree를 열고 `git-practice` repo를 확인하세요.

- 그래프 상단에 **"커밋하지 않은 변경사항"** 노드가 생겼나요?
- 하단 파일 패널 **Unstaged** 칸에 `README.md`가 보이나요?

> 💡 Git이 새 파일을 감지했다는 신호입니다. 아직 Staging Area에는 올라가지 않은 상태입니다.

### 1-3. Staging — Staged 칸으로 이동

Unstaged 칸의 `README.md` 옆 체크박스를 클릭하거나,  
파일을 선택하고 **Stage** 버튼을 클릭하세요.

- `README.md`가 **Staged** 칸으로 이동했나요?

> 💡 이 동작이 `git add`입니다. Staging Area에 올렸다는 의미입니다.

### 1-4. Commit

Sourcetree 하단 Commit 메시지 입력창에 아래와 같이 입력하고 **Commit** 버튼을 클릭하세요.

```
Add README.md
```

### 1-5. Sourcetree 그래프 확인

- **"커밋하지 않은 변경사항"** 노드가 사라졌나요?
- 그래프에 새 노드 `Add README.md`가 생겼나요?
- `main` 라벨이 이 노드에 붙어 있고, `origin/main`보다 **앞서 있는** 것을 확인하세요.

> 💡 commit은 로컬에만 기록된 상태입니다. 아직 GitHub에는 올라가지 않았습니다.

---

## Step 2 — grades.py 생성 (점수 입력 함수)

**목표**: .py + README.md 동시 수정을 같은 Commit으로 묶기

### 2-1. grades.py 작성

VS Code에서 `grades.py` 파일을 새로 만들고 아래 코드를 입력하세요.

```python
# grades.py

SUBJECTS = ["국어", "영어", "수학", "과탐"]


def get_scores():
    """4개 과목의 점수를 입력받아 딕셔너리로 반환한다."""
    scores = {}
    print("과목별 점수를 입력하세요 (0~100).\n")

    for subject in SUBJECTS:
        while True:
            try:
                score = float(input(f"{subject} 점수: "))
                if 0 <= score <= 100:
                    scores[subject] = score
                    break
                else:
                    print("  → 0에서 100 사이의 점수를 입력하세요.")
            except ValueError:
                print("  → 숫자를 입력하세요.")

    return scores


if __name__ == "__main__":
    scores = get_scores()
    print("\n입력된 점수:", scores)
```

### 2-2. README.md 업데이트

`README.md`의 `## 기능` 섹션을 아래와 같이 수정하세요.

```markdown
## 기능
- 과목별 점수 입력
```

### 2-3. Sourcetree에서 확인

- Unstaged 칸에 `grades.py`와 `README.md` **두 파일이 동시에** 보이나요?

### 2-4. 두 파일 모두 Staging 후 Commit

두 파일을 모두 Staged 칸으로 이동한 후 아래 메시지로 commit하세요.

```
Add grade input function + update README
```

> 💡 논리적으로 관련된 변경은 하나의 commit으로 묶습니다.  
> "기능을 추가했으면 README도 같이 업데이트한다"는 습관을 만드세요.

---

## Step 3 — 평균 계산 함수 추가

**목표**: 파일 단위 Selective Staging 실습

### 3-1. grades.py에 함수 추가

`get_scores()` 함수 아래에 다음 함수를 추가하세요.

```python
def calculate_average(scores):
    """점수 딕셔너리를 받아 평균을 반환한다."""
    if not scores:
        return 0.0
    return sum(scores.values()) / len(scores)
```

`if __name__` 블록도 아래와 같이 수정하세요.

```python
if __name__ == "__main__":
    scores = get_scores()
    print("\n입력된 점수:", scores)

    average = calculate_average(scores)
    print(f"평균 점수: {average:.1f}점")
```

### 3-2. README.md 업데이트

```markdown
## 기능
- 과목별 점수 입력
- 평균 점수 계산
```

### 3-3. 파일 단위 Selective Staging

Sourcetree Unstaged 칸에 `grades.py`와 `README.md`가 모두 보이는 상태에서,  
**`grades.py`만 먼저** Staged 칸으로 이동하세요. `README.md`는 Unstaged에 남겨둡니다.

```
commit: "Add average calculation"
```

이어서 `README.md`를 Staging하고 별도로 commit하세요.

```
commit: "Update README: add average feature"
```

> 💡 수정된 파일이 여러 개여도 관련 있는 것만 골라서 따로 commit할 수 있습니다.  
> 이것이 파일 단위 Selective Staging입니다.

---

## Step 4 — 결과 출력 + 최고/최저 함수 추가

**목표**: Hunk 단위 Selective Staging — 같은 파일 안에서 다른 Commit으로 나누기

### 4-1. grades.py에 두 함수를 연달아 추가

`calculate_average()` 아래에 두 함수를 **한 번에** 작성하세요.

```python
def print_result(scores, average):
    """점수와 평균을 보기 좋게 출력한다."""
    print("\n" + "=" * 30)
    print("       성적 결과")
    print("=" * 30)
    for subject in SUBJECTS:
        print(f"  {subject:<10} {scores[subject]:>6.1f}점")
    print("-" * 30)
    print(f"  {'평균':<10} {average:>6.1f}점")
    print("=" * 30)


def find_highest_lowest(scores):
    """최고점과 최저점 과목을 반환한다."""
    highest = max(SUBJECTS, key=lambda s: scores[s])
    lowest  = min(SUBJECTS, key=lambda s: scores[s])
    return highest, lowest
```

`if __name__` 블록을 최종본으로 업데이트하세요.

```python
if __name__ == "__main__":
    scores = get_scores()
    average = calculate_average(scores)
    print_result(scores, average)

    highest, lowest = find_highest_lowest(scores)
    print(f"\n최고점: {highest} ({scores[highest]:.1f}점)")
    print(f"최저점: {lowest} ({scores[lowest]:.1f}점)")
```

### 4-2. Sourcetree에서 grades.py 클릭

Unstaged 칸에서 `grades.py`를 클릭하면 오른쪽 diff 패널에  
두 함수가 모두 초록색으로 표시됩니다.

### 4-3. Hunk 단위로 나눠서 Staging

`print_result()` 함수 부분에서 **우클릭 → Stage Hunk**를 선택하세요.  
`find_highest_lowest()` 부분은 Unstaged에 남겨둡니다.

```
commit: "Add result print function"
```

이어서 나머지 `find_highest_lowest()` hunk를 Staging하고 commit하세요.

```
commit: "Add highest/lowest score finder"
```

### 4-4. README.md 최종 업데이트 후 commit

```markdown
## 기능
- 과목별 점수 입력
- 평균 점수 계산
- 성적 결과 출력
- 최고점 / 최저점 과목 확인

## 실행 예시
과목명: 국어 → 88점
과목명: 영어 → 92점
...
==============================
       성적 결과
==============================
  국어           88.0점
  영어           92.0점
  수학           75.0점
  과탐           81.0점
------------------------------
  평균           84.0점
==============================

최고점: 영어 (92.0점)
최저점: 수학 (75.0점)
```

```
commit: "Update README: add output and stats features"
```

### 4-5. 전체 그래프 확인

Sourcetree 그래프에서 각 노드를 클릭하며 diff를 확인하세요.

> 💡 각 노드가 그 시점의 스냅샷입니다.  
> 어느 노드를 클릭해도 그때 무엇이 바뀌었는지 볼 수 있습니다.

---

## Step 5 — Push + 로컬/원격 HEAD 비교

**목표**: commit ≠ push 실감, HEAD 위치 변화 시각 확인

### 5-1. Push 전 그래프 확인

Sourcetree 그래프를 보면 이런 상태입니다.

```
● Update README: add output and stats features  ← main (로컬)
● Add highest/lowest score finder
● Add result print function
● Update README: add average feature
● Add average calculation
● Add grade input function + update README       ← origin/main (원격, 뒤처짐)
● Add README.md
```

> 💡 `main`(로컬)이 `origin/main`(원격)보다 앞서 있습니다.  
> commit은 됐지만 GitHub는 아직 모르는 상태입니다.

### 5-2. Push 실행

Sourcetree 상단 **Push** 버튼을 클릭하세요.

Push 완료 후 그래프를 확인하세요.

- `main`과 `origin/main`이 **같은 노드**에 위치하나요?

### 5-3. GitHub 웹에서 확인

브라우저에서 본인 GitHub repo를 열고 확인하세요.

- commit history가 Sourcetree 그래프와 동일한가요?
- 각 commit을 클릭하면 diff가 보이나요?
- `README.md`가 GitHub 웹에서 렌더링되어 보이나요?

---

## Step 6 — 두 번째 로컬 클론 + Pull 실습

**목표**: 로컬과 원격의 독립성 체험, Pull의 의미 확인

### 6-1. 같은 원격 repo를 다른 폴더에 클론

Sourcetree 상단 **+** 버튼 → Clone → 동일한 GitHub repo URL 입력  
단, 로컬 저장 경로를 다르게 지정하세요.

```
첫 번째: Documents/git-practice
두 번째: Documents/git-practice-2   ← 경로만 다르게
```

### 6-2. Sourcetree에서 두 repo를 탭으로 열기

두 repo를 Sourcetree에서 나란히 탭으로 열어두세요.

### 6-3. 첫 번째 로컬에서 추가 작업

`git-practice/grades.py`에 주석 한 줄을 추가하세요.

```python
# gradebook v1.0 — 1주차 실습 완료
SUBJECTS = ["국어", "영어", "수학", "과학탐구"]
```

```
commit: "Add version comment"
push
```

### 6-4. 두 repo 그래프 비교

| repo | 상태 |
|---|---|
| git-practice | main, origin/main 일치 (최신) |
| git-practice-2 | main이 origin/main보다 **뒤처진** 상태 |

> 💡 같은 원격 repo를 바라보지만 로컬 상태가 다를 수 있습니다.  
> 이것이 "로컬과 원격은 독립적으로 존재한다"는 의미입니다.

### 6-5. git-practice-2에서 Pull 실행

`git-practice-2` 탭에서 Sourcetree **Pull** 버튼을 클릭하세요.

- 그래프에서 `main`이 `origin/main`과 다시 일치했나요?
- `grades.py`에 추가한 주석이 `git-practice-2`에도 반영됐나요?

---

## Step 7 — GitHub 웹에서 직접 수정 후 Pull

**목표**: 원격이 로컬보다 앞선 상황 체험

### 7-1. GitHub 웹에서 README.md 수정

GitHub repo 페이지에서 `README.md` 파일을 클릭하고  
우측 상단 연필(편집) 아이콘을 클릭하세요.

아래 섹션을 추가하고 **Commit changes**를 클릭하세요.

```markdown
## 개발 환경
- Python 3.x
- Git / GitHub
```

commit 메시지:
```
Update README from GitHub web
```

### 7-2. 로컬 Sourcetree 그래프 확인

`git-practice` 탭에서 Sourcetree 그래프를 확인하세요.

- `origin/main`이 `main`보다 **앞서 있는** 상태가 보이나요?

> 💡 원격에서 변경이 일어났지만 로컬은 아직 모르는 상태입니다.

### 7-3. Pull 실행

Sourcetree **Pull** 버튼을 클릭하세요.

- `main`과 `origin/main`이 다시 일치했나요?
- `README.md`에 방금 추가한 내용이 로컬에도 반영됐나요?

---

## Step 8 — Amend (시간 여유 있을 때)

**목표**: 실수해도 고칠 수 있다는 경험

### 8-1. 의도적으로 오타가 있는 commit 만들기

`README.md`에 아무 줄이나 추가하고 아래 메시지로 commit하세요.

```
Udate README    ← 오타
```

### 8-2. Sourcetree에서 메시지 수정

그래프에서 방금 commit을 **우클릭 → Amend latest commit**을 선택하세요.  
메시지를 올바르게 수정하세요.

```
Update README
```

> ⚠️ Amend는 **Push 전에만** 안전합니다.  
> 이미 Push한 commit을 Amend하면 원격과 충돌이 생길 수 있습니다.

---

## 1주차 실습 완료 — 최종 그래프 확인

Sourcetree 그래프가 아래와 같이 보이면 완료입니다.

```
● Update README from GitHub web
● Add version comment
● Update README: add output and stats features
● Add highest/lowest score finder
● Add result print function
● Update README: add average feature
● Add average calculation
● Add grade input function + update README
● Add README.md
```

각 노드를 클릭하며 diff를 확인하고,  
GitHub 웹의 commit history와 동일한지 비교해보세요.

---

## 핵심 개념 정리

| 동작 | Sourcetree | 의미 |
|---|---|---|
| 파일 수정 | Unstaged 칸에 표시 | Working Directory 변경 |
| Stage | Staged 칸으로 이동 | `git add` |
| Commit | 그래프에 노드 추가 | 로컬 저장소에 스냅샷 기록 |
| Push | main = origin/main 일치 | 원격에 업로드 |
| Pull | 뒤처진 로컬을 원격과 동기화 | 원격 변경사항 내려받기 |
| Hunk Staging | diff 패널에서 우클릭 | 같은 파일 안에서도 나눠서 commit |
