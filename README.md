# codyssey-mission-1
제출 저장소.
### 1. 프로젝트 개요
개발 환경 구축 및 Docker/Git 기초 익히기

### 2. 기술문서
# 1. 실행 환경
- OS: (Windows 11 pro)
- Git 버전: (2.56.0.windows. 3)
- Docker 버전: (29.9.2, build a82e528)

- ## 2. 수행 항목 체크리스트
- [O] 터미널 환경 설정 완료
- [x] 사용자 권한 설정 및 확인
- [O] Docker 설치 및 실행 확인
- [x] Dockerfile 작성 및 빌드 테스트
- [x] 포트 포워딩 및 볼륨 마운트 설정
- [x] Git 로컬 저장소 초기화 및 GitHub 원격 저장소 연결

## 3. 검증 방법 및 결과
| 항목 | 검증 명령어 | 확인 내용 | 결과 링크 |
| :--- | :--- | :--- | :--- |

| **Git** | `git --version` | 2.56.0.windows. | [결과 확인(이미지)] |<img width="712" height="104" alt="스크린샷 2026-10-30 012806" src="https://github.com/user-attachments/assets/7139bfb2-7bd3-4f72-ac09-487cb60c6761" /> |

| **Docker** | `docker --version` | 29.9.2, build a82e528 | [결과 확인(이미지)] |<img width="676" height="98" alt="스크린샷 2026-10-30 013258" src="https://github.com/user-attachments/assets/42dd903b-71a7-4146-ba3c-88dfae0704a6" />|


 ## 4. 트러블슈팅

## 🔍 사례 1: 설치 환경의 제약
- **문제**: 공용 PC 사용으로 인한 도구 설치 및 버전 확인 불가
- **원인 가설**: 시스템 권한 제한으로 직접적인 설치 실습이 어려울 것으로 판단
- **확인**: 터미널 접근 권한 확인 결과, 직접 설치가 불가능한 환경임을 인지
- **해결/대안**: 최신 버전 정보를 외부 자료를 통해 수집하여 문서화하고, 웹 에디터로 미션 수행

## 🔍 사례 2: 마크다운 문법 오류
- **문제**: 작성한 검증 방법의 내부 링크가 작동하지 않음
- **원인 가설**: 링크 연결 시 사용하는 앵커(#) 이름과 섹션 제목이 일치하지 않음
- **확인**: 섹션 제목의 띄어쓰기와 대소문자를 다시 대조해 봄
- **해결/대안**: 앵커 이름을 섹션 제목과 동일하게 수정하여 클릭 시 해당 위치로 이동하도록 해결

## 터미널 조작 로그.
-**pwd** (Print Working Directory): 현재 내가 위치한 디렉토리의 절대 경로 확인

-**mkdir practice_folder** (Make Directory): 새로운 실습용 디렉토리 생성

-**cd practice_folder** (Change Directory): 생성한 디렉토리 내부로 이동

-**touch file.txt**: 빈 텍스트 파일 생성

-**touch .hidden.txt**: 파일명 앞에 점(.)을 붙여 숨김 파일 생성

-**s -la** (List All & Long format): 숨김 파일을 포함한 전체 목록을 권한, 크기, 수정일과 함께 상세 조회

<img width="786" height="1612" alt="스크린샷 2026-10-30 011908" src="https://github.com/user-attachments/assets/bfa39509-47b0-42bf-a64e-956cdd1ee21e" />

  
