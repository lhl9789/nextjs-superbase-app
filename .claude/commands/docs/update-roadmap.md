완료된 태스크를 확인하고 ROADMAP.md의 체크박스를 업데이트합니다.

## 실행 절차

### 1단계: 완료 상태 수집

다음 두 소스를 병렬로 조회합니다:

- `mcp__shrimp-task-manager__list_tasks` (status: "completed") — 태스크 매니저 완료 목록
- 실제 파일 존재 여부를 `find` 또는 `ls`로 검증

### 2단계: ROADMAP.md 체크박스 매핑

`docs/ROADMAP.md`를 읽고, 각 태스크 ID(`SETUP-000`, `PARSE-001` 등)가 포함된 줄의 체크박스 상태를 확인합니다.

**완료 판정 기준** (둘 다 충족해야 체크):

1. shrimp-task-manager에서 `completed` 상태
2. 해당 태스크가 생성해야 할 파일이 실제로 존재

**파일 존재 검증 목록**:
| 태스크 ID | 검증 대상 파일/조건 |
|---|---|
| SETUP-000 | `src/app/(auth)` 디렉토리 없음 |
| SETUP-001 | `.env.example`에 `NOTION_API_KEY` 존재 |
| SETUP-002 | `src/lib/notion.ts`에 `getNotionClient` 함수 존재 |
| SETUP-003 | `src/lib/api-response.ts`, `src/lib/errors.ts` 존재 |
| SETUP-004 | `src/lib/validations/invoice.ts` 존재 |
| PARSE-001 | `src/types/invoice.ts`, `src/types/notion.ts` 존재 |
| PARSE-002 | `src/lib/notion.ts`에 `fetchInvoicePage` 함수 존재 |
| PARSE-003 | `src/lib/notion.test.ts` 존재 |
| INVOICE-001 | `src/app/api/invoice/[id]/route.ts` 존재 |
| INVOICE-002 | `src/components/invoice/InvoiceViewer.tsx` 존재 |
| INVOICE-003 | `src/components/invoice/PdfDownloadButton.tsx` 존재 |
| INVOICE-004 | `src/app/invoice/[id]/page.tsx` 존재 |
| INVOICE-005 | `src/app/invoice/[id]/not-found.tsx` 존재 |

### 3단계: ROADMAP.md 업데이트

- 완료 확인된 태스크: `- [ ]` → `- [x]`
- 미완료 태스크: 변경 없음
- 이미 `- [x]`인 항목은 그대로 유지

### 4단계: 변경 요약 출력

업데이트 후 다음 형식으로 결과를 출력합니다:

```
## ROADMAP 업데이트 결과

### 새로 완료 처리된 태스크
- [x] TASK-ID: 태스크명

### 여전히 미완료
- [ ] TASK-ID: 태스크명 (이유: 파일 미존재 / 태스크 미완료)

### 변경 없음 (이미 완료 표시)
- [x] TASK-ID: 태스크명
```

## 주의사항

- `docs/ROADMAP.md`만 수정합니다. 다른 파일은 건드리지 않습니다.
- 태스크 ID가 ROADMAP에 없으면 해당 항목은 건너뜁니다.
- TypeScript 에러 여부(`npx tsc --noEmit`)는 이 커맨드 범위 밖입니다.
