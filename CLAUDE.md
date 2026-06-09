# pipeline-diff 프로젝트 규칙

## 프로젝트 개요
공공영업1팀 주간 파이프라인 변동이력 분석 웹사이트.
전주/금주 엑셀 파일(인벤토리 또는 매출목록)을 업로드하면
신규·삭제·금액증가·금액감소 항목을 자동으로 분석해 표시한다.

## GitHub 정보
- 계정: soyea0529-it
- 레포지토리: soyea0529-it/pipeline-diff
- 배포: GitHub Pages (main 브랜치 기준)
- 배포 URL: https://soyea0529-it.github.io/pipeline-diff

## 파일 구조
- `pipeline-diff.html` — 메인 웹사이트 파일 (단일 파일로 모든 기능 포함)

## 기술 스택
- 순수 HTML/CSS/JavaScript (서버 불필요)
- xlsx.js 라이브러리 (엑셀 파일 읽기)
- GitHub Pages로 무료 배포

## 데이터 구조

### 인벤토리 파일 컬럼
- 키: 프로젝트코드
- 금액: 총 합계 (백만원 단위)
- 주요 컬럼: 고객사명, 프로젝트명, 영업담당자, 수주확도

### 매출목록 파일 컬럼
- 키: 매출 번호
- 금액: 합계 (원 단위)
- 주요 컬럼: 고객사명, 프로젝트명, 매출실적영업대표, 상태

## 변동 분류 기준
- 신규: 전주에 없고 금주에 있는 항목
- 삭제: 전주에 있고 금주에 없는 항목
- 증가: 금액이 증가한 항목
- 감소: 금액이 감소한 항목
- 유지: 변동 없는 항목

## 코드 작성 규칙
- 단일 HTML 파일로 유지 (pipeline-diff.html)
- 외부 라이브러리는 CDN만 사용 (설치 불필요)
- 한국어 UI 유지
- 다크 테마 유지 (배경색 #0f1117 기준)
- 금액 표시: 인벤토리는 백만원, 매출은 억/백만원 단위로 자동 변환

## GitHub 작업 규칙
- 작업 완료 후 반드시 main 브랜치에 push
- 커밋 메시지는 한국어로 작성
- 예: "인벤토리 탭 영업담당자 컬럼 추가"

## Claude Code 시작 방법 (매번)
1. Windows키 + R → cmd → 엔터
2. `cd pipeline-diff` 입력 → 엔터
3. `claude` 입력 → 엔터

## 작업 히스토리

### 2026-06-09 — 프로젝트 최초 구축
**목표:** 주간 파이프라인 변동이력을 자동으로 분석하는 웹사이트 개발 및 GitHub 배포 환경 구축

**진행 내용:**

1. **웹사이트 설계 및 개발 (claude.ai)**
   - 인벤토리 파일(100%/75%/50%/90%) 및 매출목록 파일 구조 분석
   - `pipeline-diff.html` 단일 파일로 웹사이트 완성
   - 기능: 전주/금주 엑셀 업로드 → 신규·삭제·증가·감소·유지 자동 분류
   - 상단 요약 카드 (건수 + 금액), 필터/검색, CSV 내보내기 포함
   - 다크 테마, 인벤토리/매출 탭 분리 구성

2. **개발 환경 구축 (Windows)**
   - Node.js v24.16.0 설치
   - Claude Code v2.1.169 설치 및 claude.ai 계정 로그인
   - GitHub CLI v2.93.0 설치
   - Git v2.54.0 설치
   - PowerShell 보안 정책 해제 (`Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`)

3. **GitHub 연동**
   - GitHub 계정 생성: soyea0529-it
   - 레포지토리 생성: pipeline-diff (Public)
   - `gh auth login` 으로 GitHub CLI 인증 완료
   - `gh repo clone soyea0529-it/pipeline-diff` 로 로컬 클론
   - `/install-github-app` 으로 Claude Code ↔ GitHub Actions 연동 완료
   - GitHub Actions setup complete 확인

**현재 상태:**
- Claude Code에서 명령 → GitHub PR 자동 생성 → 승인(Merge) → 자동 배포 환경 구성 완료
- `pipeline-diff.html` 파일은 아직 GitHub에 올리지 않은 상태 (다음 작업 예정)

**다음 작업 예정:**
- `pipeline-diff.html` GitHub main 브랜치에 업로드
- GitHub Pages 배포 설정 (Settings → Pages → main 브랜치)
- 웹사이트 접속 URL 확인: https://soyea0529-it.github.io/pipeline-diff

---

---

### 2026-06-09 — 2차 작업 (완료)

#### 추가된 기능

**파이프라인 추이 탭**
- 100%/90%/75%/50%(F) 구간별 주차별 잔액 선형 그래프
- 마우스 호버 시 신규/수주/실주 건수 및 금액 표시
- JSON 붙여넣기로 데이터 입력, 로컬스토리지(pipeline_weekly_data) 저장
- 입력 형식: [{"week":"260106","p100":5000,"p90":8000,"p75":12000,"p50":3000,"new":{"count":2,"amount":1500},"won":{"count":1,"amount":800},"lost":{"count":0,"amount":0}}]

**매출 추이 탭**
- 분기 목표선(빨강): Q1 5,253 / Q2 14,178 / Q3 30,101 / Q4 82,068백만 (하드코딩)
- 누적 실적선(초록 점선): 주차별 포인트
- 슬라이더 2개로 가로축 구간 확대/축소
- 주차 포인트 클릭 시 사업기회/매출 현황 상세 패널 표시
- 태그 종류: 수주/실주/신규/확도변경/연도이동/소액변동
- JSON 붙여넣기로 데이터 입력, 로컬스토리지(revenue_weekly_data) 저장

#### 다음 작업 예정
- 인벤토리 비교 프로그램: 전주/금주 엑셀 8개 업로드 → 변동 비교 엑셀 다운로드 + 텍스트 요약
  - 신규/삭제/금액변동/구간이동 자동 분류
  - 제품 감소 항목 강조 표시
  - 구간 이동 추적 (예: 90%→100%)
  - 전주대비 항목별 금액 변동 한줄 요약
- 보고문 자동 초안 생성 기능

#### 인벤토리 파일 구조 (중요)
- 헤더가 3개 행에 걸쳐있음 (row[2]: 기본헤더, row[3]: 금액대분류, row[4]: 금액세부)
- 컬럼 인덱스가 파일(100%/90%/75%/50%)마다 다름 → 반드시 동적으로 탐지해야 함
- 제품합계: row[4]에서 '제품 합계' 찾기
- 상품/용역/유지보수/총합계: row[3]에서 찾기
- 데이터 시작: row[5]부터

---

## 환경 정보
- OS: Windows 10
- Node.js: v24.16.0
- GitHub CLI: v2.93.0
- Git: v2.54.0
- Claude Code: v2.1.169
- 계정 이메일: soyea0529@gmail.com
