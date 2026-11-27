# 경범 보드

경범 님의 개인 일 보드 웹앱입니다. 블로그 글감·초안·키워드와 일상 할 일을 한 곳에서 관리합니다.
원래 tiiny.host(https://board-kyeongbeom.tiiny.site/)에 올라 있던 앱을 GitHub Pages로 옮기면서,
무료 호스팅 배너가 하단 메뉴를 가리던 문제를 없애고 모바일 사용성을 보완했습니다.

## 구성

- `index.html` — 앱 전체(단독 정적 파일, 외부 라이브러리·백엔드 없음)
- `icon-180.png`, `icon-512.png` — 홈 화면 추가용 아이콘
- `manifest.webmanifest` — 웹앱 매니페스트(standalone 표시)

## 탭

| 탭 | 내용 |
| --- | --- |
| 홈 | 남은 할 일(블로그·체험단·일상), 구상·작성 중 글감, 채택 키워드, 7일 내 발행 예정 글 |
| 할 일 | 할 일 추가(구분·우선순위·마감일), 상태 필터, D-day 배지, 일상 메모 |
| 브리핑 | 완료한 일 기록(할 일을 완료로 바꾸면 자동 기록) |
| 글쓰기 | 글감 보관함, 초안 작성(제목 후보·발행 예정일·메타 설명·본문·체크리스트·출처 링크) |
| 키워드 | 월 검색량 3만 이상 채택 표시, 작성 요령 메모(모바일에서는 카드형) |
| 설정 | 화면 모드(자동/라이트/다크), 백업 내보내기·불러오기 |

## 데이터

- 모든 데이터는 브라우저 `localStorage`의 `gb-board-v2` 키에만 저장됩니다. 서버로 전송되지 않습니다.
- 이전 버전(`gb-board-v1`) 데이터도 그대로 불러옵니다.
- 주소가 바뀌면 저장 공간도 새로 시작합니다. 기존 데이터는 이전 주소에서 **설정 → 백업 파일 저장** 후
  새 주소에서 **설정 → 백업 불러오기**로 옮기면 됩니다.
- 아이폰 사파리: 공유 → 홈 화면에 추가 하면 전체 화면 앱처럼 실행됩니다.

## 키워드 스튜디오 (`studio/`) 데이터 규칙

추천 키워드를 올리는 에이전트는 아래 형식을 지켜 주세요.

### `studio/recommendations.json`

```json
{
  "updatedAt": "2026-11-25 20:34",
  "source": "블톡",
  "items": [
    {
      "keyword": "미리캔버스",
      "grade": "S급",
      "monthlySearch": 2155200,
      "monthlyPublish": 9900,
      "saturation": 0.46,
      "entry": "매우 높음",
      "speed": "1~2일",
      "adComp": "낮음",
      "blogSlots": 10,
      "reason": "블토기 브리핑 요약",
      "briefing": {
        "verdict": "전략을 세워 도전해 보세요",
        "checks": ["상위 12개가 전부 Lv8 이상", "10월부터 검색이 느는 성수기"],
        "strategy": ["종류별 온도·시간을 표로 정리", "상위 제목에 덜 보이는 실패 복구법 각도"]
      },
      "subKeywords": [
        { "keyword": "미리캔버스 무료", "monthlySearch": 5400, "saturation": 14 },
        { "keyword": "미리캔버스 템플릿", "monthlySearch": 2100 }
      ],
      "autocomplete": ["미리캔버스 무료", "미리캔버스 사용법"],
      "topPosts": [
        {
          "title": "상위 글 제목",
          "url": "https://blog.naver.com/...",
          "headings": ["소제목1", "소제목2"],
          "summary": "본문 핵심 한 줄"
        }
      ]
    }
  ]
}
```

- `briefing`: 블톡 블토기 브리핑을 그대로 옮겨 주세요. `verdict`는 판정 문구, `checks`는 `이런 점을 살펴봤어요`, `strategy`는 `전략적으로 쓰려면` 항목이에요. Claude가 `strategy`의 빈 각도를 소제목·Q&A에 반영해요.
- `subKeywords`: 블톡 키워드 맵의 2차(연관) 키워드와, 메인과 가까운 2차 키워드를 눌러 펼친 3차(확장) 키워드를 모두 넣고, 월 검색량 30 미만은 빼 주세요.
  - `saturation`(선택): 콘텐츠 포화도 %. `100%↑`는 `100`으로 적어 주세요. 포화도 100% 이상은 발행량이 검색량보다 많다는 뜻이라 본문에 곁들이는 정도로만 써요.
- 추천 후보는 키워드 맵 말고도 블톡 `유효키워드 리스트`(매주 월요일)의 `빠진 키워드`, `나와 비슷한 블로그엔 있고 나는 없는 키워드`, `키워드 추천`(내 유효키워드·최근 포스팅·경쟁 블로그)에서 찾아도 돼요. 블토기 판정이 내 레벨로 어렵다고 나온 키워드는 빼 주세요.
- `autocomplete`: 네이버 자동완성(`https://ac.search.naver.com/nx/ac?q=키워드&r_format=json&st=100`) 결과를 넣어 주세요.
- `topPosts`: 최근 1개월 네이버 블로그 검색 상위 글 5~6개를 넣어 주세요(내 블로그 `beompercar` 글은 빼 주세요).
  - `headings`: 그 글의 소제목(목차)을 순서대로 적어 주세요. 소제목이 없으면 본문 단락의 주제를 짧게 적어 주세요.
  - `summary`: 본문을 읽고 핵심 내용을 한 줄(80자 이내)로 적어 주세요. 제목을 반복하지 말고 본문에서 실제로 다룬 내용(무료 범위, 설치 순서, 가격 등)을 적어 주세요.
  - 본문을 못 읽은 글은 `headings`와 `summary`를 빈 값으로 두고, 지어내지 마세요.
- 세 칸은 없어도 화면은 동작하지만, 있으면 원고 작성 때 블톡을 다시 열지 않아도 돼요.
- 이미 쓴 키워드는 올리지 말아 주세요. `studio/written.json`(Claude가 갱신)의 `keywords`와 같거나 `titles`에 들어 있는 키워드는 화면에서 자동으로 숨겨져요.

### `studio/written.json`

- Claude가 블로그 기록(`data/post-index.jsonl`)과 노션 기작성 아카이브로 만드는 기작성 목록이에요. 직접 고치지 말아 주세요.
- `posts`: RSS로 발행이 확인된 글 목록이에요(순위 추적용). `[{ "keyword": "메인 키워드", "title": "글 제목", "url": "https://blog.naver.com/beompercar/로그번호", "publishedAt": "YYYY-MM-DD" }]`, 최신순. Claude가 글이 발행될 때마다 같이 갱신해요.

### 작성 프롬프트

- `이 키워드로 작성하기`는 키워드 정보만 담은 짧은 요청을 복사해요. 말투·분량·제목·구성 같은 작성 규칙은 Claude 쪽 `CLAUDE.md`가 정하니 프롬프트에 넣지 마세요.

## 발행 글 검색 순위 (`rankings.json`) — 수집 방식과 파일 규격

발행한 글의 네이버 노출 순위는 `sync.json`과 **다른 파일**로 비공개 저장소
`beomiya0901-byte/studio-sync`의 `rankings.json`에 쌓입니다(설정 파일과 섞이면 동기화 충돌 409가 나기 때문).
수집·기록은 `work/check_blog_rankings.py`가 합니다. **다른 에이전트(무즈 등)도 같은 파일을 읽고 쓰거나,
아래 규칙대로 직접 수집해 같은 형식으로 넣으면 스튜디오 화면이 그대로 인식합니다.**

### 1) 수집 대상

| 출처 | 조건 |
| --- | --- |
| `studio-sync/sync.json` | `kws[]` 중 `status == "발행완료"` **또는** `url` 값이 있는 항목 |
| `board` 저장소 `studio/written.json` | `posts[]`(RSS로 확인된 발행 글) 중 **최신 20편**(`--limit-written 0` 이면 전체) |

- 두 출처를 합쳐 **키워드(공백 제거 + 소문자)가 같으면 한 번만**, **같은 글 URL(`logNo`)이면 한 번만** 조회합니다.
- 같은 키워드가 `sync.json`에도 있으면 그 항목의 `id`를 키로 쓰고, `written.json`의 URL로 빈 URL을 채워
  `logNo` 대조가 되게 합니다.
- `written.json`을 못 읽어도 `sync.json` 대상만으로 계속 진행합니다(예약 실행·새 clone 환경 대비).

### 2) 조회 규칙 — 통합검색 → 블로그탭 → 미노출

1. **네이버 PC 통합검색**: `https://search.naver.com/search.naver?query=<키워드>`
   - 응답 HTML의 `<div id="fdr-...">` 루트를 페이지 위→아래 순서로 자릅니다.
   - `data-block-id`가 `web/ · review/ · blog/ · smartblock/ · atom/ · clip/ · cafe/ · kin/` 로 시작하는
     블록만 **결과 블록**으로 인정합니다(광고·관련질문·연관검색어처럼 결과가 아닌 판은 블록 번호에서 제외).
   - `data-meta-area`가 같은 카드들을 **한 블록**으로 묶고, 처음 나온 순서가 곧 **`block`(N블록)** 입니다.
     (한 블록에 웹문서 카드와 블로그 카드가 섞여도 같은 영역이면 한 블록)
   - 카드 = `fdr` 루트 1개. 스마트블록처럼 한 루트에 카드가 여러 장이면
     `"adClassName":"_fe_view_power_content"` 경계로 나눠 카드 단위로 셉니다.
   - 카드 안의 결과 링크는 `"titleHref"/"contentHref"`(없으면 `<a href>`)에서 읽고,
     블로그 홈(작성자)·`keep.naver.com`·`pstatic.net` 같은 부가 링크는 버립니다.
   - 카드 순서는 `"r":N`(`clickLog.r`) 값, 없으면 링크 순서를 씁니다 → **`rank`(블록 안 N위)**.
   - 내 글 판별: `blog.naver.com/beompercar/<logNo>` — **`logNo`까지 같아야** 그 글로 봅니다.
     (URL이 없으면 `beompercar` 글 중 첫 번째 노출)
   - 통합검색에서 찾으면 `section`은 `통합검색`, 블록 ID·`ssc`가 스마트블록이면 `스마트블록`.
2. **통합검색에 없으면 네이버 블로그탭**: `https://search.naver.com/search.naver?ssc=tab.blog.all&sm=tab_jum&query=<키워드>`
   - 블록 ID에 `blog`가 들어 있거나 `data-meta-ssc == "tab.blog.all"` 인 블록만 봅니다.
   - 카드의 `r`이 **1~30**(`MAX_RANK`)일 때만 인정하고, 여러 번 나오면 **가장 작은 순위**를 씁니다 → `블로그탭 N위`.
3. 둘 다 없으면 **`미노출`**(`label` = `30위권 밖`).
4. **조회 실패(차단·타임아웃)는 `미노출`이 아닙니다.** 그 키워드는 이번 회차 기록을 건너뛰고,
   한 건도 성공하지 못하면 `rankings.json`을 아예 쓰지 않습니다.

> 요청 사이 `--delay`(기본 1.2초) 대기, 데스크톱 크롬 UA(`Mozilla/5.0 ... Chrome/131 ...`), `Accept-Encoding: gzip` 처리.
> 네이버가 응답 구조를 바꾸면 `data-block-id`/`_fe_view_power_content`/`"r"` 앵커를 먼저 확인하세요.

### 3) `rankings.json` 스키마 (실제 파일)

```json
{
  "updatedAt": "2026-11-27T02:21:04+09:00",
  "blogId": "beompercar",
  "items": {
    "mww1emtbsb8xm": {
      "keyword": "퍼플렉시티",
      "postUrl": "https://blog.naver.com/beompercar/224492132971",
      "section": "통합검색",
      "block": 3,
      "rank": 6,
      "position": 8,
      "label": "3블록 6위",
      "source": "integrated",
      "delta": 2,
      "checkedAt": "2026-11-27 02:21",
      "history": [
        { "checkedAt": "2026-11-26 11:28", "section": "블로그탭", "block": null, "rank": 25, "label": "블로그탭 25위" },
        { "checkedAt": "2026-11-27 02:21", "section": "통합검색", "block": 3, "rank": 6, "label": "3블록 6위" }
      ]
    }
  }
}
```

| 필드 | 설명 |
| --- | --- |
| `updatedAt` | 이번 수집 시각. ISO 8601 + `+09:00` (`2026-11-27T02:21:04+09:00`) |
| `blogId` | `beompercar`(내 블로그 아이디) |
| `items` | **키 = `sync.json` 항목의 `id`, 없으면 `공백 제거 + 소문자 키워드`**(스튜디오 `normK()`와 같은 규칙) |
| `items[].keyword` | 메인 키워드 |
| `items[].postUrl` | 실제로 찾힌 글 URL(못 찾았으면 대상 URL 그대로) |
| `items[].section` | `통합검색` · `스마트블록` · `블로그탭` · `미노출` |
| `items[].block` | 통합검색 위에서 몇 번째 결과 블록인지(1부터). 블로그탭·미노출은 `null` |
| `items[].rank` | 그 블록 안에서 몇 번째 카드인지(1부터). 블로그탭이면 블로그탭 순위 |
| `items[].position` | 페이지 전체에서 몇 번째 결과 카드인지(1부터). 블록 구성이 바뀌어도 변동을 한 숫자로 비교하려고 함께 저장. 블로그탭·미노출은 `null` |
| `items[].label` | 화면에 그대로 쓰는 표기 — `3블록 6위` / `블로그탭 25위` / `30위권 밖` |
| `items[].source` | `integrated`(통합검색) · `blogtab`(블로그탭) · `none`(미노출) |
| `items[].delta` | 직전 기록 대비 변동. **`+`면 상승**, `-`면 하락. 첫 기록·영역 변경(통합↔블로그탭)·미노출→노출은 `null`(화면에서 `NEW`), 노출→미노출은 `-(직전 순번)` |
| `items[].checkedAt` | 이 항목을 확인한 시각 `YYYY-MM-DD HH:MM`(KST) |
| `items[].history` | 최근 **10회**까지. `{checkedAt, section, block, rank, label}` 를 매번 덧붙이고 앞에서 자릅니다 |

- **`total` 필드는 쓰지 않습니다.** 전체 건수는 `items` 키 개수로 세면 됩니다(필요해지면 추가 협의).
- 요청서에서 `integratedRank`·`integratedBlock`·`integratedBlockRank`·`blogTabRank` 로 부르는 값은
  이 파일에서 각각 **`section`이 `통합검색`/`스마트블록`일 때의 `rank`·`block`**, **`section`이 `블로그탭`일 때의 `rank`** 입니다.
  필드 이름을 새로 만들지 말고 이 규격 그대로 써 주세요(스튜디오가 이 이름으로 읽습니다).
- 화면은 `items[id]` → 없으면 `items[normK(keyword)]` 순서로 찾습니다. `block`이 없는 옛 기록은 `label`을 그대로 보여줍니다.
- 정렬·비교 규칙: 통합검색·스마트블록은 `position`(없으면 `block*100+rank`), 블로그탭은 `rank`로 비교하며
  **계열이 다르면(통합 ↔ 블로그탭) 서로 비교하지 않습니다.**

### 4) 실행

```bash
python3 work/check_blog_rankings.py                  # sync.json + written.json 대상 조회 후 rankings.json 갱신
python3 work/check_blog_rankings.py --dry-run        # 조회만, 저장 안 함
python3 work/check_blog_rankings.py --limit-written 0   # written.json 발행 글 전체 조회
python3 work/check_blog_rankings.py --no-written     # sync.json 대상만
python3 work/check_blog_rankings.py --test-keyword "서치어드바이저"   # 지정 키워드만 확인(저장 안 함)
```

필요한 것: `gh` CLI 로그인(저장소 읽기/쓰기), 파이썬 표준 라이브러리만 사용.

### 5) 다른 에이전트가 같은 방식으로 수집할 때 체크리스트

1. `studio-sync/sync.json`(branch `main`)과 `board` 저장소 `studio/written.json`을 읽어 대상을 만들고,
   키워드·`logNo` 중복을 제거한다.
2. 키워드마다 통합검색 URL → (`fdr` 블록 · `data-meta-area` 묶음 · `r` 순서) → 없으면 블로그탭 URL(`r ≤ 30`) 순서로 확인한다.
3. `block`/`rank`/`position`/`label`/`section`/`source`를 위 규격대로 만들고, 직전 항목과 비교해 `delta`를 계산한 뒤
   `history`에 이번 회차를 덧붙여 최근 10회만 남긴다.
4. `updatedAt`(ISO)을 갱신해 `rankings.json`을 **최신 sha로 PUT**(409/422면 다시 읽고 1회 재시도). 부분 갱신이어도
   기존 `items`의 다른 키는 지우지 않는다(읽고-합치고-쓰기).
5. 실패한 키워드는 건너뛴다(미노출로 덮지 않는다). 성공이 0건이면 파일을 쓰지 않는다.

## 자동 요청 파일 협업 규격 (`rank-request.json` · `keyword-request.json`)

스튜디오 상단의 **[순위 새로고침]** 과 **[키워드 찾기 요청]** 은 사용자가 누르면 GitHub 토큰으로
`beomiya0901-byte/studio-sync`(branch `main`, 루트)에 **요청 파일**을 PUT 합니다.
`sync.json`·`rankings.json`과 파일을 나눠 409를 피하려는 장치이고, 피클·무즈 같은 에이전트가 이 파일을 보고 일을 집습니다.

| 파일 | 버튼 | 요청받은 일 |
| --- | --- | --- |
| `rank-request.json` | 순위 새로고침 | 위 방식으로 검색 순위를 다시 수집해 `rankings.json` 갱신 |
| `keyword-request.json` | 키워드 찾기 요청 | 블톡에서 새 추천 키워드를 찾아 `board` 저장소 `studio/recommendations.json`을 채움 |

### 스튜디오가 쓰는 내용

```json
{ "status": "pending", "requestedAt": "2026-11-27T06:12:33.123Z", "source": "studio-ui" }
```

- `requestedAt`은 UTC ISO(브라우저 `toISOString()`), `source`는 `studio-ui` 고정.
- GitHub 토큰이 연결되지 않은 기기에서는 파일을 쓰지 않고, 요청 문구를 클립보드에 복사해 사용자가 피클에게 붙여넣게 합니다.
- 스튜디오는 이 파일의 상태를 화면에 쓰지 않습니다(보낸 시각만 로컬에 표시). 상태는 **에이전트끼리 덮어쓰지 않기 위한 장부**입니다.

### 상태 흐름

```
pending ──(집은 에이전트가 claim)──▶ processing ──(작업 끝)──▶ done ──(새 요청이 오면)──▶ pending …
```

| 상태 | 쓰는 쪽 | 넣는 필드 | 뜻 |
| --- | --- | --- | --- |
| `pending` | 스튜디오 | `status`, `requestedAt`, `source` | 아직 아무도 집지 않은 새 요청 |
| `processing` | 집은 에이전트 | `status`, `by`, `claimedAt` (+기존 `requestedAt`·`source` 유지) | 이 에이전트가 작업 중. **다른 에이전트는 손대지 않는다** |
| `done` | 작업한 에이전트 | `status`, `completedAt`, `completedBy`, `note`(선택) | 끝남. 결과 요약(갱신 건수·파일)을 `note`/`result`에 남긴다 |

```json
// 피클이 집었을 때
{ "status": "processing", "by": "pickle", "claimedAt": "2026-11-27T15:13:02+09:00",
  "requestedAt": "2026-11-27T06:12:33.123Z", "source": "studio-ui" }

// 피클이 끝냈을 때
{ "status": "done", "completedBy": "pickle", "completedAt": "2026-11-27T15:21:40+09:00",
  "note": "rankings.json 12건 갱신", "by": "pickle", "claimedAt": "2026-11-27T15:13:02+09:00",
  "requestedAt": "2026-11-27T06:12:33.123Z", "source": "studio-ui" }
```

### 지켜야 할 규칙

1. **읽고 → 상태 확인 → sha 받고 → 쓰기.** 먼저 GET으로 내용과 `sha`를 확인한 뒤 PUT 합니다.
2. `pending` 이면 `processing`으로 **claim**하고 시작합니다(먼저 쓴 쪽이 주인). `by`에 자기 이름(`pickle`, `muse`)을 적습니다.
3. `processing` 이면 **건드리지 않습니다.** 다만 `claimedAt`이 **30분 이상** 지난 stale 이면 회수할 수 있고,
   이때 `note`에 `stale 회수`라고 남깁니다.
4. `done` 은 새 요청이 올 때까지 그대로 둡니다(지우지 않습니다). 새 `pending`이 오면 다시 집습니다.
5. PUT이 **409/422**(다른 쪽이 먼저 씀)면 **다시 읽어 한 번만** 재시도합니다(스튜디오와 같은 규칙).
6. 요청 파일끼리, 그리고 `sync.json`·`rankings.json`과 **서로 덮어쓰지 않습니다**(파일이 다르므로 각자 갱신).
7. 작업 결과는 요청에 맞는 파일에만 반영합니다: 순위 요청 → `rankings.json`, 키워드 요청 → `studio/recommendations.json`
   (형식은 위 `recommendations.json` 절). 결과를 만들었으면 요청 파일을 `done`으로 바꿔 남깁니다.
8. 같은 요청을 두 에이전트가 동시에 집지 않도록, **claim(PUT)이 성공한 쪽만** 작업합니다.
