<img width="915" height="357" alt="image" src="https://github.com/user-attachments/assets/5c72b8e2-5658-4ab5-bb29-03238d900bee" />

# <p align="center"> 🧩 비즈포털 BizPortal <p>
### <p align="center">
&nbsp;&nbsp;&nbsp;전자결재 · 일정 · 회의 · 채팅 · 공유함 · 메일 · AI 챗봇을 한 곳에서 🏢✨ <p>
&nbsp;&nbsp;&nbsp;결재·일정·조직을 한 곳에 모아 “요약 → 결정 → 실행” 흐름을 만드는 전자결재 중심 그룹웨어 <br>
&nbsp;&nbsp;&nbsp;&nbsp;<br>
##### <p align="center"> URL : {www.bizportal.pro} <p>
<br>

## 📌 Table of Contents
- [Overview](#-overview)
- [Demo](#-demo)
- [System Architecture](#-system-architecture)
- [Tech Stack](#-tech-stack)
- [Core Features](#-core-features)
- [ERD](#-erd)
- [API](#-api)
- [How to Start](#-how-to-start)
- [Directory Structure](#-directory-structure)
- [Members](#-members)

<br>

## ✨ Overview
**비즈포털(BizPortal)** 은 전자결재를 중심으로 회사 업무 흐름을 한 곳에서 처리할 수 있도록 만든 **그룹웨어 서비스**입니다.  
공지/게시판/일정/회의/전자결재 데이터를 **Elasticsearch 통합검색**으로 빠르게 찾고,  
사내 규정은 **RAG 기반 챗봇**으로 질의응답하며,  
**채팅(WebSocket)** 과 **실시간 알림(SSE + Redis Pub/Sub)** 으로 업무 커뮤니케이션을 끊기지 않게 연결합니다.

<br>

## 🎥 Demo
|**메인 / 통합검색**|**전자결재**|
|:-------------------:|:---------:|
|<img width="390" height="220" alt="Main Search" src="{스크린샷 URL}">|<img width="390" height="220" alt="Approval" src="{스크린샷 URL}">|
|**AI 챗봇 (RAG + TTS)**|**회의 (STT/요약)**|
|<img width="390" height="220" alt="Chatbot" src="{스크린샷 URL}">|<img width="390" height="220" alt="Meeting" src="{스크린샷 URL}">|
|**채팅 (WebSocket)**|**알림함 (SSE)**|
|<img width="390" height="220" alt="Chat" src="{스크린샷 URL}">|<img width="390" height="220" alt="Notification" src="{스크린샷 URL}">|

> ✅ Demo 시나리오 예시  
> 1) 로그인 → 2) 메인 통합검색으로 문서/일정/회의 검색  
> 3) 전자결재 상신/결재 → 4) 채팅으로 협업 → 5) SSE 알림 수신  
> 6) 사내 규정은 챗봇으로 질의 → 7) 회의 녹음/STT/요약 확인

<br>

## 🚨 System Architecture

<img width="820" height="448" alt="image" src="https://github.com/user-attachments/assets/bc9dffc9-c99b-45cc-a4ee-ea37aaace74f" />

### Traffic Flow
`HTTPS → Load Balancer(ALB) → Ingress(EKS) → Spring Boot(Tomcat) / FastAPI(Uvicorn) → DB/Cache/Search/VectorDB`

### Infra / Data Stores
- **CI/CD**: Jenkins  
- **Compute**: AWS **EC2**, **EKS(Kubernetes)**  
- **DB**: MySQL (**Amazon RDS**), Redis (**ElastiCache**), **MongoDB Atlas**  
- **Search**: **Elasticsearch** (EKS에 Pod로 운영 + **EBS PVC**)  
- **Vector DB**: **Weaviate** (EKS에 Pod로 운영 + **EBS PVC**)  
- **External API**: OpenAI (FastAPI 연동)

<br>

## 🛠 Tech stack 
<br>
<div align =center>

분야| 사용 기술|
:--------:|:------------------------------:|
**Frontend** | <img src="https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white"> <img src="https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E"> <img src="https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB"> <img src="https://img.shields.io/badge/tailwindcss-%2338B2AC.svg?style=for-the-badge&logo=tailwind-css&logoColor=white"/> <img src="https://img.shields.io/badge/vite-%23646CFF.svg?style=for-the-badge&logo=vite&logoColor=white"/> <img src="https://img.shields.io/badge/yarn-%232C8EBB.svg?style=for-the-badge&logo=yarn&logoColor=white"> <img src="https://img.shields.io/badge/zustand-8B4513.svg?style=for-the-badge&logo=react&logoColor=FFFFFF"> <img src="https://img.shields.io/badge/Axios-5A29E4?style=for-the-badge&logo=axios&logoColor=white">
**Backend** | <img src="https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi"> <img src="https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=RabbitMQ&logoColor=white"> <img src="https://img.shields.io/badge/Celery-37814A?style=for-the-badge&logo=Celery&logoColor=white"> <img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white"> <img src="https://img.shields.io/badge/Amazon RDS-527FFF?style=for-the-badge&logo=Amazon RDS&logoColor=white"> <img src="https://shields.io/badge/FFmpeg-%23171717.svg?logo=ffmpeg&style=for-the-badge&labelColor=171717&logoColor=5cb85c"> <img src="https://img.shields.io/badge/Uvicorn-22C3E6?style=for-the-badge&logo=uvicorn&logoColor=white"> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/21/Google_Mediapipe_Logo.svg/120px-Google_Mediapipe_Logo.svg.png" width="100px"/>
**DevOps** | <img src="https://img.shields.io/badge/NGINX-009639?style=for-the-badge&logo=nginx&logoColor=black"> <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white"> <img src="https://img.shields.io/badge/github%20actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white"> <img src="https://img.shields.io/badge/Amazon%20CloudFront-232F3E?style=for-the-badge&logo=Amazon-CloudFront&logoColor=white"> <img src="https://img.shields.io/badge/Amazon_EC2-FF9900?style=for-the-badge&logo=Amazon-EC2&logoColor=black"> <img src="https://img.shields.io/badge/certbot-0072C6?style=for-the-badge&logo=let's-encrypt&logoColor=white"> <img src="https://img.shields.io/badge/Amazon S3-569A31?style=for-the-badge&logo=Amazon S3&logoColor=white">
**Monitoring** |   <img src="https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=black"> <img src="https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=Prometheus&logoColor=black"> <img src="https://img.shields.io/badge/alertmanager-E74536?style=for-the-badge&logo=alertmanager&logoColor=black"> <img src = "https://img.shields.io/badge/cadvisor-1478FF?style=for-the-badge&logoColor=black"> <img src="https://img.shields.io/badge/flower-FF69B4?style=for-the-badge&logo=flower&logoColor=white"> 
**etc** | ![Slack](https://img.shields.io/static/v1?style=for-the-badge&message=Slack&color=4A154B&logo=Slack&logoColor=FFFFFF&label=) ![Notion](https://img.shields.io/static/v1?style=for-the-badge&message=Notion&color=000000&logo=Notion&logoColor=FFFFFF&label=) ![Figma](https://img.shields.io/static/v1?style=for-the-badge&message=Figma&color=F24E1E&logo=Figma&logoColor=FFFFFF&label=) <img src="https://img.shields.io/badge/swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black"> <img src="https://img.shields.io/badge/GitKraken-179287?style=for-the-badge&logo=GitKraken&logoColor=white"> <img src="https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white">
</div>

</div>

<br>

## 💡 Core Features
### 1) 전자결재
- 결재 문서 작성/상신/승인/반려 등 전자결재 프로세스 지원  
- 업무 흐름(결재 라인/상태)에 맞춘 문서 관리

### 2) 통합검색 (Elasticsearch)
- 공지/게시판/일정/전자결재/회의 등 주요 데이터 **통합 검색**
- **Nori 형태소 분석** 기반으로 한국어 검색 품질 강화

### 3) AI 챗봇 (RAG + Text-to-SQL + Agent)
- 사내 규정 문서를 **RAG**로 검색해 근거 기반 답변
- 질문에 따라 **DB 조회(Text-to-SQL)** / **RAG** / **Hybrid**로 분기해 응답 성능 개선
- **Redis 세션**으로 사용자별 문맥 유지
- 문서 **공개/비공개 접근 제어**로 권한 기반 질의 제한
- 청크 단위 스트리밍(자연스러운 답변 흐름)

### 4) 회의 AI (STT/요약)
- 회의 녹음/파일 기반 **Whisper(STT)** 로 전사
- FastAPI AI 서비스에서 요약 생성 후 백엔드로 전달(콜백 방식)

### 5) 채팅 (WebSocket)
- 사원 간 1:1/그룹 채팅 지원  
- 실시간 메시지 송수신으로 협업 속도 향상

### 6) 실시간 알림 (SSE + Redis Pub/Sub)
- 알림 이벤트 발생 시 **SSE**로 클라이언트에 실시간 전달
- 멀티 인스턴스 환경에서 **Redis Pub/Sub**로 이벤트 브로드캐스팅
- 알림함 누적 저장 및 클릭 시 관련 화면 이동

<br>

## 💎 ERD
<img width="692" height="728" alt="image" src="https://github.com/user-attachments/assets/1164d58b-9876-4c1d-bd60-fe5b78379ed4" />
URL : (https://www.notion.so/2c9f831b95c8806c89f5ccbc102432ea?source=copy_link)

<br>

## 📙 API
<img width="1459" height="615" alt="image" src="https://github.com/user-attachments/assets/e0b2413c-5469-4384-871e-b2adc875326b" />


<br>

## 🧑‍💻 How to Start
> 아래는 “프로젝트 운영 방식” 기준의 기본 예시입니다. (레포 구조/파일명은 너희 구조에 맞춰 수정)

### 1) Backend (Spring Boot)
```bash
# example
./gradlew clean build
java -jar build/libs/{spring-app}.jar
