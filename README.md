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
  <li>단순 승패 → <strong>퍼포먼스 기반 분석 가능</strong></li>
  <li>동일 승리 게임에서도 <strong>캐리 vs 팀 의존 게임 구분 가능</strong></li>
  <li>패배 경기에서도 높은 퍼포먼스 → <strong>MMR 상승 반영</strong></li>
</ul>

<p>
👉 기존 MMR 대비 “실제 퍼포먼스 반영 정확도 향상”
</p>

<p align="center">
  <img width="500" src="https://github.com/user-attachments/assets/7c94e894-daae-4a78-90a4-2505dcb21ac8"/>
</p>

<p align="center">
  <strong>MMR 기반 퍼포먼스 평가 결과 예시</strong>
</p>

<ul>
  <li><strong>캐리한 게임</strong></li>
  <li><strong>팀에 의존한 게임</strong></li>
</ul>

<p>
을 구분할 수 있도록 개선됨
</p>

<hr>

<h2>💬 서비스 적용</h2>

<ul>
  <li>Discord Bot 기반 실시간 MMR 조회</li>
  <li>사용자 랭킹 시스템 구축</li>
  <li>플레이 스타일 분석 제공</li>
</ul>

<hr>

<h2>🌐 활용 가능성</h2>

<ul>
  <li>스머프 / 비정상 유저 탐지</li>
  <li>공정한 대회 선발 기준</li>
  <li>매칭 시스템 고도화</li>
  <li>코칭 및 피드백 시스템</li>
</ul>

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
