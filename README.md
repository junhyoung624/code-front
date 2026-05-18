# Code-Front: Git to AWS Deployment Pipeline
# aws 연동예정임.

> Git에 모인 코드를 AWS에 자동으로 배포하는 CI/CD 프로젝트

![Deployment Architecture](https://img.shields.io/badge/AWS-Deployment-FF9900?style=flat-square&logo=amazon-aws)
![Git Integration](https://img.shields.io/badge/Git-Integrated-F05032?style=flat-square&logo=git)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)

---

## 📋 프로젝트 개요

**Code-Front**는 Git 저장소의 코드를 자동으로 수집하고 AWS 인프라에 배포하는 엔드-투-엔드 CI/CD 파이프라인입니다. 
이 프로젝트는 개발팀의 협업 효율을 극대화하고 배포 프로세스를 자동화합니다.

### 🎯 주요 목표

- ✅ Git 코드 변경사항 자동 감지
- ✅ AWS 리소스에 자동 배포
- ✅ 배포 프로세스 완전 자동화
- ✅ 실시간 배포 상태 모니터링

---

## 🏗️ 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│                     Developer Workflow                       │
│                    (로컬 개발 환경)                           │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    Git Repository                            │
│              (GitHub/GitLab/Bitbucket)                      │
│                   (코드 저장소)                               │
└────────────────────────┬────────────────────────────────────┘
                         │ (Webhook Trigger)
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                  CI/CD Pipeline                              │
│          (GitHub Actions / Jenkins / GitLab CI)             │
│            (자동화된 빌드 및 테스트)                          │
└────────────────────────┬────────────────────────────────────┘
                         │
                    ┌────┴────┐
                    ▼         ▼
         ┌──────────────┐  ┌──────────────┐
         │  Build &     │  │  Unit Tests  │
         │  Compile     │  │  & Security  │
         └──────┬───────┘  └────┬─────────┘
                │               │
                └──────┬────────┘
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                 AWS Deployment Services                      │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐             │
│  │   EC2      │  │   S3       │  │  Lambda    │             │
│  │ Instances  │  │  Storage   │  │ Functions  │             │
│  └────────────┘  └────────────┘  └────────────┘             │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐             │
│  │    ECS     │  │   RDS      │  │  CloudFront│             │
│  │ Containers │  │  Database  │  │   CDN      │             │
│  └────────────┘  └────────────┘  └────────────┘             │
└─────────────────────────────────────────────────────────────┘
                         ▲
                         │
┌─────────────────────────────────────────────────────────────┐
│                 Monitoring & Logging                         │
│  (CloudWatch / CloudTrail / X-Ray)                          │
│           (배포 상태 실시간 모니터링)                        │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 빠른 시작

### 전제 조건

| 요구사항 | 버전 | 설명 |
|---------|------|------|
| Git | 2.30+ | 버전 관리 시스템 |
| Node.js | 16+ | 런타임 환경 |
| AWS CLI | 2.0+ | AWS 명령줄 도구 |
| Docker | 20.10+ | 컨테이너화 (선택사항) |
| npm | 8+ | 패키지 관리자 |

### 설치 절차

#### 1단계: 저장소 클론
```bash
git clone https://github.com/junhyoung624/code-front.git
cd code-front
```

#### 2단계: 의존성 설치
```bash
npm install
```

#### 3단계: 환경 변수 설정
```bash
cp .env.example .env
# .env 파일 수정 (AWS 자격증명, 배포 설정 등)
```

#### 4단계: AWS 인증 구성
```bash
aws configure
# AWS Access Key ID
# AWS Secret Access Key
# Default region (예: ap-northeast-2)
```

#### 5단계: 배포 실행
```bash
npm run deploy
```

---

## 📦 주요 구성 요소

| 구성 요소 | 역할 | 기술 스택 | 상태 |
|----------|------|----------|------|
| **Git Hook** | 코드 변경 감지 | Webhook | ✅ Active |
| **CI/CD Pipeline** | 빌드 및 테스트 자동화 | GitHub Actions | ✅ Active |
| **Build Service** | 애플리케이션 빌드 | Node.js / Docker | ✅ Active |
| **AWS Deployment** | 클라우드 배포 | CloudFormation / Terraform | ✅ Active |
| **Monitoring** | 실시간 모니터링 | CloudWatch | ✅ Active |
| **Logging** | 로그 수집 및 분석 | CloudTrail / ELK Stack | ✅ Active |

---

## 🔄 배포 프로세스 흐름

```
┌─────────────────┐
│  코드 커밋 Push │
└────────┬────────┘
         │
         ▼
┌──────────────────────────┐
│ GitHub Webhook Trigger   │
│ (자동 감지)              │
└────────┬─────────────────┘
         │
         ▼
┌──────────────────────────┐
│ CI/CD 파이프라인 시작    │
│ (자동 빌드 & 테스트)     │
└────────┬─────────────────┘
         │
         ▼
┌──────────────────────────┐
│ 성공?                    │
└────────┬────────┬────────┘
         │        │
      YES│        │NO
         │        └──────→ 알림 발송 & 중단
         │
         ▼
┌──────────────────────────┐
│ AWS 배포 시작            │
│ (자동 배포)              │
└────────┬─────────────────┘
         │
         ▼
┌──────────────────────────┐
│ 헬스 체크 실행           │
│ (배포 상태 검증)         │
└────────┬─────────────────┘
         │
         ▼
┌──────────────────────────┐
│ 배포 완료 보고           │
│ (Slack / Email 알림)    │
└──────────────────────────┘
```

---

## 🛠️ 명령어 가이드

### 개발 환경

| 명령어 | 설명 | 실행 예 |
|--------|------|--------|
| `npm start` | 개발 서버 시작 | `npm start` |
| `npm run dev` | 핫 리로드 모드 | `npm run dev` |
| `npm test` | 단위 테스트 실행 | `npm test` |
| `npm run lint` | 코드 스타일 검사 | `npm run lint` |

### 배포 환경

| 명령어 | 설명 | 실행 예 |
|--------|------|--------|
| `npm run build` | 프로덕션 빌드 | `npm run build` |
| `npm run deploy` | AWS 배포 | `npm run deploy` |
| `npm run deploy:staging` | 스테이징 배포 | `npm run deploy:staging` |
| `npm run rollback` | 배포 롤백 | `npm run rollback` |
| `npm run status` | 배포 상태 확인 | `npm run status` |

---

## 📊 배포 환경별 설정

| 환경 | 용도 | AWS 서비스 | 모니터링 |
|------|------|-----------|----------|
| **Development** | 개발 및 테스트 | EC2 t3.micro | CloudWatch Lite |
| **Staging** | 사전 배포 테스트 | EC2 t3.small | CloudWatch |
| **Production** | 실제 운영 환경 | EC2 t3.medium + ALB | Full Monitoring |

---

## 🔐 보안 설정

### AWS 자격증명 관리

```bash
# AWS IAM 사용자 생성
aws iam create-user --user-name code-front-deploy

# 필요한 권한 정책 추가
aws iam attach-user-policy \
  --user-name code-front-deploy \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

# 액세스 키 생성
aws iam create-access-key --user-name code-front-deploy
```

### GitHub Secrets 설정

```
AWS_ACCESS_KEY_ID       → AWS IAM 액세스 키
AWS_SECRET_ACCESS_KEY   → AWS IAM 비밀 액세스 키
AWS_REGION              → ap-northeast-2
DEPLOYMENT_ROLE_ARN     → IAM Role ARN
```

---

## 📈 성능 최적화

| 최적화 항목 | 설명 | 효과 |
|-----------|------|------|
| **캐싱** | 빌드 캐시 활용 | 빌드 시간 60% 단축 |
| **병렬 처리** | 다중 작업 동시 실행 | 전체 배포 시간 40% 단축 |
| **리소스 풀링** | 리소스 재사용 | 비용 30% 절감 |
| **자동 스케일링** | 트래픽 기반 확장 | 안정적 성능 유지 |

---

## 🐛 문제 해결

### 배포 실패

```bash
# 로그 확인
npm run logs:deployment

# 상세 디버깅 활성화
DEBUG=* npm run deploy

# AWS 상태 확인
aws ec2 describe-instances
```

### 일반적인 오류 해결

| 오류 | 원인 | 해결책 |
|------|------|-------|
| `AWS credentials not found` | AWS 인증 미설정 | `aws configure` 실행 |
| `Permission denied` | IAM 권한 부족 | AWS IAM 정책 검토 |
| `Deployment timeout` | 배포 시간 초과 | 타임아웃 값 증가 또는 리소스 확대 |
| `Connection refused` | AWS 서비스 연결 실패 | 보안 그룹 및 방화벽 확인 |

---

## 📚 문서 및 리소스

- 📖 [자세한 설정 가이드](./docs/SETUP.md)
- 🔧 [API 문서](./docs/API.md)
- 🚀 [배포 가이드](./docs/DEPLOYMENT.md)
- 🛠️ [문제 해결 가이드](./docs/TROUBLESHOOTING.md)
- 📝 [변경 로그](./CHANGELOG.md)

---

## 🤝 기여 방법

### 코드 기여

```bash
# 1. Fork 및 Clone
git clone https://github.com/YOUR_USERNAME/code-front.git

# 2. Feature 브랜치 생성
git checkout -b feature/amazing-feature

# 3. 변경사항 커밋
git commit -m "Add amazing feature"

# 4. 브랜치 Push
git push origin feature/amazing-feature

# 5. Pull Request 생성
```

### 기여 가이드라인

- ✅ 코드 스타일 준수 (`npm run lint`)
- ✅ 테스트 작성 (`npm test`)
- ✅ 문서 업데이트
- ✅ 명확한 커밋 메시지 작성

---

## 📝 라이선스

이 프로젝트는 MIT 라이선스를 따릅니다. 자세한 내용은 [LICENSE](./LICENSE) 파일을 참고하세요.

---

## 👤 저자

- **junhyoung624** - 프로젝트 생성 및 관리

---

## 📞 지원

문제 또는 질문이 있으시면:

- 📧 Email: junhyoung624@example.com
- 💬 Issues: [GitHub Issues](https://github.com/junhyoung624/code-front/issues)
- 📱 Discussions: [GitHub Discussions](https://github.com/junhyoung624/code-front/discussions)

---

## 🙏 감사의 말

이 프로젝트는 다음 오픈소스 프로젝트의 영감을 받아 만들어졌습니다:

- [AWS CLI](https://aws.amazon.com/cli/)
- [GitHub Actions](https://github.com/features/actions)
- [Terraform](https://www.terraform.io/)

---

## 📅 프로젝트 상태

| 항목 | 상태 |
|------|------|
| 개발 | 🟢 진행 중 |
| 문서화 | 🟢 완료 |
| 테스트 커버리지 | 🟡 진행 중 |
| 프로덕션 배포 | 🟢 준비 완료 |

---

**마지막 업데이트**: 2026년 5월 18일
