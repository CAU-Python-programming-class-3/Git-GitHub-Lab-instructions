# 2주차 수업 환경 설정

> **소요 시간**: 약 15~20분  
> **진행 방식**: 강의자와 함께 라이브로 진행  
> **목표**: Git CLI 사용 환경 구성 + 수업용 remote 설정

---

## Step 1 — Git 설치 확인

먼저 Git이 이미 설치되어 있는지 확인합니다.

**Mac — Terminal 실행**

Spotlight(⌘ + Space) → `Terminal` 입력 → 실행

```bash
git --version
```

**Windows — PowerShell 실행**

시작 메뉴 → `PowerShell` 검색 → 실행

```powershell
git --version
```

아래와 같은 메시지가 나오면 이미 설치된 상태입니다.

```
git version 2.xx.x
```

---

## Step 2 — Git 설치 (설치되지 않은 경우)

### Mac

Mac은 Git 명령어를 처음 실행하면 설치 여부를 물어보는 창이 뜨는 경우가 있습니다.

**방법 A — Xcode Command Line Tools (권장)**

```bash
xcode-select --install
```

설치 창이 뜨면 "설치" 버튼을 클릭하고 완료될 때까지 기다립니다.  
완료 후 다시 확인합니다.

```bash
git --version
```

**방법 B — Homebrew 사용**

Homebrew가 설치되어 있는 경우:

```bash
brew install git
```

### Windows

아래 링크에서 Git for Windows를 다운로드하여 설치합니다.

```
https://git-scm.com/download/win
```

설치 시 옵션은 모두 기본값으로 진행하면 됩니다.  
설치 완료 후 PowerShell을 **새로 열고** 다시 확인합니다.

```powershell
git --version
```

---

## Step 3 — Git 사용자 정보 확인

Sourcetree를 통해 1주차 실습을 완료한 경우  
이미 설정되어 있을 가능성이 높습니다.  
아래 명령어로 확인만 하세요.

```bash
git config --global user.name
git config --global user.email
```

본인 이름과 이메일이 출력되면 설정 완료 상태입니다.

아무것도 출력되지 않는 경우에만 아래와 같이 설정하세요.

```bash
git config --global user.name "본인 이름"
git config --global user.email "GitHub 가입 이메일"
```

> 💡 GitHub 가입 이메일과 동일하게 설정해야  
> GitHub 웹에서 commit 작성자가 올바르게 표시됩니다.

---

## Step 4 — git-practice 폴더로 이동

Terminal / PowerShell에서 git-practice 폴더로 이동합니다.

**Mac:**
```bash
cd Documents/git-practice
```

**Windows:**
```powershell
cd Documents\git-practice
```

현재 위치 확인:

```bash
# Mac
pwd

# Windows
cd
```

git-practice 폴더 안에 있는지 확인합니다.

```bash
git status
```

아래와 같은 메시지가 나오면 정상입니다.

```
On branch main
nothing to commit, working tree clean
```

> ⚠️ `not a git repository` 메시지가 나오면  
> 폴더 경로가 잘못된 것입니다. Sourcetree에서  
> git-practice 우클릭 → Show in Finder/Explorer로  
> 실제 경로를 확인하세요.

---

## Step 5 — class remote 추가

수업용 원격 저장소를 `class`라는 이름으로 추가합니다.

```bash
git remote add class https://github.com/[org이름]/gradebook-class.git
```

추가된 remote 목록을 확인합니다.

```bash
git remote -v
```

아래와 같이 두 개의 remote가 보이면 정상입니다.

```
class   https://github.com/[org이름]/gradebook-class.git (fetch)
class   https://github.com/[org이름]/gradebook-class.git (push)
origin  https://github.com/본인ID/git-practice.git (fetch)
origin  https://github.com/본인ID/git-practice.git (push)
```

> 💡 `origin`은 본인의 git-practice repo  
> `class`는 수업용 공용 repo입니다.  
> remote 이름은 자유롭게 붙일 수 있으며 오늘은 `class`로 통일합니다.

---

## Step 6 — 실습 지시서 Pull

class remote에서 오늘 실습 지시서를 받아옵니다.

```bash
git pull class main
```

아래와 같은 메시지가 나오면 정상입니다.

```
From https://github.com/[org이름]/gradebook-class
 * branch            main -> FETCH_HEAD
Updating xxxxxxx..xxxxxxx
Fast-forward
 INSTRUCTIONS_lecture2.md | ...
```

로컬 폴더에 `INSTRUCTIONS_lecture2.md`가 생겼는지 확인합니다.

```bash
# Mac
ls

# Windows
dir
```

VS Code에서 파일을 열어 내용을 확인하세요.

```bash
code INSTRUCTIONS_lecture2.md
```

> 💡 `code` 명령어가 안 되는 경우  
> VS Code에서 직접 파일을 열면 됩니다.  
> File → Open File → INSTRUCTIONS_lecture2.md 선택

---

## Step 7 — Sourcetree에서 확인

Sourcetree에서 git-practice repo를 열고 확인합니다.

- Unstaged 칸에 `INSTRUCTIONS_lecture2.md`가 보이는지 확인
- 이 파일은 실습 참고용이므로 **commit하지 않아도 됩니다**

> 💡 commit하지 않으려면 `.gitignore`에 추가하면 됩니다.
> 
> ```
> INSTRUCTIONS_lecture2.md
> ```

---

## 환경 설정 완료

아래 항목이 모두 확인되면 준비 완료입니다.

- [ ] `git --version` 정상 출력
- [ ] `git config` 사용자 정보 설정 완료
- [ ] `git status` — git-practice 폴더에서 정상 출력
- [ ] `git remote -v` — origin과 class 두 개 표시
- [ ] `INSTRUCTIONS_lecture2.md` 로컬에 생성 확인

이제 실습 지시서(`INSTRUCTIONS_lecture2.md`)를 따라 진행합니다.
