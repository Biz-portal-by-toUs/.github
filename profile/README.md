# <p align="center"> 🧩 비즈포털 BizPortal <p>
### <p align="center">
결재·일정·조직을 한 곳에 모아 “요약 → 결정 → 실행” 흐름을 만드는 전자결재 중심 그룹웨어 <br>
&nbsp;&nbsp;&nbsp;&nbsp;<img width="915" height="357" alt="image" src="https://github.com/user-attachments/assets/5c72b8e2-5658-4ab5-bb29-03238d900bee" /><br>
&nbsp;&nbsp;&nbsp;전자결재 · 일정 · 회의 · 채팅 · 알림 · 통합검색 · AI 챗봇을 한 곳에서 🏢✨ <p>
##### <p align="center"> URL : {서비스 URL} <p>
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

## 🛠 Tech Stack
<br>
<div align="center">

분야| 사용 기술|
:--------:|:------------------------------:|
**Frontend** | Thymeleaf
**Backend** | Spring Boot (Tomcat) · FastAPI (Uvicorn)
**DB/Cache** | MySQL (RDS) · Redis (ElastiCache) · MongoDB Atlas
**Search/Vector** | Elasticsearch (EKS + EBS PVC) · Weaviate (EKS + EBS PVC)
**DevOps** | AWS EC2 · AWS EKS · ALB · Ingress · Jenkins
**AI** | RAG · Text-to-SQL · OpenAI API

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
<img width="1290" height="500" alt="ERD" src="{ERD 이미지 URL}">

<br>

## 📙 API
<img alt="API" src="{Swagger/문서 이미지 URL 또는 링크}">

<br>

## 🧑‍💻 How to Start
> 아래는 “프로젝트 운영 방식” 기준의 기본 예시입니다. (레포 구조/파일명은 너희 구조에 맞춰 수정)

### 1) Backend (Spring Boot)
```bash
# example
./gradlew clean build
java -jar build/libs/{spring-app}.jar
