# 🎮 LoL MMR 게임 영향력 모델링 프로젝트

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=R&logoColor=white"/>
  <img src="https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=MySQL&logoColor=white"/>
  <img src="https://img.shields.io/badge/LoL_API-5C2D91?style=for-the-badge&logo=riot-games&logoColor=white"/>
</p>


> **“단순한 승패가 아닌, 진짜 ‘기여도’를 측정하자.”**

**‘리그 오브 레전드(LoL) 영향력 기반 MMR 시스템 분석’** 프로젝트는 실제 게임 내 퍼포먼스를 반영한 **정량적 MMR 시스템을 설계**하는 것을 목표로 합니다.
Discord 봇을 활용해 전적 데이터를 수집하고, 포지션별 주요 지표를 도출한 뒤, RandomForest 기반 분석으로 기여도를 수치화하여 **Game Impact Score**를 개발했습니다.

* 포지션별 특성을 반영한 **가중치 설정**
* 상대방과 비교한 **상대 영향력 지표(impact\_vs\_str)** 생성
* 기여도에 따른 **MMR 변화량 계산 로직 설계**
* 치트/스머프 사용자 탐지 및 대회 선발 기준 제시 가능

해당 분석을 통해 단순한 승패가 아닌, **진정한 팀 기여도와 공정한 실력 평가 체계**를 구축할 수 있습니다.

---

이 프로젝트는 단순한 승패 결과가 아닌, 실제 **게임 내 퍼포먼스**를 기반으로 한 리그 오브 레전드(LoL)의 맞춤형 MMR(Matchmaking Rating) 시스템을 구축하는 것을 목표로 합니다. 상세한 전적 데이터를 분석하여 각 플레이어의 영향력을 수치화하고, 이를 기반으로 부정 행위자 탐지, 공정한 매칭, 대회 선발 등에 활용할 수 있는 정교한 점수 체계를 설계합니다.

---
## 🎯 목표

✅ 단순 승패가 아닌 **기여도 기반 평가**로 더 정교한 MMR 점수 체계 제공
✅ **치트/스머프 사용자 탐지**로 매칭 공정성 향상
✅ **팀 구성의 질적 향상**을 위한 평가 시스템 구축

---

## 📂 데이터 및 도구

* **데이터 출처**: 공개된 LoL 전적 로그 + 자체 라벨링 데이터
* **주요 변수**: 킬, 어시스트, 딜량, 골드, 시야 점수, 오브젝트 기여도 등
* **사용 도구**:
  `Python`, `R`, `SQL`, `pandas`, `scikit-learn`, `seaborn`, `matplotlib`
<div align="Center">
  <img width="600" height="330" alt="Civil Bot Table" src="https://github.com/user-attachments/assets/324d5d1c-648d-435e-a37c-4709414d94f6" style="margin-right: 20px; display: inline-block; vertical-align: middle;" />
  <div style="display: inline-block; text-align: left; max-width: 500px; vertical-align: middle;">
    <h4>🧮 Civil War Bot 정규화 테이블</h4>
    <p>각 포지션별 주요 변수들(KDA, 오브젝트 기여 등)의 분포를 정규화하여<br>
    영향력 지수(Game Impact Score) 산출의 기준 테이블로 사용합니다.<br>
    이를 통해 포지션 차이에 따른 수치 왜곡 없이 정량 비교가 가능합니다.</p>
  </div>
</div>

---

## 📊 분석 방법론

1. **역할별 지표 가중치 부여**: 포지션 별 주요 능력치로 볼 수 있는 지표 선정을 통한 MMR 계산 시 가중치 설정 로직
  - 각 포지션은 다른 스탯(예: 서포터의 시야 점수, 원거리 딜러의 골드 효율, 정글러의 오브젝트 컨트롤)을 강조합니다.
  - 가중치가 적용된 지표는 라인과 관계없이 공정한 영향력 평가를 보장합니다.
2. **랜덤 포레스트 기반 영향력 모델링**: 특징 중요도를 도출하고 게임 영향력(GAME_IMPACT) 점수 모델을 구축하기 위해 랜덤 포레스트 분류기(RandomForestClassifier)를 활용.
  - 모델의 장점 :
    - 범주형 및 연속형 변수 모두 처리 가능
    - 과적합 완화
    - 경기 데이터의 복잡하고 비선형적인 패턴 파악
    
3. **도메인 가중치 할**: 지표는 역할 상황 및 게임 상태(예: 초반 vs 후반)에 따라 동적인 가중치를 받습니다.
  - ex . **서포터의 경우 ‘시야 점수’ 지표에 대한 가중치를 다른 포지션보다 2.5배 설정**)
5. **상대적 영향력 분석 (IMPACT_VS_STR)**:
    - 플레이어의 기여도를 자신의 라인 상대 또는 역할 동료와 비교하여 계산합니다.
    - 라인 우위, 서포터 효율성 또는 정글러 템포 컨트롤을 식별하는 데 도움이 됩니다.
    - 동일한 경기 환경 내에서 나란히 벤치마킹이 가능합니다.
6. **MMR 조정 로직 설계**: 기여도 점수는 경기 후 MMR 업데이트를 결정합니다.


| 게임 결과 | 기여도 수준 | MMR 변동 |
|---|---|---|
| 🟥 패배 | 높음 | ⬆️ 상승 |
| 🟩 승리 | 낮음 | ⬇️ 하락 |
| 무관 | 보통 | ➖ 변화 없음 |

* ⬆️ 패배했지만 활약도가 높은 경우 → 가산점
* ⬇️ 승리했지만 기여도가 낮은 경우 → 감점
* ➖ 평균적인 기여도 → 점수 유지

---

## 🔍 주요 특징

✨ **포지션별 정규화**로 공정한 비교 가능
✨ **스코어 0\~100 정량화**로 직관적인 영향력 파악
✨ **이상 사용자(치터/스머프)** 탐지 기능 내장
✨ **시각화 기반 분석 리포트** 동봉 (PDF 포함)

---

## 📁 프로젝트 구조

```
MMR_project/
│
├── data/               # 전처리된 매치 로그 및 원본 데이터
├── eda/                # 변수 분포 및 영향력 분석
├── modeling/           # 분류기 및 점수 계산 모델 코드
├── MMR_report.pdf      # 전체 리포트 및 시각화 결과
└── README.md           # 프로젝트 설명 문서
```

---

## 📌 활용 예정 내용 및 활용 사례

> **📅 업데이트 예정 항목**  
> 🎯 **Smurf / 치터 탐지**: 비정상 사용자 조기 판별  
> 🏆 **대회 운영 기준 수립**: 공정한 선발 기준 제시  
> ⚙️ **매칭 시스템 고도화**: 커뮤니티 또는 서버 운영 시 활용 가능
<p align="center">
  <img width="600" height="700" alt="civil but table 1" src="https://github.com/user-attachments/assets/25ddc58e-87ca-48f2-be6e-c253f03c163e" style="margin-right: 20px; display: inline-block;" />
  <img width="600" height="700" alt="Civil Bot Table" src="https://github.com/user-attachments/assets/5126c2ef-02dd-4307-b85e-593e0aa6dde7" style="margin-right: 20px; display: inline-block;" />
</p>

<div align="center" style="max-width: 700px; text-align: left;">
  <h4>🧮 사이트 활용 예시</h4>
  <p>
    분석된 기여도 기반으로 포지션별 고수 유저를 구분하여 MMR 기준을 시각화합니다.<br>
    이를 통해 실력 기반 매칭과 팀 구성의 질적 향상에 기여할 수 있습니다.
  </p>
</div>

**활용 사례**

---

## 🧑‍💻 작성자

**김필준 (Piljun Kim)**
📧 [kimpj1997@naver.com](mailto:kimpj1997@naver.com)
🔗 [Notion 포트폴리오 보기](https://www.notion.so/LOL-league-of-legend-238481d8bb10808e82d7d8a3ed85b6da)

---

> *“모든 승리가 같지 않듯, 우리는 팀 게임에서 진짜 중요한 것을 측정할 수 있어야 합니다.”*
