# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

Next.js + Supabase Starter Kit. App Router 기반 TypeScript 프로젝트로, Tailwind CSS와 shadcn/ui(`new-york` 스타일)를 사용하며, `@supabase/ssr`를 통해 쿠키 기반 Supabase Auth가 구성되어 있다. 소스는 저장소 루트(`app/`, `components/`, `lib/`)에 위치한다 — **`src/` 디렉토리는 존재하지 않는다.** `docs/guides/*.md` 문서들은 `src/` 기반 구조를 설명하고 있는데, 이는 다른/범용 템플릿용으로 작성된 것이라 실제 폴더 구조와 맞지 않는다. 해당 문서의 네이밍 등 컨벤션은 참고만 하고, 폴더 구조 설명은 무시할 것.

## 커맨드

- `npm run dev` — 개발 서버 실행 (localhost:3000)
- `npm run build` — 프로덕션 빌드
- `npm run start` — 프로덕션 빌드 실행
- `npm run lint` — ESLint (flat config, `next/core-web-vitals` + `next/typescript` 확장)

이 저장소에는 테스트 러너가 구성되어 있지 않다.

## 환경 변수

`.env.local`에 다음이 필요하다:
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` (anon/publishable 키)

`lib/utils.ts`의 `hasEnvVars`는 이 값들이 없을 때 인증 UI 대신 `EnvVarWarning`을 보여주고, proxy/middleware에서 세션 갱신을 스킵하는 데 쓰인다.

## 아키텍처

### Supabase 클라이언트 생성 (서로 다른 3가지 진입점 — 혼용하지 말 것)

- `lib/supabase/client.ts` — **클라이언트 컴포넌트**용 `createClient()`, `createBrowserClient` 사용.
- `lib/supabase/server.ts` — **서버 컴포넌트 / 라우트 핸들러 / 서버 액션**용 `async createClient()`, `next/headers`의 쿠키에 바인딩된 `createServerClient` 사용. Supabase 가이드에 따라 이 클라이언트를 모듈 레벨 싱글톤으로 두면 안 됨(Fluid Compute에서 문제 발생) — 항상 사용하는 함수 내부에서 새로 생성해야 한다.
- `lib/supabase/proxy.ts` — `updateSession(request)`. 루트의 `proxy.ts` 미들웨어가 모든 요청마다 인증 세션을 갱신하고, 비공개 경로에 접근하는 미인증 사용자를 리다이렉트하는 데 사용. 이 파일 안의 쿠키 처리 코드 순서를 바꾸지 말 것(파일 내 주석 참고) — 잘못 건드리면 세션이 조용히 끊길 수 있다.

`lib/supabase/database.types.ts`는 생성된 DB 타입(`Database`)을 담고 있으며 브라우저/서버 클라이언트 양쪽에서 타입이 지정된 쿼리 결과를 위해 import된다. 스키마가 바뀌면 Supabase MCP 툴/CLI로 재생성할 것 — 직접 수정하지 말 것.

### 라우팅 구조

- `app/auth/*` — login, sign-up, sign-up-success, forgot-password, update-password, confirm, error. 페이지는 얇게 구성되어 있으며, 각각 `components/`의 폼 컴포넌트 하나를 렌더링한다 (예: `components/login-form.tsx`).
- `app/protected/*` — 인증된 영역 예시. `layout.tsx`는 공통 nav/footer를 렌더링하고, `page.tsx`는 서버 사이드에서 `supabase.auth.getClaims()`로 현재 유저를 조회하고 실패 시 `/auth/login`으로 리다이렉트한다.
- 루트 `proxy.ts`의 matcher는 `_next/static`, `_next/image`, `favicon.ico`, 주요 이미지 확장자를 제외한 모든 경로를 `updateSession`으로 통과시키며, 경로가 `/`, `/login`으로 시작, 또는 `/auth`로 시작하는 경우를 제외하고는 `/auth/login`으로 리다이렉트한다.

### UI 컴포넌트

- `components/ui/*` — shadcn/ui 프리미티브(button, card, input, label, checkbox, dropdown-menu, badge). 추가 시 `npx shadcn@latest add <component>` 사용. 설정은 `components.json`에 있음 (`new-york` 스타일, Tailwind prefix 없음, CSS 변수 사용, base color `neutral`).
- `components/tutorial/*` — 스타터킷 튜토리얼용 컴포넌트(코드 블록, 셋업 단계)로, 앱 기능과 무관함 — 실제 개발 시 삭제해도 무방.
- 경로 별칭 `@/*`는 저장소 루트로 매핑됨(`tsconfig.json` 참고), 예: `@/components/ui/button`, `@/lib/utils`.

## 로컬 툴링 관련 참고

- `.claude/agents/`, `.claude/commands/`, `shrimp_data/`, `docs/`는 이전 세션에서 생성된 Claude Code/에이전트 관련 산출물(서브에이전트, shrimp-task-manager 상태, 가이드 문서)이며 애플리케이션 코드가 아니다.
- 이 프로젝트에서 활성화된 MCP 서버(`.claude/settings.local.json`): `supabase`, `playwright`, `filesystem`, `sequential-thinking`, `shadcn`, `shrimp-task-manager`.
