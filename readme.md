# ⚽ 2026 FIFA World Cup Match Prediction

> 머신러닝을 활용한 2026 FIFA 북중미 월드컵 경기 결과 예측 프로젝트

---

## 📌 프로젝트 개요

1872~2026년 국제 축구 경기 데이터(45,797경기)를 기반으로  
2026 FIFA 월드컵 전 경기의 승/무/패 확률을 예측하는 머신러닝 모델을 구축합니다.

---

## 🎯 분석 목표

1. 2026 WC 전 경기 승/무/패 확률 예측
2. 시뮬레이션을 통한 우승팀 확률 분포 도출
3. 대한민국 조별리그 성적 및 토너먼트 진출 가능성 예측
4. 이변(Upset) 요인 분석
5. 역대 WC 우승팀 공통 feature 패턴 분석

---

## 🛠️ 기술 스택

![Python](https://img.shields.io/badge/Python-3.10-blue)
![XGBoost](https://img.shields.io/badge/XGBoost-2.0-orange)
![LightGBM](https://img.shields.io/badge/LightGBM-4.0-green)
![Optuna](https://img.shields.io/badge/Optuna-3.0-purple)
![SHAP](https://img.shields.io/badge/SHAP-0.44-red)

---

## 📁 폴더 구조

```
wc2026-match-prediction/
│
├── @전처리/
│   └── preprocessing.ipynb       # 피처 엔지니어링, 데이터 분할
│
├── @2026데이터/
│   └── wc2026_builder.ipynb      # 2026 WC 72경기 입력 데이터 생성
│
├── @최종/
│   ├── 01_eda.ipynb              # 탐색적 데이터 분석
│   ├── 02_baseline.ipynb         # Logistic Regression 베이스라인
│   ├── 03_xgboost.ipynb          # XGBoost + Optuna 튜닝
│   ├── 04_lightgbm.ipynb         # LightGBM + Optuna 튜닝
│   ├── 05_ensemble.ipynb         # Soft Voting / Stacking 앙상블
│   ├── 06_poisson.ipynb          # Poisson 회귀 (기대 득점 예측)
│   ├── 07_monte_carlo.ipynb      # 몬테카를로 시뮬레이션
│   └── 08_shap.ipynb             # SHAP 해석 분석
│
├── sandbox/                       # 개인 테스트 공간
│
├── .gitignore
└── README.md
```

---

## ⚙️ 주요 피처 (52개)

| 카테고리 | 피처 |
|---|---|
| Elo 레이팅 | `delta_elo`, `home_elo`, `away_elo` |
| 단기 폼 | `home_form`, `away_form` (EWMA, span=5) |
| 직전 4년 성적 | 득점·실점·승무패·득실차 등 18개 |
| WC 경험 | `wc_participations`, `wc_titles` |
| 경기 환경 | `neutral`, `is_wc`, `is_host` |
| Gap 피처 | 홈/원정 차이 7개 |
| 대륙 원핫 | AFC·CAF·CONCACAF·CONMEBOL·OFC·UEFA 12개 |

---

## 📊 모델 성능 비교

| 모델 | Accuracy | Log Loss | ROC-AUC |
|---|---|---|---|
| 베이스라인 (LR) | 62.6% | 0.8290 | 0.7695 |
| XGBoost + Optuna | 64.3% | 0.8026 | 0.7843 |
| LightGBM + Optuna | 64.5% | 0.8038 | 0.7841 |
| **Soft Voting (최종)** | **64.5%** | **0.8019** | **0.7847** |

---

## 🗂️ 데이터

데이터는 Google Drive로 관리합니다. (레포지토리에 포함되지 않음)

| 파일 | 설명 |
|---|---|
| `results_preprocessed.csv` | 전처리 완료 데이터 (45,797경기 × 55열) |
| `X_train.csv` / `X_test.csv` | 학습/테스트 피처 (split 기준: 2022-01-01) |
| `y_train.csv` / `y_test.csv` | 레이블 (H=2, D=1, A=0) |
| `w_train.csv` | 대회 가중치 (WC=1.0 ~ 친선=0.3) |
| `wc2026_matches.csv` | 2026 WC 72경기 입력 데이터 |

---

## 🔬 연구 가설

| 가설 | 내용 | EDA 결과 |
|---|---|---|
| H1 | ΔElo가 가장 강력한 단일 예측 변수 | ✅ 지지 (상관계수 0.581) |
| H2 | EWMA 폼이 4년 평균보다 예측력 높음 | ❌ 기각 (4년 승률 0.363 > 폼 0.292) |
| H4 | 이변 시 강팀 폼 저하·중립경기가 주요 원인 | ⚠️ 부분 지지 (중립경기 유의, 폼 비유의) |
| H5 | 우승팀은 공격보다 수비 지표가 더 강한 신호 | ✅ 지지 (수비 p=0.000 vs 공격 p=0.018) |

---

## 📅 대회 정보

- **일정**: 2026년 6월 11일 ~ 7월 19일
- **개최국**: 미국, 캐나다, 멕시코 (3국 공동)
- **참가국**: 48개국 / 12개 조
- **한국 조**: A조 (체코, 멕시코, 남아공)