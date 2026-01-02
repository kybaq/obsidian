---
tags:
  - 클라
---

# 한 줄 요약

UI 를 가장 작은 단위 부터 조합해가며 설계하는 컴포넌트 구조화 방법.

# 구성 요소

- 원자(Atoms): 더 이상 분해할 수 없는 최소 단위의 요소 (ex. 버튼, 입력창)
- 분자(Molecules): 두 개 이상의 원자로 구성, 하나의 단위로 동작하는 간단한 UI 그룹 (ex. 폼)
- 유기체(Organisms): 원자/분자/유기체로 구성, 보다 크고 복잡한 UI 그룹 (ex. 헤더 영역)
- 템플릿(Templates): 레이아웃 그 자체, 실제 데이터는 연동하지 않은 상태의 UI 그룹
- 페이지(Pages): 템플릿의 인스턴스화, 실제로 완성한 UI

원자(Atoms) -> 분자(Molecules) -> 유기체(Oragnisms) -> 템플릿(Templeates) -> 페이지(Pages) 로 확장해나가는 방식.


> [!주의] 템플릿은 페이지에서 데이터 연동만 빠진 건데, 존재할 필요가 있는지?
> 말 그대로, 템플릿은 레이아웃만 존재하는데 여기에서 데이터를 연동하면 그게 페이지가 된다.
> 경계가 모호한 지점이 있어 Pages 로 흡수되어 사용하는 경우가 많다고 한다.


## 원자(Atoms)

```tsx
// atoms/Button.tsx
<Button variant="primary" disabled />
```

**특징**

- 재사용 최우선
- 원자 하나로는 큰 기능적 의미 없음

## 분자(Molecules)

```tsx
// molecules/SearchInput.tsx
<Input />
<Button />
```

**특징**

- 최소한의 기능적 의미를 나타냄(ex. 검색을 위한 입력 창과 제출 버튼)

### 유기체(Organisms)

```tsx
// organisms/Header.tsx
<Logo />
<SearchInput />
<UserMenu />
```

**특징**

- 기능 단위로 의미를 갖는 그룹(ex. Header, Footer, Navigation Bar 등)
- API 를 연결하는 지점으로서 역할
- 실제 페이지 레이아웃에서 어느 위치에 배치할지 보이기 시작하는 그룹

## 템플릿(Templates)

```tsx
// templates/MainLayout.tsx
<Header />
<Content />
<Footer />
```

**특징**

- 실제 데이터 ❌
- 구조만 있음
- 요즘 실무에서는 **잘 안 쓰거나 Page로 흡수되는 경우 많음**

## 페이지(Pages)

```tsx
// pages/HomePage.tsx
<MainLayout>
  <ProductList data={...} />
</MainLayout>
```

**특징**

- API 호출
- 상태 관리
- 라우팅 연결
- 비즈니스 로직 존재


# Atomic Design 을 사용하는 목적

## 목적

1. **재사용성 극대화**
2. **디자인 시스템과의 정렬**
3. **컴포넌트 책임 범위 명확화**
4. **팀원 간 암묵적 규칙 제공**

# 각 요소를 구분하기 어려울 때

 “이 컴포넌트의 **책임과 재사용 범위**가 어디까지?” 를 생각하는게 도움이 된다.
 
예를 들어,

- 다른 프로젝트에서도 그대로 쓸 수 있나? → atom
- 특정 UI 의미가 생겼나? → molecule
- 화면 기능 단위인가? → organism

| 단계       | 상태                      |
| -------- | ----------------------- |
| Atom     | UI 상태 (hover, disabled) |
| Molecule | 입력 값 처리                 |
| Organism | API, 비즈니스 상태            |
| Pages    | 전체 흐름 관리                |
