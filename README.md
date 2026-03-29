
# 🏛️ insurance-ai-governance

### 보험업권 지능형 언더라이팅의 AI 거버넌스 실증
**Empirical AI Governance for Intelligent Insurance Underwriting**

멀티에이전트 합의제 · 순환 정합성 검증 · GRI 기반 Human-on-the-Loop  
*Multi-Agent Consensus · Round-trip Fidelity Validation · GRI-based Human-on-the-Loop*

---

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Model](https://img.shields.io/badge/LLM-Qwen2.5--72B--AWQ-purple)](https://huggingface.co/Qwen/Qwen2.5-72B-Instruct-AWQ)
[![Inference](https://img.shields.io/badge/Inference-vLLM-green)](https://github.com/vllm-project/vllm)
[![Records](https://img.shields.io/badge/Records-10%2C058-orange)]()
[![Compliance](https://img.shields.io/badge/AI%20기본법-2026-red)]()

> 본 레퍼지토리는 아래 논문의 공식 코드 구현입니다.  
> This repository is the official implementation of the following paper.
>
> **"보험업권 지능형 언더라이팅의 AI 거버넌스 실증: 멀티에이전트 합의제, 순환 정합성 검증 및 GRI 기반 Human-on-the-Loop 설계"**  
> *"Empirical AI Governance for Intelligent Insurance Underwriting: Multi-Agent Consensus, Round-trip Fidelity Validation, and GRI-based Human-on-the-Loop Design"*  
> *(보험학회지 투고 / Submitted to Korean Insurance Journal)*

---

## 📌 개요 | Overview

본 연구는 2026년 시행된 **「인공지능 기본법」** 및 **금융감독원 AI 활용 가이드라인**이 보험업권에 부과하는 세 가지 핵심 의무 — ① AI 판단의 설명가능성, ② 데이터 무결성 검증, ③ 인간 감독 체계 — 를 실무적으로 충족하는 **Sf–Cr–GRI 3축 거버넌스 프레임워크**를 최초로 대규모 실증한 연구입니다.

This study presents the first large-scale empirical demonstration of the **Sf–Cr–GRI triaxial governance framework**, designed to satisfy the three core obligations imposed by South Korea's **AI Basic Act (2026)** and **FSS AI Guidelines**: ① AI explainability, ② data integrity verification, and ③ human oversight.

외부 클라우드 API를 일절 사용하지 않고, 금융권 망 분리 원칙을 준수하는 **온프레미스 Private LLM(Qwen 2.5-72B, AWQ 4-bit)** 환경에서 **10,058건**의 건강검진 데이터를 대상으로 9단계 실증 실험을 수행하였습니다.

All experiments are conducted entirely on-premise using a **Private LLM (Qwen 2.5-72B, AWQ 4-bit)** with no external API calls, across **10,058 health examination records** in a nine-stage pipeline.

---

## 🔑 핵심 결과 | Key Results

| 축 / Axis | 지표 / Metric | 결과 / Result |
|---|---|---|
| ① 데이터 무결성 / Data Integrity | 평균 Sf / Mean Sf Score | **0.9956** (SD = 0.0377) |
| ① 데이터 무결성 / Data Integrity | 실질적 무결성 / Substantial integrity | **90.4%** of records |
| ① 데이터 무결성 / Data Integrity | 오류 귀인 / Error attribution | **87.9%** extraction-stage |
| ② 판정 불확실성 / Uncertainty | 전원 합의 / Full consensus (Cr = 1.0) | **83.3%** (8,375 cases) |
| ② 판정 불확실성 / Uncertainty | 룰-AI 일치율 / Rule–AI agreement | 72.5% (κ = 0.456) |
| ② 판정 불확실성 / Uncertainty | AI 더 엄격 / AI stricter (of disagreements) | **74.1%** |
| ③ 거버넌스 자동화 / Governance | 완전 자동화 / Auto-approved (L1) | **63.9%** |
| ③ 거버넌스 자동화 / Governance | HITL 대상 / HITL required (L3+L4) | **16.7%** |
| 설명가능성 / Explainability | 유의 키워드 카테고리 / Significant categories | **10 / 11** (p < .05) |
| 설명가능성 / Explainability | 최강 상관 / Strongest correlation | Blood pressure r = **+0.610** |

---

## 🗂️ 레퍼지토리 구조 | Repository Structure

```
insurance-ai-governance/
│
├── 📓 1_합성소견서생성_공개ver.ipynb
│       합성 소견서 생성 파이프라인
│       Synthetic medical report generation pipeline
│       → D_orig (정형 데이터) → S_synth (비정형 소견서)
│       → Structured health data → Synthetic medical reports
│
├── 📓 2_합성소견서_무결성체크_공개ver.ipynb
│       재추출 · 순환 정합성 검증 · 환각 분류 · 오류 귀인
│       Re-extraction · Round-trip Fidelity · Hallucination classification · Error attribution
│       → S_synth → D_recon → Sf Score → 환각 유형 분류
│       → S_synth → D_recon → Sf Score → Hallucination type classification
│
├── 📓 3_지능형언더라이팅실증_공개ver.ipynb
│       멀티에이전트 심사 · 룰베이스 비교 · GRI · HITL · 시각화 전체
│       Multi-agent underwriting · Rule-based comparison · GRI · HITL · Full visualization
│       → 델파이 합의제 AI 심사 → 판정 간극 분석 → 키워드 상관 → GRI → 감사 등급
│       → Delphi consensus AI review → Gap analysis → Keyword correlation → GRI → Audit levels
│
├── 📁 data/
│   └── README_data.md          # 데이터 접근 방법 / Data access instructions
│
├── requirements.txt
├── LICENSE
└── README.md
```

### 노트북-실험단계 매핑 | Notebook–Pipeline Stage Mapping

| 노트북 / Notebook | 실험 단계 / Pipeline Stage | 핵심 산출물 / Key Output |
|---|---|---|
| `1_합성소견서생성_공개ver.ipynb` | 단계 1: 합성 소견서 생성 | `s_synth.jsonl` |
| `2_합성소견서_무결성체크_공개ver.ipynb` | 단계 2–4: 재추출 · Sf 산출 · 환각 분류 · 오류 귀인 | `d_recon.jsonl` · `sf_scores.csv` · `hallucination_report.csv` |
| `3_지능형언더라이팅실증_공개ver.ipynb` | 단계 5–9: 룰베이스 · AI 심사 · 간극 분석 · XAI · GRI · HITL | `rule_adjudication.csv` · `ai_adjudication.csv` · `gri_scores.csv` · `audit_levels.csv` |

---

## ⚙️ 시스템 요구사항 | System Requirements

### 하드웨어 / Hardware

| 구성 요소 | 사양 |
|---|---|
| GPU | NVIDIA A100 80GB PCIe |
| 유효 VRAM / Effective VRAM | 40 GB (MIG 3g·40GB 파티션) |
| CPU | 8 vCore |
| RAM | 96 GiB |

> AWQ 4-bit 양자화 적용으로 단일 GPU(40GB)에서 72B 모델 구동이 가능합니다.  
> AWQ 4-bit quantization enables 72B model inference on a single 40 GB GPU partition.

### 소프트웨어 / Software

```
Python >= 3.10
vLLM >= 0.4.0
transformers >= 4.40.0
pandas >= 2.0
numpy >= 1.24
scipy >= 1.11
scikit-learn >= 1.3
pingouin >= 0.5        # ICC(3,1) 산출 / ICC computation
matplotlib >= 3.7
seaborn >= 0.12
pyyaml >= 6.0
tqdm >= 4.65
jupyter >= 1.0
```

```bash
pip install -r requirements.txt
```

---

## 🤖 모델 | Model

| 항목 | 상세 |
|---|---|
| 기반 모델 / Base model | [Qwen2.5-72B-Instruct](https://huggingface.co/Qwen/Qwen2.5-72B-Instruct) |
| 양자화 / Quantization | AWQ 4-bit → [Qwen2.5-72B-Instruct-AWQ](https://huggingface.co/Qwen/Qwen2.5-72B-Instruct-AWQ) |
| 추론 엔진 / Inference engine | [vLLM](https://github.com/vllm-project/vllm) with PagedAttention |
| Temperature | `0` (결정론적 출력 / Deterministic output) |
| 배포 방식 / Deployment | **온프레미스 전용 / On-premise only — no external API** |

vLLM 서버 실행 후 노트북을 순서대로 실행하세요.  
Launch the vLLM server first, then run the notebooks in order.

```bash
python -m vllm.entrypoints.openai.api_server \
    --model Qwen/Qwen2.5-72B-Instruct-AWQ \
    --quantization awq \
    --gpu-memory-utilization 0.95 \
    --max-model-len 4096
```

---

## 📊 데이터 | Data

본 연구는 **국민건강영양조사(KNHANES)** 공공데이터를 사용합니다.  
This study uses the **Korea National Health and Nutrition Examination Survey (KNHANES)** public dataset.

| 항목 | 상세 |
|---|---|
| 출처 / Source | 질병관리청(KDCA) / Korea Disease Control and Prevention Agency |
| 공식 다운로드 / Download | [https://knhanes.kdca.go.kr](https://knhanes.kdca.go.kr) |
| 사용 레코드 수 / Records | 10,058건 |
| 변수 수 / Variables | 24개 (연속형 10개 + 범주형 14개) |
| 원본 데이터 포함 여부 / Raw data | ❌ 미포함 — 위 링크에서 직접 다운로드 필요 |

### 데이터 준비 / Data Preparation

KNHANES 다운로드 후, 전처리된 파일을 아래 경로에 위치시키세요.  
After downloading, place the processed file at:

```
data/knhanes_processed.csv
```

컬럼 스키마 및 전처리 방법은 [`data/README_data.md`](data/README_data.md)를 참조하세요.  
Column schema and preprocessing steps are documented in [`data/README_data.md`](data/README_data.md).

---

## 🚀 실행 순서 | How to Run

노트북을 번호 순서대로 실행하면 9단계 전체 파이프라인이 완성됩니다.  
Run the notebooks in order to reproduce the full nine-stage pipeline.

```
Step 1  →  1_합성소견서생성_공개ver.ipynb
           합성 소견서 S_synth 생성
           Generate synthetic medical reports S_synth

Step 2  →  2_합성소견서_무결성체크_공개ver.ipynb
           재추출(D_recon) · Sf 산출 · 환각 유형 분류 · 오류 귀인 분석
           Re-extraction · Sf computation · Hallucination classification · Error attribution

Step 3  →  3_지능형언더라이팅실증_공개ver.ipynb
           룰베이스 심사 · 멀티에이전트 AI 심사 · 판정 간극 분석
           키워드-위험점수 상관(XAI) · GRI 산출 · 4단계 감사 분류 · 시각화
           Rule-based & multi-agent AI underwriting · Gap analysis
           Keyword–risk correlation (XAI) · GRI · Audit classification · Visualization
```

각 노트북은 독립적으로 실행 가능하며, 이전 단계의 출력 파일을 `data/` 폴더에 저장한 뒤 다음 노트북에서 불러오는 방식으로 연결됩니다.  
Each notebook can be run independently. Outputs from each stage are saved to the `data/` folder and loaded by the next notebook.

---

## 📐 핵심 수식 | Key Formulas

### 순환 정합성 지표 (Sf) / Round-trip Fidelity Score

$$S_f = \sum_{i=1}^{n} w_i \cdot \mathbb{1}(D_{\text{orig},i} \approx D_{\text{recon},i})$$

- $w_i$: 2-round 델파이 시뮬레이션으로 도출한 임상 지표 가중치  
  Clinical importance weight derived from 2-round Delphi simulation
- $\mathbb{1}(\cdot)$: 변수별 임상 허용 오차 $\tau_i$ 기반 지표 함수  
  Indicator function based on variable-specific clinical tolerance $\tau_i$

### 복합 리스크 지수 (GRI) / Governance Risk Index

$$\text{GRI} = 0.4 \cdot (1 - S_f) + 0.4 \cdot (1 - C_r) + 0.2 \cdot \text{ConflictPenalty}$$

- **ConflictPenalty**: 룰-AI 판정 등급 차이 × 0.5 (최대 1.0)  
  Grade gap between rule-based and AI adjudication × 0.5 (max 1.0)
- $C_r$: 5인 에이전트 합의도 = 최빈 판정 동의 수 / 5  
  Agent consensus rate = count of agents agreeing on modal verdict / 5

### 4단계 감사 등급 / 4-Level Audit Classification

| 등급 / Level | 조건 / Condition | 인간 개입 / Human Intervention | 비율 / Rate |
|---|---|---|---|
| **L1** Auto-Approve | GRI ≤ 0.01 AND Cr = 1.0 | 없음 / None | 63.9% |
| **L2** Sampling Audit | Cr = 1.0 AND GRI > 0.01 | 사후 샘플링 / Post-hoc sampling | 19.4% |
| **L3** Human Review | Cr < 1.0, GRI ≤ P95 | 검토 권고 / Review recommended | 7.7% |
| **L4** HITL Required | Cr < 1.0 AND GRI > P95 | 승인 필수 / Approval mandatory | 9.0% |

---

## 👥 멀티에이전트 구성 | Multi-Agent Design

| 에이전트 / Agent | 심사 중점 / Focus | 주요 레퍼런스 / Reference |
|---|---|---|
| 수석 언더라이터 / Chief Underwriter | 인수 정책, 역선택 리스크 | FSC (2021); Cummins & Weiss (2009) |
| 임상 전문의 / Clinical Physician | 의학적 인과관계, 합병증 | Singhal et al. (2023); Yusuf et al. (2004) |
| 보험 계리사 / Actuary | 통계적 위험률, 기대 손실액 | Klugman et al. (2012); Frees et al. (2014) |
| 예방의학 전문가 / Preventive Medicine | 생활습관 장기 영향 | WHO (2022); Khaw et al. (2008) |
| 준법 감시인 / Compliance Officer | 공정성, 설명가능성, 소비자 보호 | Jobin et al. (2019); EU AI Act (2024) |

에이전트별 시스템 프롬프트 전문은 `3_지능형_언더라이팅_실증_공개ver.ipynb` 내부를 참조하세요.  
Full system prompt definitions are included in `3_지능형_언더라이팅_실증_공개ver.ipynb`.

---

## 🔒 보안 및 규제 준수 | Security & Regulatory Compliance

| 항목 / Item | 상태 / Status |
|---|---|
| 외부 API 미사용 / No external API calls | ✅ |
| 금융권 망 분리 원칙 준수 / Network-separation compliance | ✅ |
| 「인공지능 기본법」(2026) 고위험 AI 요건 / AI Basic Act | ✅ |
| 금융감독원 AI 활용 가이드라인 / FSS AI Guidelines | ✅ |
| EU AI Act (2024) 고위험 AI 분류 / High-risk AI obligations | ✅ |
| NAIC AI Model Bulletin (2024) | ✅ |

---

## 📄 인용 | Citation

본 코드 또는 프레임워크를 연구에 활용하실 경우 아래와 같이 인용해 주세요.  
If you use this code or framework, please cite:

---

## 📬 문의 | Contact

코드 또는 방법론 관련 문의는 [GitHub Issue](../../issues)를 통해 남겨주세요.  
For questions regarding the code or methodology, please open a [GitHub Issue](../../issues).

---

## 📜 라이선스 | License

본 프로젝트는 **MIT 라이선스** 하에 배포됩니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.  
This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.

> **데이터 이용 안내 / Data Notice**  
> KNHANES 데이터셋은 질병관리청 자체 이용 약관의 적용을 받습니다.  
> The KNHANES dataset is subject to KDCA's own terms of use.  
> 접근 및 이용 조건은 [KDCA 데이터 포털](https://knhanes.kdca.go.kr)을 참조하세요.  
> Refer to the [KDCA data portal](https://knhanes.kdca.go.kr) for access and usage conditions.
