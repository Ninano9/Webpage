# Design Document

## Overview

이 디자인은 현재 웹사이트의 색상 통일성 문제를 해결하고 margin 기반 레이아웃을 Flexbox로 전환하는 포괄적인 리팩토링을 다룹니다. 주요 목표는 ff533f 색상의 과도한 사용을 줄이면서도 브랜드 정체성을 유지하고, 현대적이고 유지보수가 용이한 CSS 구조를 구축하는 것입니다.

## Architecture

### 색상 시스템 아키텍처

**Primary Color Palette:**
- Primary: #ff533f (메인 브랜드 색상, 제한적 사용)
- Primary Light: #ff7a66 (호버 상태, 보조 강조)
- Primary Dark: #e63c28 (액티브 상태)
- Neutral: #f8f9fa (배경색)
- Secondary: #6c757d (텍스트, 보조 요소)
- Accent: #007bff (링크, 인터랙션)

**색상 사용 원칙:**
1. ff533f는 로고, 주요 CTA 버튼, 브랜드 요소에만 사용
2. 스크롤바, 드롭다운 등 UI 요소는 보조 색상 사용
3. 텍스트는 가독성을 위해 중성 색상 사용

### 레이아웃 시스템 아키텍처

**Flexbox 기반 레이아웃:**
```css
/* 컨테이너 시스템 */
.flex-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.flex-main {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.flex-row {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.flex-col {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
```

## Components and Interfaces

### 1. Header Component
**현재 문제점:**
- 복잡한 margin/position 기반 레이아웃
- ff533f 색상 과다 사용

**개선된 디자인:**
```css
.header {
  display: flex;
  flex-direction: column;
  background: #fff;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.header-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
}

.navigation {
  display: flex;
  justify-content: center;
  background: #f8f9fa;
}

.nav-item {
  color: #333;
  transition: color 0.3s ease;
}

.nav-item:hover {
  color: #ff533f; /* 브랜드 색상은 호버시에만 */
}
```

### 2. Content Grid System
**현재 문제점:**
- 고정된 margin 값으로 인한 반응형 제약
- 복잡한 positioning

**개선된 디자인:**
```css
.content-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
  padding: 2rem;
}

.content-section {
  display: flex;
  flex-direction: column;
  border: 1px solid #e9ecef;
  border-radius: 10px;
  overflow: hidden;
}

.section-header {
  background: #f8f9fa;
  padding: 1rem;
  border-bottom: 1px solid #e9ecef;
  font-weight: bold;
  text-align: center;
}
```

### 3. Card Component System
**현재 문제점:**
- 인라인 스타일과 복잡한 margin 조정

**개선된 디자인:**
```css
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  padding: 1rem;
}

.card {
  flex: 1;
  min-width: 250px;
  max-width: 300px;
  display: flex;
  flex-direction: column;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s ease;
}

.card:hover {
  transform: translateY(-2px);
}

.card-image {
  aspect-ratio: 16/9;
  object-fit: cover;
}

.card-content {
  padding: 1rem;
  flex: 1;
  display: flex;
  flex-direction: column;
}
```

### 4. Button System
**현재 문제점:**
- ff533f 색상의 과도한 사용

**개선된 디자인:**
```css
.btn-primary {
  background-color: #ff533f;
  border: none;
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 6px;
  transition: all 0.2s ease;
}

.btn-primary:hover {
  background-color: #e63c28;
  transform: translateY(-1px);
}

.btn-secondary {
  background-color: #6c757d;
  border: none;
  color: white;
}

.btn-outline {
  background-color: transparent;
  border: 2px solid #ff533f;
  color: #ff533f;
}
```

## Data Models

### CSS 변수 시스템
```css
:root {
  /* 색상 시스템 */
  --color-primary: #ff533f;
  --color-primary-light: #ff7a66;
  --color-primary-dark: #e63c28;
  --color-secondary: #6c757d;
  --color-accent: #007bff;
  --color-neutral-100: #f8f9fa;
  --color-neutral-200: #e9ecef;
  --color-neutral-300: #dee2e6;
  --color-neutral-800: #343a40;
  
  /* 간격 시스템 */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  --spacing-xxl: 3rem;
  
  /* 타이포그래피 */
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --font-size-xxl: 1.5rem;
  
  /* 레이아웃 */
  --container-max-width: 1200px;
  --border-radius: 8px;
  --box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
```

## Error Handling

### CSS 폴백 시스템
```css
/* Flexbox 미지원 브라우저 대응 */
.flex-fallback {
  display: block;
}

.flex-fallback > * {
  display: inline-block;
  vertical-align: top;
  width: calc(25% - 1rem);
  margin-right: 1rem;
}

/* 색상 접근성 대응 */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-neutral-100: #343a40;
    --color-neutral-800: #f8f9fa;
  }
}
```

### 반응형 브레이크포인트
```css
/* 모바일 우선 접근법 */
.responsive-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 768px) {
  .responsive-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .responsive-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

## Testing Strategy

### 1. 시각적 회귀 테스트
- 색상 변경 전후 스크린샷 비교
- 다양한 브라우저에서의 렌더링 확인
- 모바일/데스크톱 레이아웃 검증

### 2. 접근성 테스트
- 색상 대비 비율 검증 (WCAG 2.1 AA 기준)
- 키보드 네비게이션 테스트
- 스크린 리더 호환성 확인

### 3. 성능 테스트
- CSS 파일 크기 최적화 확인
- 렌더링 성능 측정
- 애니메이션 성능 검증

### 4. 크로스 브라우저 테스트
- Chrome, Firefox, Safari, Edge에서 Flexbox 동작 확인
- IE11 폴백 스타일 검증 (필요시)
- 모바일 브라우저 호환성 확인

### 5. 반응형 테스트
```css
/* 테스트용 브레이크포인트 표시 */
.debug-breakpoint::before {
  content: "Mobile";
  position: fixed;
  top: 0;
  right: 0;
  background: red;
  color: white;
  padding: 0.25rem;
  z-index: 9999;
}

@media (min-width: 768px) {
  .debug-breakpoint::before {
    content: "Tablet";
  }
}

@media (min-width: 1024px) {
  .debug-breakpoint::before {
    content: "Desktop";
  }
}
```

이 디자인은 현재 코드의 문제점들을 체계적으로 해결하면서도 기존 기능을 유지하고, 향후 확장성과 유지보수성을 크게 개선할 것입니다.