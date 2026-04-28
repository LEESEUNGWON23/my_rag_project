# RAG 기반 AI 지식 검색 시스템 — 포트폴리오

> 기밀 유지를 위해 회사명 및 제품명은 표기하지 않습니다.  
> 모든 코드 스니펫은 실제 프로덕션에서 제가 작성한 코드의 일부입니다.

**기간:** 2026년 3월 ~ 4월  
**역할:** AI 백엔드 엔지니어 — RAG 파이프라인 및 LLM 백엔드 단독 개발  
**스택:** Python · FastAPI · LangChain · Qdrant · vLLM · Docling · asyncio

---

## 시스템 개요

기업용 문서를 수집·임베딩하여 벡터 DB에 저장하고, 의미 기반 하이브리드 검색과 재랭킹을 거쳐 LLM 답변을 생성하는 **RAG 파이프라인을 처음부터 설계·구현**했습니다.

```
[ 문서 업로드 ]
      │
      ▼
[ 파일 파싱 ] ── 15가지 이상 포맷 (LangChain / Docling)
      │
      ▼
[ 임베딩 ] ── Dense + Sparse 벡터 (vLLM 원격 서빙)
      │
      ▼
[ Qdrant 벡터 DB ] ── prd / dev 컬렉션 분리
      │
      ▼
[ 하이브리드 검색 ] ── RRF 융합 (병렬 인코딩)
      │
      ▼
[ 재랭커 ] ── vLLM 비동기 배치 병렬 처리
      │
      ▼
[ LLM 답변 생성 ] ── 스트리밍 SSE / 컨텍스트 압축 / 사용자 메모리
```

---

## 프로젝트 구성

| 저장소 | 역할 | 커밋 수 |
|--------|------|---------|
| [api-rag →](rag/README.md) | 문서 수집, 임베딩, 하이브리드 검색, 재랭킹 | 64+ |
| [be-fastapi →](fastapi/README.md) | 채팅 API, LLM 라우팅, 대화 컨텍스트 관리 | 31+ |

---

## 기술 스택

| 분류 | 기술 |
|------|------|
| 언어 / 프레임워크 | Python 3.12, FastAPI |
| 벡터 DB | Qdrant (Dense + Sparse 분리 컬렉션) |
| LLM 서빙 | vLLM (임베딩, 재랭킹, 생성 모두 원격 API) |
| 문서 파싱 | LangChain, Docling, OpenDataLoaderPDF |
| 비동기 처리 | asyncio, aiohttp, ThreadPoolExecutor |
| 코드 파싱 | LangChain LanguageParser (tree-sitter 기반) |
