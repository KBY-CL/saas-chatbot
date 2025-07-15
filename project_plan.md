# AI 건설안전 전문가 챗봇 UI 개발 계획서

## 1. 프로젝트 개요

### 프로젝트 기본 정보
- **프로젝트명**: AI 건설안전 전문가 챗봇 UI 개발
- **프로젝트 목표**: 건설 현장의 작업 공정별 위험 요인과 안전 대책을 안내하는 AI 챗봇의 사용자 인터페이스(UI)를 개발. 직관적이고 안정적인 UI를 제공하여 사용자가 쉽고 빠르게 필요한 정보를 얻을 수 있도록 함.
- **개발 기간**: 총 1.5주 ~ 2주 (10-14일)
- **대상 사용자**: 건설 현장 작업자, 안전 관리자, 현장 감독자

### 기술 스택
- **AI 코드 생성**: Cursor AI
- **프론트엔드**: Vue.js 3, Nuxt 3, Vuetify 3
- **백엔드 연동**: n8n (Webhook 방식)
- **상태 관리**: Pinia (Vue 3 권장)
- **HTTP 클라이언트**: Nuxt의 내장 `$fetch` 또는 `useFetch`
- **아이콘**: Material Design Icons (MDI)
- **스타일링**: SCSS/CSS3

## 2. 상세 요구사항

### 2.1 UI/UX 요구사항
#### 레이아웃 구조
```
┌─────────────────────────────────────┐
│ 헤더 (타이틀, 창 제어 버튼)            │
├─────────────────────────────────────┤
│                                     │
│ 대화 내용 표시창                      │
│ - 사용자 메시지 (우측 정렬)            │
│ - AI 응답 메시지 (좌측 정렬)           │
│ - 자동 스크롤 기능                    │
│                                     │
├─────────────────────────────────────┤
│ 입력창 + 전송버튼 + 음성인식 버튼       │
└─────────────────────────────────────┘
```

#### 색상 팔레트
- **헤더 배경**: #FF5722 (Deep Orange)
- **사용자 메시지 박스**: #E3F2FD (Light Blue)
- **AI 메시지 박스**: #F5F5F5 (Light Grey)
- **액센트 컬러**: #FF5722 (Deep Orange)
- **텍스트 컬러**: #212121 (Dark Grey)

### 2.2 기능 요구사항
#### 핵심 기능
1. **메시지 송수신**
   - 사용자 입력 (텍스트)
   - AI 응답 수신 및 표시
   - 실시간 타이핑 애니메이션

2. **사용자 편의 기능**
   - 메시지 복사 기능
   - 대화 내용 저장 (LocalStorage)
   - 음성인식 버튼 (연동 준비)
   - 창 최소화/최대화/닫기

3. **UX 개선 기능**
   - 로딩 인디케이터
   - 에러 핸들링 및 사용자 피드백
   - 자동 스크롤 및 스크롤 위치 기억

### 2.3 API 연동 스펙
#### n8n Webhook 연동
```javascript
// 요청 형식
POST /webhook/construction-safety-chat
{
  "message": "철근 작업 시 안전 장비는 무엇인가요?",
  "timestamp": "2024-01-15T10:30:00Z",
  "sessionId": "uuid-session-id"
}

// 응답 형식
{
  "response": "철근 작업 시 필요한 안전 장비는...",
  "timestamp": "2024-01-15T10:30:05Z",
  "sessionId": "uuid-session-id",
  "status": "success"
}
```

## 3. 개발 단계 및 일정

### 3.1 1단계: 프로젝트 초기 설정 (1일)
#### 개발 환경 구성
- [x] Nuxt 3 프로젝트 생성
- [x] Vuetify 3 설치 및 설정
- [x] Pinia 상태 관리 설정
- [x] 기본 폴더 구조 생성
- [x] ESLint, Prettier 설정

#### Cursor AI 프롬프트 준비
```
프로젝트 컨텍스트:
- Vue 3 + Nuxt 3 + Vuetify 3
- 건설안전 전문가 챗봇 UI
- 데스크톱 우선 반응형 디자인
- Material Design 가이드라인 준수
```

### 3.2 2단계: UI 프로토타입 생성 (2일)
#### 컴포넌트 구조
```
components/
├── layout/
│   ├── AppHeader.vue
│   └── WindowControls.vue
├── chat/
│   ├── ChatContainer.vue
│   ├── MessageList.vue
│   ├── MessageItem.vue
│   └── InputArea.vue
└── common/
    ├── LoadingSpinner.vue
    ├── CopyButton.vue
    └── FloatingMenu.vue (완료)
```

#### FloatingMenu.vue 구현 내역 (2025-07-15 기준)
- [x] 플로팅 메뉴 UI 오른쪽 하단 고정 위치
- [x] pill형(타원형) 네이비 배경, 흰색 텍스트/아이콘, flat 스타일
- [x] 메뉴 항목: 건설안전 A.I(방패), 위험성평가 A.I(경고), 세금계산서 A.I(문서), 현장개통/해지 A.I(건물)
- [x] 각 버튼 좌측 텍스트, 우측 아이콘, 넉넉한 간격
- [x] 최소화(-) 버튼: 네이비 원형 배경+흰색 아이콘, 메뉴 닫힘 시 동일 위치에 로봇 아이콘 표시
- [x] 반응형(모바일 하단 우측)
- [x] hover/포커스 시 컬러 강조, 크기 변화 없음
- [x] 접근성(aria-label, role, 키보드 포커스)
- [x] 불필요한 대시보드/사이드바/상단바 제거, 메뉴만 표시

#### 진행상황
- [x] FloatingMenu.vue UI/UX 1차 완성
- [x] 디자인 피드백 반영(색상, 위치, 아이콘, 버튼 간격 등)
- [x] 최소화/펼치기 동작 구현
- [x] 코드 구조 및 스타일 정리

#### Cursor AI 활용 지점
- 각 컴포넌트별 기본 구조 생성
- Vuetify 컴포넌트 활용한 UI 구현
- 반응형 레이아웃 구성

### 3.3 3단계: 상태 관리 및 기능 구현 (3일)
#### 상태 관리 (Pinia Store)
```javascript
// stores/chat.js
export const useChatStore = defineStore('chat', {
  state: () => ({
    messages: [],
    isLoading: false,
    sessionId: null
  }),
  actions: {
    async sendMessage(message) { ... },
    addMessage(message) { ... },
    clearMessages() { ... }
  }
})
```

#### 구현 기능
- [ ] 메시지 송수신 로직
- [ ] 로컬 스토리지 연동
- [ ] 에러 처리 및 재시도 로직
- [ ] 메시지 복사 기능
- [ ] 음성인식 버튼 UI (기능 연동 준비)

### 3.4 4단계: n8n 연동 및 테스트 (2일)
#### API 통신 구현
```javascript
// composables/useChat.js
export const useChat = () => {
  const sendMessage = async (message) => {
    try {
      const response = await $fetch('/api/chat', {
        method: 'POST',
        body: { message, sessionId: generateSessionId() }
      })
      return response
    } catch (error) {
      throw new Error('메시지 전송 실패')
    }
  }
  return { sendMessage }
}
```

#### 테스트 시나리오
- [ ] 정상적인 메시지 송수신
- [ ] 네트워크 오류 처리
- [ ] 긴 응답 시간 처리
- [ ] 동시 다중 요청 처리

### 3.5 5단계: 최종 검토 및 최적화 (1일)
#### 성능 최적화
- [ ] 컴포넌트 지연 로딩
- [ ] 메시지 가상화 (긴 대화 내용)
- [ ] 번들 크기 최적화

#### 접근성 및 사용성
- [ ] 키보드 내비게이션
- [ ] 스크린 리더 지원
- [ ] 고대비 모드 지원

## 4. Cursor AI 활용 전략

### 4.1 효과적인 프롬프트 작성법
```
# 컴포넌트 생성 시
"Vue 3 Composition API와 Vuetify 3을 사용하여 
채팅 메시지 컴포넌트를 생성해주세요.
- 사용자/AI 메시지 구분 표시
- 복사 버튼 포함
- 타임스탬프 표시
- Material Design 가이드라인 준수"

# 스타일링 시
"다음 디자인 요구사항에 맞춰 SCSS 스타일을 작성해주세요:
- 헤더 배경: #1976D2
- 둥근 모서리 8px
- 그림자 효과 적용
- 반응형 디자인 (모바일: 768px 이하)"
```

### 4.2 코드 리뷰 체크리스트
- [ ] Vue 3 Composition API 올바른 사용
- [ ] Vuetify 3 컴포넌트 최신 문법 적용
- [ ] TypeScript 타입 안정성 확인
- [ ] 접근성 (aria-label, role 등) 고려
- [ ] 성능 최적화 (v-memo, defineAsyncComponent 등)

## 5. 리스크 관리

### 5.1 기술적 리스크
| 리스크 | 영향도 | 확률 | 대응 방안 |
|--------|--------|------|-----------|
| AI 생성 코드 품질 | 중간 | 높음 | 단계별 코드 리뷰 및 테스트 |
| Vuetify 3 호환성 | 높음 | 중간 | 공식 문서 참조 및 대체 컴포넌트 준비 |
| 백엔드 연동 지연 | 높음 | 중간 | Mock API 우선 개발 |
| 성능 이슈 | 중간 | 낮음 | 프로파일링 및 최적화 |

### 5.2 일정 리스크
- **완화 전략**: 매일 진행 상황 점검 및 조정
- **예비 계획**: 필수 기능 우선 개발 후 부가 기능 추가

## 6. 향후 발전 계획

### 6.1 단기 개선 사항 (1-2주)
- [ ] 음성인식 API 연동 (Web Speech API)
- [ ] 대화 내용 검색 기능
- [ ] 메시지 즐겨찾기 기능
- [ ] 다크 모드 지원

### 6.2 중기 개선 사항 (1-2개월)
- [ ] 모바일 앱 버전 개발 (PWA)
- [ ] 오프라인 모드 지원
- [ ] 다국어 지원 (i18n)
- [ ] 사용자 맞춤 설정

### 6.3 장기 개선 사항 (3-6개월)
- [ ] AI 학습 데이터 피드백 시스템
- [ ] 관리자 대시보드 개발
- [ ] 통계 및 분석 기능
- [ ] 팀 협업 기능

## 7. 품질 보증

### 7.1 테스트 계획
- **단위 테스트**: Vue Test Utils + Vitest
- **통합 테스트**: Cypress 또는 Playwright
- **사용자 테스트**: 실제 건설 현장 작업자 대상

### 7.2 코드 품질 관리
- **코딩 컨벤션**: ESLint + Prettier
- **커밋 메시지**: Conventional Commits
- **코드 리뷰**: Pull Request 기반

## 8. 참고 자료

### 8.1 개발 문서
- [Vue 3 공식 문서](https://vuejs.org/)
- [Nuxt 3 공식 문서](https://nuxt.com/)
- [Vuetify 3 공식 문서](https://vuetifyjs.com/)
- [Material Design Guidelines](https://material.io/design)

### 8.2 Cursor AI 활용 가이드
- 명확하고 구체적인 요구사항 제시
- 기술 스택과 버전 명시
- 예상 결과물 샘플 제공
- 단계별 진행으로 복잡도 관리