# Story_Flow_Analysis_Text_Editor

URL: [https://story-flow-analysis.kro.kr]

## 1. 개발 환경
### 프론트엔드
1. Runtime Environment: Node >= v20.9.0
2. Frontend Library: React v18.2.0
3. State Management Library: react-redux ^9.1.0, @reduxjs/toolkit ^2.2.3, redux-thunk ^3.1.0
4. Editor Library: react-quill ^2.0.0
5. CSS Library: styled-components ^6.1.8
6. Other middlewares: axios ^1.6.8, bootstrap ^5.3.3, dom-to-image ^2.6.0, react-chrono ^2.6.1, react-force-graph-2d ^1.25.5, react-pro-sidebar ^1.1.0, react-spinners ^0.13.8
### 백엔드
1. Runtime Environment: Node >= v20.9.0
2. Backend Framework: express ^4.19.2
3. Database Server: MySQL
4. Database Middleware: mysql2 ^3.9.3
5. ORM Middleware: sequelize ^6.37.2, sequelize-cli ^6.6.2
6. Session Management Middleware: cookie-parser ^1.4.6, express-mysql-session ^3.0.0, express-session ^1.18.0
7. Encryption Middleware: bcrypt ^5.1.1
8. Other middlewares: axios ^1.6.8, body-parser ^1.20.2, cors ^2.8.5, dotenv ^16.4.5
### 클라우드 리소스
1. Azure VM
2. Azure Blob
3. Azure OpenAI
### CI/CD
1. 프론트엔드: Github Action
   - frontend 브랜치에 push하면 Github Action에 의해 자동으로 빌드된 후 Azure VM에 접속하여 build 파일을 갱신함 (frontend 브랜치의 .github/workflows 폴더에서 워크플로우 파일(.yml) 확인 가능)
   - React는 기본적으로 CSR을 지원하므로 정적파일로 빌드 후 정적파일 호스팅이 가능한 서버(여기서는 Azure VM에 배포되고 있는 백엔드)에 업로드만 해도 정상적으로 실행이 됨
   - development 브랜치의 server.js 파일에서 const buildPath = path.join(__dirname, 'build');
app.use(express.static(buildPath)); 부분이 정적 파일 제공에 해당함
2. 백엔드: Github Action
   - development 브랜치에 push하면 Github Action에 의해 Azure VM에 git pull됨 (development 브랜치의 .github/workflows 폴더에서 워크플로우 파일(.yml) 확인 가능)
3. PM2 설치
   - npm install pm2 -g
   - 이후 실행은 워크플로우 파일에 의해 자동으로 됨 (pm2 restart server 등)
### Web Server(Nginx)
- 설치: sudo apt-get update 후 sudo apt-get install nginx
-  내도메인.한국 에서 도메인 발급 후 /etc/nginx/sites-available/default 파일에 도메인을 추가함

## 2. 브랜치
1. 프론트엔드 최종 배포: frontend
2. 백엔드 최종 배포: development
3. 그 외
   - backend 브랜치: 백엔드 부분 데모 (이후 배포 문제로 아예 development로 옮김)
   - feature/redux: Redux 적용 (frontend로부터 분기)
   - feature/editor: Quill Editor 설정 관련 (frontend로부터 분기)

## 3. 코드 실행 방법
1. 프론트엔드
   - 코드 다운로드
   - npm install
   - npm start (API 호출 부분은 실행 안 될 수 있음)
2. 백엔드
   - 코드 다운로드
   - npm install
   - MySQL에 story_flow_analysis_text_editor 데이터베이스 생성
   - npx sequelize db:migrate --env development 명령어 실행 (마이그레이션 과정)
   - nodemon server.js (만약 안된다면 node server.js)
3. 배포 (Azure VM을 사용한다고 가정)
   - npm install pm2 -g
   - sudo apt-get install nginx
   - 프론트엔드: Visual Studio Code 등에서 터미널을 띄우고 npm run build하면 생기는 build 폴더를 위 [백엔드 코드 폴더] 에 폴더 째로 넣음
   - 백엔드: VM에서 git pull 하여 코드를 내려받은 다음 cd [백엔드 코드 폴더] 한 후 pm2 start server.js --name server

## 4. 홈페이지 사용 가이드
#### 1. 도메인 진입
- 로그인/회원가입 중 고를 수 있음
#### 2. 회원가입 클릭 후 회원가입 진행
- ID는 알파벳으로 시작하며, 최소 5글자 이상이어야 함
- 비밀번호는 적어도 1개의 대문자, 소문자, 그리고 숫자를 포함한 8글자 이상이어야 함
- 비밀번호 란과 비밀번호 확인 란에 적은 내용은 일치해야 함
- ID, 비밀번호, 비밀번호 확인 중 하나라도 제대로 입력하지 않았다면 서버에 정보가 제출되지 않음
#### 3. 로그인 페이지로 넘어가서 가입한 계정으로 로그인
- 올바른 ID와 비밀번호를 입력하지 않으면 다음 단계로 넘어가지 않음
#### 4. 로그인하면 /post-list 페이지로 진입 가능
- 로그아웃하기 전까진 /, /login으로 재진입 불가능
- 우측 상단의 로그아웃 버튼을 클릭하여 로그아웃 가능
- 하단의 + 를 클릭하여 새 글 작성 가능함
#### 5. 4에서 + 를 클릭하여 /post-write 페이지로 진입하고, 텍스트 에디터에 글 작성 가능
- 에디터 메뉴에 있는 모든 기능들은 사용 가능함 (글자 크기, 좌측/중앙/우측 정렬, 볼드체, 이탤릭체, 밑줄, 취소선, 번호 매기기, 기호 매기기, 글자 색, 글자 배경색, 하이퍼링크, 이미지 업로드)
- 좌측에 있는 사이드바에서 자기가 지금까지 작성한 글 중 아무거나 누르면 그 글의 세부정보 페이지로 넘어감
- 좌측 하단의 글 목록 보기 버튼을 누르면 /post-list 페이지로 넘어감
- 좌측 하단의 로그아웃 버튼을 누르면 로그아웃됨
- 중앙 하단의 저장 버튼을 누르면 글이 저장됨
- 중앙 하단의 취소 버튼을 누르면 경고창이 한 번 뜬 후 페이지를 나가게 됨
#### 6. AI 기능 사용: 소설 작성 후 /(슬래시)를 눌러 드롭다운 표시
- 슬래시를 누르면 약간의 페이드인 애니메이션과 함께 7가지의 기능 종류가 있는 드롭다운이 표시됨
- 드롭다운을 띄운 후 화면의 다른 부분을 클릭하면 다시 드롭다운이 사라짐
- 사용할 기능을 누르면 30초~1분 정도 로딩 화면을 표시한 후 결과가 나옴
- ★rate-limit 정책으로 인해 종종 오류가 날 수 있습니다. F12를 눌러 네트워크탭에서 확인하면 500 Internal Error가 뜰 텐데 구체적으로는 429 too many requests error를 OpenAI에서 반환합니다.
- ★이는 예산의 한계상 API키 하나로만 시스템을 구현하고 있어서 그런데, 실제로 서비스를 한다면 유료 회원에 한해 API키를 한명씩 각각 발급해주는 식의 설계가 추가로 필요해 보입니다.
- 너무 짧거나 의미 없는 문자열(ㅁㅇㅇㅇㅇ 등) 등을 입력할 경우 Error가 뜨거나 올바르지 않은 답변이 올 수 있음 (최소 100자 권장)
- 텍스트 형태로 답변을 반환하는 경우(요약하기, 주제 찾기, 키워드 추출, 인물 수 분석, 이야기 흐름 판단) 에디터 내부 하단에 파란색 글자로 추가됨
- 이미지 형태로 답변을 반환하는 경우(타임라인 분석, 인물 관계 분석) 에디터 내부 하단에 이미지가 추가됨
#### 7. 저장 버튼을 눌러 저장 가능. 그러나 자동저장이 가능하므로 꼭 저장 버튼을 누르지는 않아도 됨
- 자동저장은 1분마다 이루어지며, AI 기능을 사용하기 직전과 에디터 창을 끌 때도 에디터의 내용이 자동으로 저장됨
- 저장될 때마다 에디터 우측 하단에 저장되었다는 문구와 저장 시각이 5초간 표시됨
- 저장 시 제목이 입력되지 않았을 경우 저장 날짜를 임시 제목으로 하여 저장됨
#### 8. /post-list로 돌아온 후, 작성한 글의 제목을 클릭하면 이전에 작성했던 글 세부 내용 조회 가능
- 혹은 저장 후 사이드바에서 글 제목을 눌러도 세부 내용 조회 가능
#### 9. 8번에서 '수정' 버튼을 눌러 글 수정 가능
#### 10. /post-list의 헤더나 /post-write의 사이드바 등에 있는 '로그아웃' 버튼을 눌러 로그아웃 시 /login으로 리다이렉트됨

## 5. 에러 관련
#### 1. 세션 만료: 로그인 후 24시간이 지나 세션만료가 됐는데 페이지를 여전히 브라우저에 켜놨을 경우 발생
- alert이 뜬 후 자동으로 로그아웃 됨
#### 2. AI 기능 사용 중 에러
- gpt-3.5의 분간 한도를 다 쓴 것이므로 잠깐 기다린 후 다시 사용
