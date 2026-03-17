# Daily Planner

**비개발자가 AI로 만든 업무 효율 최적화 웹앱**

하루를 4가지 시야(View)로 보면, 일하는 방식이 달라진다.

[Live Demo](https://daily-planner-product.netlify.app/product.html)

---

## Problem

매일 같은 일을 반복하면서도 **시간을 어디에 쓰는지, 뭘 완료했는지** 보이지 않았다.

- 주간 계획과 오늘 할 일이 분리되어 초점이 흐트러짐
- 프로젝트별 진행 상황을 한눈에 파악할 방법이 없음
- 장기 과제와 일일 업무 사이의 연결고리 부족
- "기록해야 한다"는 의식이 오히려 **일의 부담**으로 작용

> "기록이 목표가 되면 일이 돼버린다. 일의 부산물로 알아서 나오게 하는 게 핵심."

---

## Solution — 4-View System

하나의 업무를 **4가지 시각**으로 봐야 전체가 보인다.

### Today View
오늘 집중할 일 + 08:30~21:00 타임라인. 드래그로 시간 배정하면 그게 일정이 된다.

### Weekly View
월~금 5일을 한 화면에. 요일별 완료 현황과 시간 배정을 비교할 수 있다.

### History View
월별 프로젝트별 누적 완료율. 유사한 태스크를 자동으로 묶어주는 스마트 그룹핑.

### Analytics View
시간 배정 vs 실제 완료 비교. 주간 트렌드로 나의 생산성 패턴을 발견한다.

---

## Key Features

| 기능 | 설계 의도 |
|------|---------|
| **체크박스 순환** (open → 진행중 → 완료) | 상태 변화를 즉각 시각화 |
| **프로젝트 4컬럼 그리드** | 여러 프로젝트를 한눈에, 드래그로 순서 변경 |
| **타임라인 Drag & Drop** | 언제 할 건지 정하면 그게 일정 |
| **자동 이월** | 미완료 태스크는 다음날 자동 전달 |
| **워킹 가든** | 프로젝트별 연속 활동일수로 식물이 자란다 (🌰→🌱→🌿→🌸→🌳) |
| **Google Calendar 연동** | 4개 캘린더 통합, 타임라인에 시각화 |
| **반복 태스크 & 서브태스크** | 루틴 자동 생성, 큰 업무 쪼개기 |
| **템플릿** | 자주 쓰는 업무 세트를 원클릭 생성 |
| **PWA** | 앱 설치 + 오프라인 지원 |

---

## Tech Stack

| 영역 | 기술 |
|------|------|
| Frontend | React 19, TypeScript, Vite 7, Tailwind CSS v4 |
| Database | Supabase PostgreSQL (9 tables) |
| Drag & Drop | @dnd-kit |
| Icons | lucide-react |
| Calendar | Google Calendar API |
| Deploy | Netlify (static hosting) |

---

## Architecture

```
┌─────────────────────────────────────────────┐
│                   App.tsx                    │
│          DnD Context + Global State         │
├──────────┬──────────────────┬───────────────┤
│ Sidebar  │   Main Panel     │  Right Panel  │
│          │                  │               │
│ Projects │ Today: Timeline  │ Stats         │
│ Filters  │ Weekly: 5-Col    │ Today Tasks   │
│ CRUD     │ History: Monthly │ Completed     │
│ Views    │ Analytics        │ Long-term     │
│          │                  │ Captures      │
├──────────┴──────────────────┴───────────────┤
│              Supabase REST API              │
│  (todos, projects, time_slots, gcal, ...)   │
└─────────────────────────────────────────────┘
```

### Core Hooks
- `useTodos` — CRUD + 자동이월 + 드래그 리오더 + 프로젝트 관리
- `useTimeSlots` — 타임슬롯 CRUD + 드래그 리사이즈
- `useKeyboardShortcuts` — 글로벌 단축키 (n: 새 태스크, 0~9: 필터)

---

## Demo & Screenshots

**Live Demo**: [daily-planner-product.netlify.app](https://daily-planner-product.netlify.app/product.html)

> 스크린샷은 추후 추가 예정

---

## Design Philosophy

### "기록은 일의 부산물이어야 한다"
할 일을 체크하고, 시간을 배정하고, 완료하는 과정 자체가 기록이 된다. 별도로 기록하는 시간은 0.

### 제약 조건 속 창의성
워킹 가든(게이미피케이션)을 구현할 때 **새 DB 테이블 0개, npm 의존성 0개**를 추가했다. 기존 `done_at` 타임스탬프만으로 연속 활동일수를 계산한다.

### 감정 장치가 생산성을 만든다
- 완료 시 confetti 애니메이션으로 성취감
- 3일 이상 방치된 프로젝트에 넛지 배너
- 워킹 가든의 식물 성장이 "오늘도 한 걸음"을 느끼게 한다

---

## Numbers

| 항목 | 수치 |
|------|------|
| 개발 기간 | 12일 |
| 컴포넌트 | 32개 |
| 주요 기능 | 11개 |
| CSS 애니메이션 | 12+ keyframes |
| DB 테이블 | 9개 |
| 반응형 | 320px ~ 4K |

---

## Getting Started

```bash
# Clone
git clone https://github.com/[your-username]/daily-planner.git

# Install
npm install

# Environment
cp .env.example .env
# Add your Supabase URL and anon key

# Dev server (port 2500)
npm run dev

# Build
npm run build
```

---

## Built With

이 프로젝트는 **Claude Code**를 활용하여 비개발자가 설계하고 구현했습니다.

AI가 코드를 작성했지만, **왜 이 기능이 필요한지, 어떤 UX가 맞는지**는 매일 이 도구를 쓰는 사람의 관점에서 나왔습니다.

*AI-assisted development by a non-developer*
