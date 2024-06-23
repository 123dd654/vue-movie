# API를 이용한 영화사이트 만들기

## Film Chart 소개

- Film Chart 사이트는 최신 영화 랭킹 정보를 제공하는 웹사이트로,
  사용자가 영화 검색, 상세보기, 티저 영상 감상, 등장인물 확인, 관련 영화 추천 등의 기능을 통해 다양한 영화 정보를 손쉽게 얻을 수 있습니다.
  직관적인 인터페이스와 풍부한 영화 데이터베이스를 바탕으로 사용자는 원하는 영화를 빠르고 정확하게 찾을 수 있습니다.

<img src="https://github.com/123dd654/vue-movie/assets/161431124/6bc676e4-0e35-400c-bd2d-e9ca6fd17bd9" alt="메인화면">

---

## 주요 기능

- 검색 기능: 영화 제목, 장르, 출연 배우 등을 통해 다양한 방법으로 영화를 검색할 수 있습니다.
- 상세보기 기능: 영화의 줄거리, 감독, 출연진 등 상세 정보를 제공합니다.
- 티저 영상: 영화의 예고편 영상을 바로 감상할 수 있습니다.
- 등장인물: 주요 등장인물의 정보를 확인할 수 있습니다.
- 관련 영화 추천: 사용자가 관심을 가질만한 관련 영화를 추천합니다.

---

## 주요 컴포넌트 설명

#### DetailView.vue

- 사용자가 선택한 영화의 상세 정보를 제공하는 컴포넌트입니다.
  영화의 줄거리, 감독, 출연진 정보, 티저 영상 등을 포함하며, 관련 영화 목록을 함께 보여줍니다.

#### HomeView.vue

- 사이트의 메인 페이지 역할을 하는 컴포넌트로, 최신 영화 랭킹, 인기 영화 리스트 등을 사용자에게 보여줍니다.
  메인 페이지에서 사용자가 가장 관심을 가질 만한 콘텐츠를 한눈에 확인할 수 있습니다.

#### SearchView.vue

- 사용자가 입력한 검색어를 바탕으로 영화 목록을 필터링하여 결과를 보여주는 컴포넌트입니다.
  검색 결과에 따라 관련 영화를 리스트 형태로 제공하며, 각 영화에 대한 간략한 정보와 링크를 제공합니다.
  상세보기 버튼을 추가하여 정보를 확인할 수 있게 하였습니다.

---

## 사용한 기술

- Vue.js: 사용자 인터페이스를 구축하기 위한 프론트엔드 프레임워크로, 컴포넌트 기반 아키텍처를 통해 재사용 가능한 UI 구성 요소를 만들 수 있습니다.
- Vue Router: Vue.js 애플리케이션에서 클라이언트 사이드 라우팅을 관리하기 위한 공식 라우터입니다.
- Axios: HTTP 요청을 처리하기 위한 Promise 기반 HTTP 클라이언트로, API와의 통신을 용이하게 합니다.
- Vuetify: Vue.js 애플리케이션에서 사용자 인터페이스를 빠르게 구축하기 위한 Material Design 컴포넌트 프레임워크입니다.

---

## 개발 과정 중 느낀점

이 사이트를 개발하는 과정에서 가장 어려웠던 점은 Vue.js를 처음 배우면서 구조를 잡는 것이 어색했다는 점입니다.
그러나 반복적인 연습과 학습을 통해 점점 익숙해졌고, 최종적으로 안정적인 구조를 갖춘 웹사이트를 완성할 수 있었습니다.

그리고 추가적으로 key값을 입력했을때 데이터를 불러오는게 재미있었고 흥미로운 경험이 되어 기뻤습니다.
