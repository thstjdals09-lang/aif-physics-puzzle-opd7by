# 스타 카운터 (Star Counter) — 밸런스 튜닝 및 검증 보고서 (Balance Report)

**문서 버전:** v0.9.0-BETA  
**작성자:** Lead Systems Designer  
**대상 빌드:** Release Candidate Target (Build 2024.11-RC)  
**플랫폼:** Web (React + Matter.js Engine)  

---

## 1. 개요 및 밸런스 목표 (Executive Summary)

본 보고서는 `스타 카운터`의 베타 마일스톤 단계에서 진행된 물리 엔진 파라미터 보정, 미라클(Miracle) 4종의 성능 지표 재조정, 시퀀스(발사 순서) 시너지 계수 검증, 그리고 레벨별 난이도 곡선(Progression Curve) 튜닝 결과를 종합합니다.

### 1.1 핵심 밸런스 목표
1. **스타 궤적의 예측 가능성 확보:** Matter.js 기반의 궤적 제어에서 플레이어의 의도가 85% 이상 반영되도록 공기 저항 및 반발 계수 표준화.
2. **미라클 발사 순서 최적화 유도:** 단순 단일 스킬 연타를 억제하고, '순서 조절(Usage Order)'에 따른 콤보 효율 격차를 최대 2.5배로 설계하여 전략성 강화.
3. **가족 전략(Family Strategy) 타겟팅:** 스테이지 1~10의 클리어 성공률 80% 이상 유지, 후반 스테이지(21~30)는 반복 시도(평균 3.8회)를 통한 최적화 재미 유도.
4. **인플레이션 방지:** 게임 내 점수(Score) 및 스킬 포인트(Skill Upgrade Points) 수급 속도를 통제하여 30레벨 클리어 시점 최종 해금률을 82~85% 수준으로 안착.

---

## 2. 물리 엔진 파라미터 튜닝 (Matter.js Physics Tuning)

스타 발사체의 궤적 렌더링 및 물리 판정의 일관성을 위해 Matter.js 기본 환경 변수 및 스타 아키타입(Star Archetypes)별 물리 수치를 튜닝했습니다.

### 2.1 월드 물리 상수 (World Constants)
```javascript
export const PHYSICS_CONFIG = {
  engine: {
    enableSleeping: false,
    positionIterations: 8, // 충돌 관통 방지
    velocityIterations: 6,
    constraintIterations: 4,
  },
  world: {
    gravity: {
      x: 0.0,
      y: 0.85, // 낙하 안정감 보정: 기존 1.0 -> 0.85 (체공 시간 15% 증가)
      scale: 0.001,
    },
    bounds: {
      min: { x: 0, y: 0 },
      max
