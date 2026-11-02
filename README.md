# codyssey-mission-1
제출 저장소.

### 1. 프로젝트 개요
개발 환경 구축 및 Docker/Git 기초 익히기: 리눅스 CLI 환경 제어, Dockerfile 기반 커스텀 웹 서버 컨테이너화, 포트 포워딩, 바인드 마운트 및 Docker 볼륨 영속성 검증, Git 버전 관리 및 GitHub 원격 저장소 동기화

### 2. 기술문서

## 1. 실행 환경
- OS: Windows 11 Pro
- Shell/Terminal: Git Bash (MSYS2/UCRT64)
- Git 버전: 2.56.0.windows.3
- Docker 버전: 29.9.2, build a82e528

## 2. 수행 항목 체크리스트
- [X] 터미널 환경 설정 및 CLI 기본 조작 완료
- [X] 사용자 권한 설정 및 확인 (chmod)
- [X] Docker 설치 및 실행 확인
- [X] Dockerfile 작성 및 빌드 테스트
- [X] 포트 포워딩 접속 검증
- [X] 바인드 마운트를 통한 실시간 소스코드 변경 반영
- [X] Docker 볼륨 생성 및 데이터 영속성 검증
- [X] Git 로컬 저장소 초기화 및 GitHub 원격 저장소 연결

## 3. 검증 방법 및 결과
| 항목 | 검증 명령어 | 확인 내용 | 결과 스크린샷 |
| :--- | :--- | :--- | :--- |
| **Git** | `git --version` | 2.56.0.windows.3 | <img width="712" height="104" alt="Git 버전" src="https://github.com/user-attachments/assets/7139bfb2-7bd3-4f72-ac09-487cb60c6761" /> |
| **Docker** | `docker --version` | 29.9.2, build a82e528 | <img width="676" height="98" alt="Docker 버전" src="https://github.com/user-attachments/assets/42dd903b-71a7-4146-ba3c-88dfae0704a6" /> |

## 4. 트러블슈팅 (Troubleshooting)

### 🔍 사례 1: Windows Git Bash 환경의 Docker 볼륨 마운트 경로 변환 오류
- **문제**: Docker 볼륨 마운트 검증 중 `cat: 'C:/Program Files/Git/data/test.txt': No such file or directory` 에러가 발생하며 컨테이너 내부 경로를 인식하지 못함.
- **원인 가설**: Windows Git Bash(MSYS2) 셸이 리눅스 POSIX 절대 경로(`/data/...`)를 감지하고 호스트의 Windows Git 설치 절대 경로(`C:/Program Files/Git/...`)로 자동 변환하여 컨테이너로 전달했을 것으로 판단함.
- **확인**: 실제 터미널 출력 에러 메시지를 확인한 결과, 컨테이너 내부의 `/data`가 아니라 Git Bash 설치 경로가 인자로 주입된 것을 확인함.
- **해결/대안**: 명령어 실행 시 경로 자동 변환을 비활성화하는 환경변수 플래그인 `MSYS_NO_PATHCONV=1`을 Docker 명령어 앞에 접두어로 붙여 실행(`MSYS_NO_PATHCONV=1 docker run ...`)함으로써 리눅스 원본 경로 그대로 마운트 및 조회가 성공하도록 해결함.

### 🔍 사례 2: 작업 디렉터리 경로 오판으로 인한 최상위 홈 디렉토리 Git 초기화
- **문제**: `git init` 실행 후 `git status`를 확인했을 때, 실습 파일뿐만 아니라 Windows 시스템 파일(`AppData`, `NTUSER.DAT`, `ntuser.ini` 등) 수천 개가 스테이징 추적 대상(`Untracked files`)으로 노출되고 권한 경고가 발생함.
- **원인 가설**: 실습용 서브 폴더(`~/practice_folder`)로 이동하지 않고 사용자 최상위 홈 디렉토리(`~`, `C:/Users/si292`)에서 `git init`을 실행하여 로컬 저장소가 잘못 설정되었을 것으로 추정함.
- **확인**: 터미널 프롬프트의 현재 작업 디렉터리(`pwd`)를 확인한 결과 홈 디렉토리 위치임을 인지함.
- **해결/대안**: 잘못 생성된 홈 디렉토리의 Git 설정 숨김 폴더를 `rm -rf .git`으로 즉시 영구 삭제하여 시스템 파일 추적을 해제하고, `cd ~/practice_folder`로 명확하게 이동한 후 `git init`을 재수행하여 실습 파일 5개만 깔끔하게 추적되도록 해결함.

### 🔍 사례 3: Dockerfile 복사 경로 오타로 인한 커스텀 페이지 렌더링 실패
- **문제**: 커스텀 웹 서버 이미지를 빌드하고 포트 포워딩(`8080:80`)으로 접속했으나, 직접 작성한 `index.html` 문구가 아닌 순정 NGINX 기본 웰컴 페이지가 브라우저에 표시됨.
- **원인 가설**: `Dockerfile` 내부의 `COPY` 지시어에서 대상 웹 루트 디렉터리 경로에 오타가 발생하여 파일이 다른 위치로 복사되었을 것으로 추정함.
- **확인**: 작성된 `Dockerfile` 코드를 점검한 결과, `/usr/share/nginx/html/` 경로 중 `/usr/`가 `/user/`로 오기입된 것을 확인함.
- **해결/대안**: 경로를 `/usr/share/nginx/html/index.html`로 정상 수정하고 `docker build -t my-web-server:1.0 .`로 이미지를 재빌드하여 브라우저에 커스텀 문구가 올바르게 렌더링되도록 조치함.

## 5. 터미널 조작 로그
- **pwd** (Print Working Directory): 현재 내가 위치한 디렉토리의 절대 경로 확인
- **mkdir practice_folder** (Make Directory): 새로운 실습용 디렉토리 생성
- **cd practice_folder** (Change Directory): 생성한 디렉토리 내부로 이동
- **touch file.txt**: 빈 텍스트 파일 생성
- **touch .hidden.txt**: 파일명 앞에 점(.)을 붙여 숨김 파일 생성
- **ls -la** (List All & Long format): 숨김 파일을 포함한 전체 목록을 권한, 크기, 수정일과 함께 상세 조회

<img width="786" height="1612" alt="터미널 CLI 기본 조작 증빙" src="https://github.com/user-attachments/assets/bfa39509-47b0-42bf-a64e-956cdd1ee21e" />

## 6. 사용자 권한 설정 및 확인 실습
Windows(NTFS) 호스트 환경의 권한 모델 한계를 보완하기 위해 Ubuntu 리눅스 컨테이너 내부에서 `chmod` 명령어로 파일 및 디렉토리 권한을 변경하고 검증했습니다.

| 구분 | 대상 | 변경 전 권한 | 실행 명령 | 변경 후 권한 | 설명 |
| :--- | :--- | :---: | :--- | :---: | :--- |
| **파일** | `file.txt` | `-rw-r--r--` (644) | `chmod 755 file.txt` | `-rwxr-xr-x` (755) | 실행 권한(`x`) 추가 부여 |
| **디렉토리** | `perm_dir` | `drwxr-xr-x` (755) | `chmod 700 perm_dir` | `drwx------` (700) | 소유자 외 타 사용자 접근/읽기 차단 |

<img width="784" height="708" alt="사용자 권한 설정 결과" src="https://github.com/user-attachments/assets/8c0b8029-0ebc-406c-a6d8-ae98bc1feae6" />

## 7. Dockerfile 기반 커스텀 웹 서버 이미지 제작 및 포트 포워딩 실습
경량 웹 서버인 `nginx:alpine` 베이스 이미지에 커스텀 `index.html` 파일을 포함하는 컨테이너 이미지를 빌드하고, 포트 포워딩을 통해 호스트 브라우저에서 정상 접속되는지 검증했습니다.

### 1) 소스 파일 및 Dockerfile 작성
* **`index.html`**:
  ```html
  <h1>Hello from My Custom Docker Web Server!</h1>