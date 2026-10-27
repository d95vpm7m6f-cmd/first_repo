# 소소한 한 끼 — 레시피 공유 사이트

베이지·크림 스타일의 반응형 레시피 웹앱 시작 프로젝트입니다. 공개 열람, 카테고리 필터, 검색, 재료 계량 조절, 관리자 로그인, 레시피 CRUD, 사진 업로드, Supabase Realtime 구독이 포함되어 있습니다.

## 현재 상태

- Supabase 환경변수가 없으면 샘플 레시피 3개가 표시되는 미리보기 모드로 동작합니다.
- 데이터베이스 연결 및 실제 저장/수정/삭제/사진 업로드/실시간 동기화는 Supabase 설정 후 작동합니다.
- 이 프로젝트는 아직 인터넷에 배포된 사이트가 아닙니다. Vercel 배포 후 생성된 URL을 공유할 수 있습니다.

## 1. Supabase 설정

1. https://supabase.com 에서 프로젝트를 생성합니다.
2. 프로젝트의 SQL Editor에서 `supabase-schema.sql` 전체를 실행합니다.
3. Authentication > URL Configuration에서 나중에 사용할 Vercel 도메인을 Redirect URL에 추가합니다.
4. Authentication > Providers에서 Email 로그인 링크가 활성화되어 있는지 확인합니다.
5. Project Settings > API에서 Project URL과 anon/public key를 복사합니다. `service_role` 키는 절대 프런트엔드에 넣지 마세요.

**중요한 관리자 보안:** 현재 SQL은 `authenticated` 사용자에게 레시피 편집 권한을 줍니다. 개인 사이트 운영 전에는 Supabase Auth에서 관리자 계정만 허용하고, 공개 회원 가입을 비활성화하거나 DB 정책을 관리자 이메일/UID로 제한하세요. 이메일 OTP 로그인은 프로젝트의 Auth 설정에 따라 작동합니다.

## 2. 로컬 실행

Node.js 20 이상을 설치한 뒤 프로젝트 폴더에서:

```bash
npm install
cp .env.example .env.local
```

`.env.local`을 열고 다음 값을 입력합니다.

```env
VITE_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

그다음:

```bash
npm run dev
```

## 3. Vercel 배포

1. 프로젝트 파일을 GitHub 저장소에 올립니다.
2. https://vercel.com 에서 해당 저장소를 Import 합니다.
3. Environment Variables에 `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`를 추가합니다.
4. Deploy를 누릅니다. Vercel이 공개 URL을 발급합니다.
5. 발급된 도메인을 Supabase Auth의 Site URL 및 Redirect URLs에 등록합니다.

## 기능

- 전체 레시피 검색 (제목, 소개, 재료명)
- 카테고리별 필터
- 레시피 상세 화면
- 기준 인분 변경 시 재료량 자동 계산
- 관리자 이메일 로그인 링크
- 레시피 등록/수정/삭제
- 사진 URL 등록 및 Supabase Storage 업로드
- Supabase Realtime을 통한 변경 반영
- 모바일 반응형 디자인

## 알려진 한계 / 다음 개선

- 사진 업로드 용량은 기본 5 MB로 제한했습니다.
- 레시피 상세 페이지는 앱 내부 화면이며, 각 레시피별 SEO/고유 URL은 추가 구현이 필요합니다.
- 관리자 계정 제한을 적용한 후 공개 배포하세요. anon key는 브라우저에 노출되는 키이며, 보안은 RLS 정책으로 지켜야 합니다.
