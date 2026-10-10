# METAMATCH v7.4 재배포 패키지

## 파일
- `index.html` : GitHub Pages 공개 배포용. DEV 패널 숨김.
- `metamatch_v7_4_dev.html` : 무료/정밀 결과 바로보기 DEV 포함본.
- `metamatch_v7_4_supabase.sql` : Supabase 데이터 수집용 SQL.

## 배포 순서
1. Supabase Dashboard → SQL Editor에서 `metamatch_v7_4_supabase.sql` 전체 실행.
2. SQL 실행 후 아래 확인 쿼리 실행:
   `select count(*) from public.metamatch_responses;`
3. GitHub Pages 저장소의 기존 `index.html`을 이 패키지의 `index.html`로 교체.
4. GitHub Pages가 새 버전을 배포한 뒤 실제 설문을 1회 끝까지 진행.
5. Supabase SQL Editor에서:
   `select * from public.metamatch_v7_analytics order by created_at desc limit 5;`
   로 무료 응답 저장을 확인.
6. 정밀분석까지 진행한 뒤 같은 행의 `completion_stage`가 `precision`인지 확인.

## 이번 버전부터 추가 저장
- `age_band`를 별도 컬럼으로 저장
- `consent_version = 2026-10-v1`
- 무료 37문항 원응답 전체 `free_answers`
- 무료 계산 결과 `free_analysis`
- 정밀 10문항 원응답 전체 `precision_answers`
- 정밀 분석 결과 `precision_analysis`
- MBTI / 주기능 / 보조기능 / 추천 전략 / 변화 의향·여력
- 무료·정밀 완료 시각

## 보안
브라우저의 publishable key는 공개되어도 되는 키입니다. 대신 RLS로:
- 익명 사용자는 새 무료 응답 INSERT만 가능
- 응답 SELECT/UPDATE/DELETE 불가
- 정밀 결과 업데이트는 SECURITY DEFINER RPC 한 개로만 가능
하게 구성했습니다.

DEV 미리보기는 `DEV_PREVIEW=true` 상태에서 DB 저장을 막습니다.
