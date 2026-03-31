# 특허 심사 대응 AI 툴 (SA-2)

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python)
![uv](https://img.shields.io/badge/package_manager-uv-purple)
![Model(tool1, 2)](https://img.shields.io/badge/LLM-DeepSeek_v3-green)


KIPRIS Plus API로 특허 데이터를 수집하고, PDF를 파싱해 구조화된 JSON을 만든 뒤,
특허 심사 대응에 필요한 5개 AI 툴의 학습/평가 데이터셋을 구축하는 프로젝트입니다.

---

## Table of Contents

- [전체 파이프라인](#전체-파이프라인)
- [디렉토리 구조](#디렉토리-구조)
- [설치 및 실행](#설치-및-실행)
- [파싱 모듈](#파싱-모듈)
- [툴 현황](#툴-현황)
- [협업 가이드](#협업-가이드)

---

## 전체 파이프라인

```mermaid
flowchart TD
    A[KIPRIS Plus API] --> B1[특허 공보 PDF]
    A --> B2[의견제출통지서 PDF]
    A --> B3[인용특허 PDF 한/일]
    A --> B4[보정서 JSON]

    B1 --> C1[patent_extract.py]
    B2 --> C2[extract_v6.py]
    B3 --> C3[jp_extract.py]
    B4 --> C4[그대로 활용]

    C1 --> D[구조화 JSON]
    C2 --> D
    C3 --> D
    C4 --> D

    D --> E1[Tool1 데이터셋]
    D --> E2[Tool2 데이터셋]
    D --> E3[Tool3 데이터셋]
    D --> E4[Tool4 데이터셋]
    D --> E5[Tool5 데이터셋]

    E1 --> F{성능 평가}
    E2 --> F
    E3 --> F
    E4 --> F
    E5 --> F

    F -->|목표 달성| G[완료]
    F -->|한계 확인| H[LoRA 파인튜닝]
    H --> F
```

---

## 디렉토리 구조

```
.
├── patent_parsing/       특허 공보 PDF 파싱
│   └── patent_extract.py
├── OA_parsing/           의견제출통지서 PDF 파싱
│   └── extract_v6.py
├── JP_Cited_Patents/     일본 인용특허 PDF 파싱
│   └── jp_extract.py
├── AMD_parsing/          보정서 파싱
│   └── amd_extract.py
├── Figure_parsing/       도면 파싱
│   └── figure_extract.py
├── kipris/               KIPRIS API 데이터 수집
├── sa2_tool1.py          Tool1 구현체 (거절이유 분석)
├── sa2_tool2.py          Tool2 구현체 (청구항 파싱)
├── docs/                 툴별 데이터셋 기획 문서
└── _archive/             사용 종료 파일 보관
```

---

## 설치 및 실행

**사전 요구사항:** [uv](https://docs.astral.sh/uv/) 설치

```bash
git clone <repo-url>
cd pdf-ex

# 의존성 설치
uv sync

# 환경변수 설정
cp .env.example .env
# .env 파일에 OPENROUTER_API_KEY 입력
```

---

## 파싱 모듈

| 모듈 | 입력 | 출력 |
|---|---|---|
| `patent_parsing/patent_extract.py` | 특허 공보 PDF | `patent_parsed.json` |
| `OA_parsing/extract_v6.py` | 의견제출통지서 PDF | `oa_parsed.json` |
| `JP_Cited_Patents/jp_extract.py` | 일본 인용특허 PDF | `jp_parsed.json` |
| `AMD_parsing/amd_extract.py` | 보정서 JSON | 구조화 JSON |
| `Figure_parsing/figure_extract.py` | 특허 도면 PDF | 도면 파싱 결과 |

---

## 툴 현황

| 툴 | 기능 | 평가 방식 | 문서 |
|---|---|---|---|
| Tool1 | 거절이유 분석 | 자동 (F1, Accuracy, LLM judge) | [📄 개요](docs/tool1데이터셋개요.md) |
| Tool2 | 청구항 파싱 | 자동 (F1, LLM judge) | [📄 개요](docs/tool2데이터셋개요.md) |
| Tool3 | 클레임 차트 (O/X) | 자동 (O/X 분류) | [📄 개요](docs/tool3데이터셋개요.md) |
| Tool4 | 차이점·대응전략 | 사람 검수 필요 | [📄 개요](docs/tool4데이터셋개요.md) |
| Tool5 | 보정안 생성 | 사람 검수 필요 | [📄 개요](docs/tool5데이터셋개요.md) |

성능 개선 전략: **Phase 1** DSPy 프롬프트 최적화 → 한계 확인 시 **Phase 2** LoRA 파인튜닝

---

## 협업 가이드

자세한 내용은 [CONTRIBUTING.md](docs/CONTRIBUTING.md)를 참고하세요.
