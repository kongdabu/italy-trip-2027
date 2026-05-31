# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

n8n 워크플로우 자동화 플랫폼을 로컬 Docker 환경에서 운영하고, Claude Code의 n8n MCP를 통해 워크플로우를 생성·관리하는 프로젝트.

## 환경 실행 명령어

```bash
# n8n 컨테이너 시작
docker compose up -d

# n8n 컨테이너 중지
docker compose down

# 로그 확인
docker compose logs -f n8n

# 컨테이너 상태 확인
docker compose ps
```

## 접속 정보

- n8n UI: http://localhost:5679
- API 엔드포인트: http://localhost:5679/api/v1
- API Key: `n8n/.env` 파일의 `N8N_API_KEY` 참조
- 타임존: Asia/Seoul

## 아키텍처

- **인프라**: Docker Compose로 n8n 단일 컨테이너 운영
- **데이터 영속성**: `n8n_data` Docker named volume (`/home/node/.n8n`)
- **MCP 연동**: Claude Code의 `n8n-mcp` 도구로 워크플로우 생성·실행·관리
- **웹훅 URL**: `http://localhost:5679/` (로컬 전용)

## n8n MCP 도구 사용 패턴

워크플로우 작업 시 다음 순서로 MCP 도구를 활용한다:

1. `n8n_health_check` — 연결 확인
2. `n8n_list_workflows` — 기존 워크플로우 목록 조회
3. `n8n_generate_workflow` / `n8n_create_workflow` — 신규 워크플로우 생성
4. `n8n_validate_workflow` — 배포 전 검증
5. `n8n_test_workflow` — 실행 테스트
6. `n8n_executions` — 실행 이력 확인

## 주의 사항

- `n8n/.env`에 API Key가 평문으로 저장되어 있으므로 외부 저장소에 푸시하지 않는다.
- 웹훅 URL이 localhost로 고정되어 있어 외부에서 트리거할 수 없다. 외부 연동 시 ngrok 등 터널링 필요.
- 데이터는 Docker volume에만 저장되므로 `docker compose down -v` 실행 시 모든 워크플로우가 삭제된다.
