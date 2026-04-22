# 🎮 LoL 사용자 설정 게임 기반 MMR 지표 개발 및 실제 서비스 제공

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=R&logoColor=white"/>
  <img src="https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=MySQL&logoColor=white"/>
  <img src="https://img.shields.io/badge/LoL_API-5C2D91?style=for-the-badge&logo=riot-games&logoColor=white"/>
</p>


> **“승패가 아닌, 실제 기여도로 플레이어를 평가할 수 있는가?”**

기존 MMR 시스템은 동일 포지션 + 유사 실력 매칭을 전제로 설계되어 있다.
하지만 사용자 설정 게임(내전) 환경에서는 이 전제가 깨지며, 실제 퍼포먼스를 반영하지 못하는 문제가 발생한다.

본 프로젝트는 이러한 한계를 해결하기 위해 **퍼포먼스 기반 MMR 시스템 (MMR_custom)** 을 설계하고 실제 서비스로 구현하였다.

---
## 🚨문제 정의

내전 환경에서는 다음과 같은 구조적 문제가 발생한다:

- 포지션 비고정 → 숙련도 반영 불가
- 팀 간 실력 불균형 → 승패 왜곡
- 동일 포지션 간 실력 차이 존재
- 경기 난이도 반영 불가
- 개인 기여도와 무관한 평가 구조

👉 기존 MMR은 “공정한 매칭 환경”을 가정하기 때문에
비정형 환경에서는 실제 실력을 제대로 평가하지 못한다.

---
## 🎯 목표

- ✅ 단순 승패가 아닌 **기여도 기반 평가 시스템 구축**
- ✅ **상대 실력 및 경기 난이도 반영**
- ✅ **포지션별 특성 고려한 정량 평가**
- ✅ **실제 서비스 적용 가능한 MMR 시스템 설계**
<hr>

<h2>⚙️ MMR_custom 시스템 구조</h2>

<p>
본 프로젝트는 단순한 승패가 아닌, 개인의 기여도, 상대 비교, 경기 난이도를 모두 반영하는
퍼포먼스 기반 MMR 시스템을 구축한다.
</p>

<pre>
게임 데이터:</strong> 전적 로그 기반 데이터 수집
→ Feature Engineering:</strong> per-minute, per-death 등 정규화 지표 생성
→ 포지션별 가중치:</strong> 역할별 중요 지표 반영
→ Game Impact:</strong> 게임 내 영향력 수치화
→ 인분 지표:</strong> 팀 내 기여도 평가
→ 상대 비교:</strong> 동일 포지션 상대와 비교
→ Elo 반영:</strong> 기대 성과 대비 실제 성과 반영
→ MMR 업데이트:</strong> 안정성 로직 적용
</pre>

<hr>


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
<hr>



<h2>📊 핵심 지표</h2>

<h3>1. Game Impact Score</h3>
<p>
단순 KDA가 아닌 다양한 게임 지표를 기반으로 플레이어의 전체 영향력을 수치화한 지표
</p>

<ul>
  <li>포지션별 중요 지표 반영</li>
  <li>RandomForest 기반 비선형 관계 반영</li>
  <li>정규화를 통한 공정 비교</li>
</ul>

<h3>2. 팀 내 기여도 (인분)</h3>
<p>
팀 전체 영향력 대비 개인 비율을 기반으로 계산된 지표
</p>

<ul>
  <li>패배팀에서도 높은 기여도 반영 가능</li>
  <li>강한 팀에서의 과대평가 방지</li>
  <li>직관적인 해석 가능</li>
</ul>

<h3>3. 상대 포지션 비교</h3>
<p>
동일 포지션 상대와의 퍼포먼스를 비교하여 라인전 영향력을 평가
</p>

<ul>
  <li>직접적인 경쟁 구도 반영</li>
  <li>상대 난이도 반영</li>
  <li>공정한 비교 기준 제공</li>
</ul>

<h3>4. 기대 성과 대비 실제 성과</h3>
<p>
Elo 기반 기대 승률과 실제 퍼포먼스를 결합하여 상대 성과 반영
</p>

<ul>
  <li>강한 상대 상대로 높은 성과 → 높은 가치</li>
  <li>약한 상대 상대로 평균 성과 → 낮은 가치</li>
</ul>


<h2>🛠 분석 방법</h2>

<p>
초기에는 로지스틱 회귀를 활용했지만, 게임 데이터의 비선형성과 변수 간 상호작용을 충분히 반영하지 못하는 문제가 발생하였다.
</p>

<p>
이를 해결하기 위해 RandomForest 기반 Feature Importance를 활용하여:
</p>

<ul>
  <li>비선형 관계 반영</li>
  <li>변수 간 상호작용 고려</li>
  <li>포지션별 중요 지표 도출</li>
</ul>

<p>
또한 Quantile Transformation을 통해 포지션 간 분포 차이를 보정하였다.
</p>

<hr>

<h2>📈 MMR 산정 방식</h2>

<ul>
  <li>초기 MMR: 1300</li>
  <li>승리: +20</li>
  <li>패배: -15</li>
</ul>

<p>
단, 실제 MMR 변화량은 다음 요소를 반영하여 동적으로 조정된다:
</p>

<ul>
  <li>개인 기여도</li>
  <li>상대 대비 퍼포먼스</li>
  <li>기대 성과 대비 결과</li>
</ul>

<p>
안정성을 위해 다음 로직 적용:
</p>

<ul>
  <li>K-factor 감소</li>
  <li>변동 폭 제한</li>
  <li>분포 안정화</li>
</ul>

<hr>

<h2>🚀 결과 및 개선 효과</h2>

<ul>
  <li>기존 KDA 중심 평가 → <strong>기여도 기반 평가로 전환</strong></li>
  <li>단순 승패 기록 → <strong>퍼포먼스 중심 분석 가능</strong></li>
  <li>동일 승리 게임에서도 <strong>캐리한 게임과 팀에 의존한 게임을 구분 가능</strong></li>
  <li>패배 경기에서도 높은 퍼포먼스가 있었다면 <strong>MMR 상승 반영 가능</strong></li>
</ul>

<p>
기존 전적 시스템은 KDA와 승패 중심으로 구성되어 있어, 실제로 누가 팀에 더 크게 기여했는지 직관적으로 파악하기 어렵다.
본 프로젝트는 <strong>MMR</strong>, <strong>game_n_person_contribution(인분 지표)</strong>, 
<strong>game_impact_vs_opponent(라인전 지표)</strong>, 
<strong>game_impact_winloss_norm</strong>을 활용하여 이러한 한계를 보완하였다.
</p>

<p align="center">
  <img width="700" src="https://github.com/user-attachments/assets/7c94e894-daae-4a78-90a4-2505dcb21ac8"/>
</p>

<p align="center">
  <strong>내전 전적 페이지 예시</strong>
</p>

<p>
기존 유저 정보 오른쪽 빈 공간에는 <strong>레이더 차트</strong>와 간단한 해석 문구를 배치하여 
플레이 스타일을 직관적으로 보여줄 수 있다.
또한 최근 전적 영역에서는 챔피언 정보 아래에 <strong>인분 지표</strong>, <strong>라인전 지표</strong>, 
그리고 <strong>MMR 등락 폭</strong>을 함께 표시함으로써 단순 KDA 이상의 해석이 가능하다.
</p>
<hr>

<h2>💬 서비스 적용 및 분석</h2>

<p>
MMR_custom 지표는 단순 분석을 넘어 실제 서비스 환경에서 활용 가능하도록 설계되었다.
기존 KDA 및 승률 중심 분석을 확장하여, 유저 및 챔피언 단위에서 보다 정교한 퍼포먼스 분석이 가능하다.
</p>

<!-- 🔥 1. 유저 분석 -->
<h3>👤 유저 분석</h3>

<p align="center">
  <img width="420" src="https://github.com/user-attachments/assets/6a88488b-ddc2-4678-b33a-7b1360d90920")/>
</p>

<p>
MMR과 game_impact_winloss_norm 지표를 기반으로 유저의 전체 퍼포먼스를 분석한다.
기존 KDA·승률 중심 분석과 달리, 팀 내 기여도와 상대 대비 영향력을 함께 해석할 수 있다.
</p>

<hr>

<!-- 🔥 2. 챔피언 분석 -->
<h3>🧠 챔피언 분석</h3>

<p align="center">
  <img width="600" src="https://github.com/user-attachments/assets/dde9561a-e3b3-4856-a928-0ec5ba7786c4"/>
</p>

<p>
기존 챔피언 분석 페이지는 승률과 판수 중심으로 구성되어 있다.
하지만 본 프로젝트의 지표를 적용하면, 단순 승률이 아닌 실제 퍼포먼스와 기여도를 반영한 분석이 가능하다.
</p>

<p>
이를 통해 기존 랭킹에서는 확인할 수 없었던 <strong>실제 영향력</strong>,
<strong>라인전 경쟁력</strong>, <strong>챔피언별 기여도 차이</strong>까지 확장하여 분석할 수 있다.
</p>

<hr>

<h2>🌐 활용 가능성 및 확장 방향</h2>

<ul>
  <li><strong>스머프 / 비정상 유저 탐지</strong>: 비정상적인 퍼포먼스 패턴을 기반으로 이상 유저 조기 식별</li>
  <li><strong>공정한 경쟁 기준 수립</strong>: 단순 승률이 아닌 기여도 기반 선수 선발 기준 제공</li>
  <li><strong>매칭 시스템 고도화</strong>: 퍼포먼스 기반 매칭으로 기존 MMR 한계 보완</li>
  <li><strong>챔피언 분석 정밀도 향상</strong>: 승률·판수 중심 분석을 넘어 실제 영향력 기반 분석 가능</li>
</ul>

<p>
기존 챔피언 분석 페이지는 <strong>승률</strong>과 <strong>판수</strong> 중심으로 구성되어 있어,
실제 퍼포먼스와 기여도를 반영하는 데 한계가 있다.
하지만 본 프로젝트의 지표를 적용하면 단순 결과가 아닌 
<strong>실제 영향력과 기여도 기반 분석</strong>이 가능해진다.
</p>

<p>
이를 통해 기존 랭킹 시스템에서는 확인할 수 없었던 
<strong>라인전 경쟁력</strong>, <strong>실제 영향력</strong>, 
<strong>챔피언별 기여도 차이</strong>까지 확장하여 분석할 수 있다.
</p>

<!-- 🔥 서비스 적용 이미지 -->
<p align="center">
  <img width="450" height="700" src="https://github.com/user-attachments/assets/1e2e7fdc-9772-4c2d-8275-28c25d710155%22" style="margin-right: 10px; display: inline-block;" />
</p>

<p align="center">
  <strong>MMR 기반 서비스 인터페이스 적용 예시</strong>
</p>

<p>
한 게임 내에서 유저의 퍼포먼스를 직관적으로 보여주고,
MMR을 기반으로 라인별 실력 순위 및 기여도를 함께 제공할 수 있다.
이를 통해 기존 전적 시스템보다 더 깊이 있는 유저 분석과 비교가 가능하다.
</p>
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

## 📌 예상되는 콘텐츠 및 활용 사례

### **📅 업데이트 예정 항목**

> 🎯 **스머프 / 핵 탐지**: 비정상 사용자 조기 감지
>
> 🏆 **경쟁 기준 수립**: 공정한 선정 기준 제시
>
> ⚙️ **매칭 시스템 고도화**

<p align="center">
  <img width="450" height="700" alt="civil but table 1" src="https://github.com/user-attachments/assets/25ddc58e-87ca-48f2-be6e-c253f03c163e" style="margin-right: 10px; display: inline-block;" />
  <img width="450" height="700"  alt="ciVIL BOT TABLE" src="https://github.com/user-attachments/assets/5126c2ef-02dd-4307-b85e-593e0aa6dde7" style="margin-left: 10px; display: inline-block;" />
</p>

<div align="center" style="max-width: 700px; text-align: left;">
  <h4> 🧮 인터넷 서비스 제공 예시 </h4>
  <p>
    한 게임 내 유저의 지표를 직관적으로 보여줍니다.<br>
    MMR을 토대로 라인별 실력 순위를 제공합니다.
  </p>
</div>

### **💬 활용 사례**
<p align="center">
  <img width="439" height="622" alt="Image" src="https://github.com/user-attachments/assets/36470bd3-00d5-4b87-bc52-aa7d7f036ffb" style="margin-right: 10px; display: inline-block;" />
  <img width="565" height="622" alt="Image" src="https://github.com/user-attachments/assets/addf6436-4bae-4a4e-8471-b085722a50de" style="margin-left: 10px; display: inline-block;" />
</p>

<div align="center" style="max-width: 700px; text-align: left;">
  <h4> 🧮 서비스 제공 사례 </h4>
  <p>
  mmr 지표를 토대로 유저들의 개선점 제안.<br>
  Discord BOT 연동 및 실제 서비스 제공.
  </p>
</div>


---

## 🧑‍💻 작성자

**김필준 (Piljun Kim)**
📧 [kimpj1997@naver.com](mailto:kimpj1997@naver.com)
🔗 [Notion 포트폴리오 보기](https://www.notion.so/LOL-league-of-legend-238481d8bb10808e82d7d8a3ed85b6da)

---

> *“모든 승리가 같지 않듯, 우리는 팀 게임에서 진짜 중요한 것을 측정할 수 있어야 합니다.”*
