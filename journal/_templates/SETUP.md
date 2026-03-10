# Obsidian Deep Work 시스템 설정 가이드 (Core 플러그인 기준)

이 vault는 아래 폴더 구조를 기준으로 합니다.
- `journal/0-d/` (Daily notes)
- `journal/1-w/` (Weekly notes, 선택)
- `journal/_templates/` (Templates)

## 1) Core Plugins 켜기
`Settings(설정) → Core plugins`에서 아래를 ON으로 바꿉니다.
- `Daily notes`
- `Templates`
- (선택) `Starred` 또는 `Bookmarks`/`Favorites`(버전에 따라 이름이 다를 수 있음)

## 2) Daily notes 설정
`Settings → Daily notes`에서 아래처럼 맞춥니다.
- `New file location` = `journal/0-d`
- (선택) `Date format` = `YYYY-MM-DD`
- `Template file location` = `journal/_templates/deep-work-daily` (확장자 없이 선택되는 UI도 있음)
- `Open daily note on startup` = ON

## 3) Templates 설정
`Settings → Templates`에서 아래처럼 맞춥니다.
- `Template folder location` = `journal/_templates`
- (선택) `Date format` = `YYYY-MM-DD` (템플릿의 `{{date}}`에 적용)

## 4) Control Board 고정(선택)
`journal/control-board.md`를 자주 보이게 만드는 방법(둘 중 하나)
- 파일 탐색기에서 `control-board.md` 우클릭 → `Add to favorites / Bookmark` (표기명은 버전에 따라 다름)
- `control-board.md`를 열고 탭 우클릭 → `Pin(고정)`

