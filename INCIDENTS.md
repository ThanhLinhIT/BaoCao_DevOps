# Incident Demo Guide - QA / SRE Engineer

> Muc tieu: dung 3 incident co the tao loi, xac dinh dung layer, va fix nhanh ngay tai local.
> File nay la ban chot de khop voi `backend/__tests__/integration.test.js` va `scripts/test-incidents.js`.

---

## Incident 1: `GET /api/questions` tra HTTP 500 do sai `SUPABASE_URL`

| Truong | Noi dung |
|---|---|
| Hien tuong | Goi `GET /api/questions` bi 500, trong khi `GET /api/health` van 200 OK. |
| Layer loi | L2 External (Supabase / DB connection) |
| Nguyen nhan | Bien `SUPABASE_URL` sai lam backend khong truy cap duoc Supabase. |
| Cach tao loi de demo | Sua tam `backend/.env`, dat `SUPABASE_URL` thanh gia tri sai, roi restart backend. |
| Cach fix | Tra lai `SUPABASE_URL` dung, restart backend, goi lai `GET /api/questions` phai tra JSON array. |
| Bang chung | Postman/curl, log backend, va `node scripts/test-incidents.js`. |

## Incident 2: CORS loi do sai `FRONTEND_URL`

| Truong | Noi dung |
|---|---|
| Hien tuong | Frontend goi API bi chan boi CORS; browser bao `blocked by CORS policy`. |
| Layer loi | L3 Backend (CORS middleware / config) |
| Nguyen nhan | `FRONTEND_URL` khong trung voi origin thuc te cua frontend. |
| Cach tao loi de demo | Sua tam `backend/.env`, dat `FRONTEND_URL=http://localhost:9999`, roi restart backend. |
| Cach fix | Tra lai `FRONTEND_URL=http://localhost:5173`, restart backend, kiem tra lai header CORS. |
| Bang chung | Response header, console browser, va `node scripts/test-incidents.js`. |

## Incident 3: `POST /api/quiz/submit` tra 400 khi body thieu truong bat buoc

| Truong | Noi dung |
|---|---|
| Hien tuong | Submit quiz nhung backend tra `400 Bad Request`. |
| Layer loi | L3 Backend (validation) |
| Nguyen nhan | Request body thieu `playerName` hoac `answers`. |
| Cach tao loi de demo | Gui body rong `{}` hoac chi gui mot phan du lieu. |
| Cach fix | Gui body hop le, co `playerName` va `answers`. |
| Bang chung | Response 400 truoc fix va response thanh cong sau fix. |

---

## Kich ban demo ngan

1. Incident 1: doi `SUPABASE_URL` sai, restart backend, chung minh `/api/health` van 200 nhung `/api/questions` loi 500, sau do tra lai dung va test lai.
2. Incident 2: doi `FRONTEND_URL` sai, restart backend, gui request voi `Origin: http://localhost:5173`, chung minh header CORS sai hoac browser bi chan, sau do tra lai dung va test lai.
3. Incident 3: gui `POST /api/quiz/submit` voi `{}` de nhan 400, sau do gui body hop le de fix.

## Pham vi file lien quan

| File | Vai tro |
|---|---|
| `backend/__tests__/integration.test.js` | Ghi ro 3 incident va cac baseline check tu dong |
| `scripts/test-incidents.js` | Script demo / kiem tra nhanh dung 3 incident |
