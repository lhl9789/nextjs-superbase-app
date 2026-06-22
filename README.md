<a href="https://demo-nextjs-with-supabase.vercel.app/">
  <img alt="Next.js and Supabase Starter Kit - the fastest way to build apps with Next.js and Supabase" src="https://demo-nextjs-with-supabase.vercel.app/opengraph-image.png">
  <h1 align="center">nextjs-superbase-app</h1>
</a>

<p align="center">
  Next.js (App Router) + Supabase Auth 기반 스타터 프로젝트
</p>

## 개요

[Next.js](https://nextjs.org) App Router와 [Supabase](https://supabase.com)를 결합한 스타터 킷입니다.

- App Router 전반(Client/Server Component, Route Handler, Proxy/Middleware)에서 동작
- `@supabase/ssr`로 쿠키 기반 Supabase Auth 구성
- 이메일/비밀번호 인증 흐름 포함 (로그인, 회원가입, 비밀번호 재설정 등)
- [Tailwind CSS](https://tailwindcss.com) + [shadcn/ui](https://ui.shadcn.com/) (`new-york` 스타일)

## 기술 스택

- Next.js (App Router), React 19, TypeScript
- Supabase (`@supabase/ssr`, `@supabase/supabase-js`)
- Tailwind CSS, shadcn/ui, lucide-react
- ESLint (`next/core-web-vitals`, `next/typescript`)

## 시작하기

1. 의존성 설치

   ```bash
   npm install
   ```

2. `.env.local` 파일을 만들고 Supabase 프로젝트 정보를 입력합니다.

   ```env
   NEXT_PUBLIC_SUPABASE_URL=[Supabase 프로젝트 URL]
   NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=[Supabase publishable/anon 키]
   ```

   값은 [Supabase 대시보드 > 프로젝트 > API 설정](https://supabase.com/dashboard/project/_/settings/api)에서 확인할 수 있습니다.

3. 개발 서버 실행

   ```bash
   npm run dev
   ```

   [http://localhost:3000](http://localhost:3000)에서 확인합니다.

## 주요 스크립트

| 명령어          | 설명               |
| --------------- | ------------------ |
| `npm run dev`   | 개발 서버 실행     |
| `npm run build` | 프로덕션 빌드      |
| `npm run start` | 프로덕션 빌드 실행 |
| `npm run lint`  | ESLint 검사        |

## 프로젝트 구조

```
app/
├── auth/        # 로그인, 회원가입, 비밀번호 재설정 등 인증 페이지
├── protected/   # 인증된 사용자만 접근 가능한 예시 페이지
└── layout.tsx   # 루트 레이아웃 (ThemeProvider 등)

components/
├── ui/          # shadcn/ui 기반 UI 컴포넌트
├── tutorial/    # 스타터킷 튜토리얼용 컴포넌트
└── *.tsx        # 인증 폼 등 기능 컴포넌트

lib/
└── supabase/    # client.ts(브라우저), server.ts(서버), proxy.ts(미들웨어) Supabase 클라이언트
```

자세한 아키텍처 설명과 개발 가이드는 [CLAUDE.md](./CLAUDE.md)를 참고하세요.

## 참고

- [Supabase Auth - Next.js App Router 가이드](https://github.com/supabase/supabase/tree/master/examples/auth/nextjs)
- [Supabase 로컬 개발 가이드](https://supabase.com/docs/guides/getting-started/local-development)
