---
title: "MSA 기반 풀스택 프로젝트 구조"
date: 2025-06-30
categories: [DevLog]  # ✅ 꼭 있어야 함!
permalink: /rootproject/
tags:
  - MSA
  - React
  - SpringBoot
  - FastAPI
  - Kubernetes
---
이 구조는 MSA 기반 풀스택 프로젝트를 시작할 때 매우 유용합니다.

## ✅ 프로젝트 구조
```bash
root-project/
├── frontend/                # 🎨 React, Vue 프론트엔드
├── backend/                 # 🔒 Spring Boot (인증, 회원 등 MSA)
├── aiserver/                # 🤖 FastAPI 기반 AI 분석 서버
├── nginx/                   # 🌐 Nginx 설정 및 정적 파일
├── k8s/                     # ☸️ 쿠버네티스 배포 리소스 
├── infra/                   # ☁️ IaC 구성 
├── docs/                    # 📝 프로젝트 문서 
├── .github/
│   └── workflows/
│       └── deploy.yml       # GitHub Actions → K8s 배포로 확장 가능
├── docker-compose.yml       # 🐳 로컬 개발용
├── .gitignore               # 🔒 node_modules, .env 등
├── .env.example             # ⚙️ 환경변수 예시
└── README.md                # 📘 프로젝트 개요
```
---

## ✅ 프로젝트 설명
```bash
| 폴더                  | 역할                          
| -------------------- | ---------- 
| `frontend/`          | 사용자가 보는 화면  **웹사이트나 앱의 화면을 만드는 폴더**예요!             
| `backend/`           | 기능과 저장소     **회원가입, 로그인, 데이터 저장 같은 뇌 역할**이에요       
| `aiserver/`          | AI 분석 기능    **똑똑한 분석을 담당하는 친구**예요                 
| `nginx/`             | 문지기 역할      **누가 어디로 갈지 길 안내해주는 문지기**예요            
| `k8s/`               | 배포와 운영      **프로젝트를 세상에 올리고 지키는 병사들**이에요           
| `infra/`             | 클라우드 설정     **AWS에 집 짓는 설계도**라고 보면 돼요              
| `docs/`              | 설명서         **프로젝트 사용법이나 설계도를 적는 공책**이에요           
| `.github/workflows/` | 자동 배포       **자동으로 일하게 만드는 로봇 명령서**예요              
| `docker-compose.yml` | 로컬 개발 환경    **개발자들이 내 컴퓨터에서 테스트할 수 있게 도와주는 파일**이에요 
| `.env.example`       | 환경변수 예시     **비밀번호, 키 같은 설정을 따로 관리해주는 안내서**예요      
| `README.md`          | 소개 문서       **이 프로젝트가 뭔지 설명해주는 표지**예요              
```
---