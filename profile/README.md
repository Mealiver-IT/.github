# 🎫 Mealiver-IT (밀리버릿)

> 배달앱 오픈런 선착순 쿠폰 발급 시스템 — 재고 10,000장·동시요청 20,000건에도 초과발급 0건, 300만 건 발급이력에 대한 결정론적 정합성 자기검증

## 레포지토리

| 레포 | 역할 |
|---|---|
| [Mealiver-IT-BE](https://github.com/Mealiver-IT/Mealiver-IT-BE) | 백엔드 — 동시성 제어(V1~V4 버전 사다리), 정합성 검증 배치, 더미데이터 파이프라인 |
| [Mealiver-IT-FE](https://github.com/Mealiver-IT/Mealiver-IT-FE) | 프론트엔드 — 소비자 화면(이벤트/결제/쿠폰함) + 관리자 대시보드 |
| [Mealiver-IT-Infra](https://github.com/Mealiver-IT/Mealiver-IT-Infra) | 인프라 — MySQL/Redis/Prometheus/Grafana/Adminer/API/FE docker-compose |

각 레포의 세부 아키텍처·기술 스택·핵심 기능은 해당 레포 README를 참고하세요. 여기서는 세 레포를 함께 띄우는 순서만 다룹니다.

## 빠른 시작 (전체 시스템)

### 사전 요구사항

- Java 21, Node.js 18+, Docker

### 1) 인프라 (MySQL·Redis)

```bash
git clone https://github.com/Mealiver-IT/Mealiver-IT-Infra.git
cd Mealiver-IT-Infra
docker compose up -d mysql redis
```

### 2) 백엔드

```bash
git clone https://github.com/Mealiver-IT/Mealiver-IT-BE.git
cd Mealiver-IT-BE
./mvnw -o install -pl entity -DskipTests
./mvnw -o -f api/pom.xml spring-boot:run -Dspring-boot.run.profiles=local
```

`local` 프로필 기본 DB 접속 정보는 `api/src/main/resources/application-local.properties` 참고 — 1)에서 띄운 MySQL과 포트가 다르면 오버라이드하세요.

### 3) 프론트엔드

```bash
git clone https://github.com/Mealiver-IT/Mealiver-IT-FE.git
cd Mealiver-IT-FE
npm install
npm run dev
```

- 소비자 화면: http://localhost:5173
- 관리자 대시보드: http://localhost:5173/admin

### 4) 부하테스트 (선택)

```bash
cd Mealiver-IT-BE/api/src/test/K6/phase1
k6 run smoke-test.js
```

---

**Team 태진아**
