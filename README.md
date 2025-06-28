# 📊 가상 데이터 리포지토리 기반 서비스 분석 및 추천 시스템

Streamlit으로 구현된 이 프로젝트는 2년간의 온라인 광고 및 오프라인 이벤트 데이터를 바탕으로,  
마케팅 성과를 분석하고 사용자 행동을 예측하는 데이터 기반 시각화 및 머신러닝 프로젝트입니다.

🔗 **배포 링크**: [Streamlit 웹 앱 바로가기](https://apprjgroup3-dtiyavdpz8ywuhdu6nhint.Streamlit.app/)

---

## 📅 프로젝트 개요

- **개발 기간**: 2025년 3월 14일 ~ 2025년 3월 27일  
- **배포일**: 2025년 3월 27일  
- **가상 서비스 운영 기간**: 2023년 1월 ~ 2024년 12월 (2년간)

---

## ✅ 업데이트 내역

### 📌 [2025.03.15 수정 사항]
- 온라인 광고 페이지에 웹사이트 유입 관련 지표 추가 (체류 시간, 페이지뷰 등)
- 유입 경로 항목 확장: 이메일, 카카오톡, 메타, 블로그, 카페, 인스타그램 등 총 12개로 구성
- ‘키워드 광고’를 통한 유입일 경우에만 **대표 키워드** 정보 표기
- 전환 항목을 **회원가입 / 앱 다운로드 / 뉴스레터 구독**으로 구분
- 전환율 = 각 항목별 유입 대비 전환 비율로 정확히 계산
- 전체 온라인 데이터 Row 수가 약 **24,000건**으로 증가

### 📌 [2025.03.20 수정 사항]
- 전환 수가 총 유입 수보다 많아지지 않도록 데이터 정합성 보정
- (전환 수 ≤ 유입 수) 조건 보장

---

## 🖥️ 웹 애플리케이션 구조

| 페이지명        | 설명 |
|-----------------|------|
| **Home**        | 프로젝트 소개 및 페이지 안내 역할을 수행하는 메인 화면 |
| **Summary**     | 온라인 + 오프라인 데이터를 종합적으로 보여주는 대시보드 (필터 없음) |
| **Offline**     | 날짜, 요일, 지역 필터링을 통한 오프라인 활동 데이터 시각화 |
| **Online**      | 유입 경로, 키워드, 디바이스 필터링 기반 온라인 광고 성과 분석 |
| **Pred Model**  | 머신러닝 모델을 활용한 사용자 예측 및 추천 기능 제공 |

---

## 🌐 온라인 광고 분석 기능

### 1. 💬 키워드 광고 운영 지표
- 대표 키워드 10개를 기준으로 키워드 광고(Naver 등) 운영
- 광고 성과 지표:
  - 노출수
  - 클릭 수
  - 클릭률 (CTR)
  - 클릭당 비용 (CPC)
  - 총 광고비
  - 전환 수
  - 전환율

### 2. 📲 유입 정보
- **유입 기기**: PC, Mobile
- **유입 경로**:
  - 직접 유입, 키워드 검색, 블로그, 카페
  - 이메일, 카카오톡, Meta, Instagram, YouTube
  - 배너 광고, Twitter(X), 기타 SNS

> 키워드 정보는 "키워드 검색" 유입일 경우에만 기록됩니다.

### 3. ⏱ 체류 및 페이지뷰 지표
- 전체 체류 시간
- 유입당 평균 체류 시간
- 전체 페이지뷰 수
- 유입당 평균 페이지뷰 수

### 4. ✅ 전환 정보
- 전환 항목:
  - 회원가입
  - 앱 다운로드
  - 뉴스레터 또는 알림 설정 구독
- 전환율:
  - 각 전환 항목에 대한 **유입 대비 전환 비율** 계산

---

## 🏢 오프라인 이벤트 데이터 구성

### 1. 📍 캠페인 개요
- 전국 일부 주요 지역에서 **2년간 캠페인 개최**를 가정

### 2. 📝 수집 지표
- 지역명
- 방문자 수
- 연령대, 성별
- 이벤트 종류 (예: 설명회, 체험존 등)
- 참여자 수, 참여율

> (유입 수 ≥ 전환 수)를 만족하도록 데이터 생성 로직 수정됨

---

## 👤 회원 데이터 기반 분석

오프라인 캠페인 참가자 중 무작위로 추출된 **1,000명 회원** 데이터를 활용하여 머신러닝 모델을 구현합니다.

### 1. 🎯 신규 서비스 구독 예측
- 입력: 나이, 성별, 결혼 여부
- 출력: 구독 여부 예측 (Classification)

### 2. 💡 캠페인 추천 모델
- 입력 조건: 나이, 성별, 도시, 결혼 여부
- 출력: 가장 구독률이 높은 캠페인 추천

### 3. 🌐 온라인 채널 추천
- 조건: 나이, 성별, 결혼 여부
- 출력: 전환율이 가장 높은 상위 3개 채널 추천

---

## 🤖 머신러닝 알고리즘

```python
from sklearn.model_selection import train_test_split, GridSearchCV, StratifiedKFold, cross_val_score
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier, VotingClassifier, RandomForestRegressor
from sklearn.linear_model import LogisticRegression, LinearRegression
from sklearn.metrics import (
    accuracy_score, confusion_matrix, precision_score, recall_score, f1_score,
    roc_curve, roc_auc_score, mean_squared_error, mean_absolute_error, r2_score,
    classification_report
)
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
```

---

## 🧰 시각화 및 데이터 처리 도구
```python
import pandas as pd
import numpy as np
import datetime
import time
import seaborn as sns
import matplotlib.pyplot as plt
import plotly.express as px
import plotly.graph_objs as go
import folium
from random import sample
```

---

## 💡 기대 효과
가상의 온라인/오프라인 통합 데이터를 통해 실무형 분석 로직 경험

다양한 조건 기반 예측/추천 모델링 실습

마케팅 실무에서 활용 가능한 데이터 기반 전략 설계 능력 강화

Streamlit 기반 다중 페이지 대시보드 구성 능력 향상

---

## ⚠️ 참고 사항
본 프로젝트의 데이터는 모두 가상의 시뮬레이션 데이터입니다.

실제 기업/서비스와 무관하며, 교육 및 실습 목적으로 제작되었습니다.

---

## 📜 라이선스 (License)

이 프로젝트는 [MIT License](LICENSE) 를 따릅니다.
