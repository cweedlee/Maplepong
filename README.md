# ft_transcendence

**React-like SPA Framework built from scratch in Vanilla JS**  
바닐라 JS로 직접 구현한 React 유사 SPA 프레임워크

---

## Overview / 프로젝트 개요

A pong game and user management web application built without any frontend framework.  
The core challenge was implementing a React-like SPA framework from scratch using only vanilla JavaScript.

외부 프론트엔드 라이브러리 없이 제작한 핑퐁 게임 및 유저 관리 웹 애플리케이션입니다.  
바닐라 JavaScript만으로 React 유사 SPA 프레임워크를 직접 구현하는 것이 핵심 과제였습니다.

---

## Core Framework / 핵심 프레임워크

> Framework source is located in `/src/core`  
> 프레임워크 소스코드는 `/src/core` 경로에 있습니다

The framework implements the following React internals from scratch:  
다음 React 내부 동작을 직접 구현했습니다:

- **Functional Components** — 함수형 컴포넌트 구조
- **FiberNode** — React의 Fiber 아키텍처 구현
- **Virtual DOM & Diff Algorithm** — 가상 DOM 및 diff 알고리즘
- **useState** — 상태 관리 훅
- **useEffect** — 사이드 이펙트 훅
- **useRef** — DOM 참조 훅
- **useGlobalState** — 전역 상태 관리
- **Client-side Router** — 클라이언트 사이드 라우팅
- **Event Bubbling** — 이벤트 버블링 처리

> Why functional components?  
> 클래스형 컴포넌트는 가독성이 떨어지고, `this`가 mutable하며 메모리를 더 소비한다는 점을 고려하여 함수형 컴포넌트 형태로 설계했습니다.

---

## Features / 주요 기능

- Pong game (real-time multiplayer) / 핑퐁 게임 (실시간 멀티플레이)
- User registration & login / 회원가입 및 로그인
- Friend system / 친구 추가 기능
- Game invitation / 게임 초대 기능

---

## Tech Stack / 기술 스택

| Part | Stack |
|------|-------|
| Frontend | Vanilla JavaScript |
| Backend | Django |
| Infra | Nginx, Docker |
| Auth | JWT Token |
| Realtime | WebSocket |
| UI | Bootstrap |

---

## Role / 담당 역할

Frontend Lead / 프론트엔드 팀장

- Designed and implemented the entire core framework / 프레임워크 전체 설계 및 구현
- Page development / 페이지 기능 구현
- UI design / UI 디자인

---

## Note / 참고

Backend is in a separate repository. Local execution requires backend setup.  
백엔드는 별도 레포에 있습니다. 로컬 실행 시 백엔드 설정이 필요합니다.

For implementation details and problem-solving process, see HERE:  
구현 과정 및 문제 해결 내용은 여기를 참고해주세요:  
👉 [HERE](./CORE.md)

---

## Period / 진행 기간

2024.02 — 2024.06
