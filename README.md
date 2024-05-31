# Story_Flow_Analysis_Text_Editor

## 1. 개발 환경
### 프론트엔드
1. Runtime Environment: Node >= v20.9.0
2. Frontend Library: React v18.2.0
3. State Management Library: react-redux ^9.1.0, @reduxjs/toolkit ^2.2.3, redux-thunk ^3.1.0
4. Editor Library: react-quill ^2.0.0
5. CSS Library: styled-components ^6.1.8
6. Other middlewares: axios ^1.6.8, bootstrap ^5.3.3, dom-to-image ^2.6.0, react-chrono ^2.6.1, react-force-graph-2d ^1.25.5, react-pro-sidebar ^1.1.0,
   
### 백엔드
1. Runtime Environment: Node >= v20.9.0
2. Backend Framework: express ^4.19.2
3. Database Server: MySQL
4. Database Middleware: mysql2 ^3.9.3
5. ORM Middleware: sequelize ^6.37.2, sequelize-cli ^6.6.2
6. Session Management Middleware: cookie-parser ^1.4.6, express-mysql-session ^3.0.0, express-session ^1.18.0
7. Encryption Middleware: bcrypt ^5.1.1
8. Other middlewares: axios ^1.6.8, body-parser ^1.20.2, cors ^2.8.5, dotenv ^16.4.5

## 2. 개발 브랜치
1. Frontend: frontend
2. Backend: development
3. 그 외
   - backend 브랜치: 백엔드 부분 데모 (이후 배포 문제로 아예 development로 옮김)
   - feature/redux: Redux 적용 (frontend로부터 분기)
   - feature/editor: Quill Editor 설정 관련 (frontend로부터 분기)

## 3. 실행 방법
1. 도메인 진입
2. 회원가입 클릭 후 회원가입 진행
3. 로그인 페이지로 넘어가서 가입한 계정으로 로그인
4. 로그인하면 /post-list로 진입 가능 (로그인 후에는 로그아웃하기 전까진 /, /login으로 재진입 불가능)
5. + 를 눌러 새 글 작성 시작
6. 소설 작성 후 /(슬래시)를 눌러 드롭다운 표시
7. 사용할 기능 클릭
8. 1) <<요약하기/주제 찾기/키워드 추출/인물 수 분석/이야기 흐름 판단 中 선택>> 에디터 하단에 분석 결과가 텍스트로 삽입됨
   2) <<인물 관계 분석/타임라인 분석 中 선택>> 에디터 하단에 분석 결과가 이미지로 삽입됨
9. 저장 버튼을 눌러 저장 가능. 그러나 1분마다 자동저장이 되고, 에디터 페이지에서 나가면 자동으로 저장되므로 꼭 누르지는 않아도 됨
10. /post-list로 돌아온 후, 작성한 글의 제목을 클릭하면 아까 작성했던 글 세부 내용 조회 가능
11. 9번에서 '수정' 버튼을 눌러 글 수정 가능
12. /post-list의 헤더나 /post-write의 사이드바 등에 있는 '로그아웃' 버튼을 눌러 로그아웃 시 /login으로 리다이렉트됨
