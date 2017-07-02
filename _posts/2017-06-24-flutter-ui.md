---
layout: post
title: Flutter - 구글의 새로운 크로스 플랫폼 UI 프레임워크
date: 2017-06-24 03:24:03 +0000
---

<center>
<img src="https://steemitimages.com/DQmdRAKUiMLUtRSFG61z4fWKAMgXn9kVEPPKWK9HZtCT127/image.png" style="max-width:100%;">
</center>
Flutter라는 구글의 새로운 모바일용 UI 프레임워크를 소개할게요.

### 특징
- 부드러운 사용자 경험, 높은 퍼포먼스를 목표로 해요.
- 크로스 플랫폼 UI 프레임워크 (안드로이드, iOS를 지원하고, 구글의 새로운 OS [Fuchsia](https://github.com/fuchsia-mirror)를 지원할 것으로 보여요)
- 구글의 (한 때 버려졌던) 프로그래밍 언어 [Dart](https://www.dartlang.org/) 기반이에요!
- 구글의 프로젝트이지만 [AngularJS](https://angularjs.org/)가 아니라 페이스북 [React](https://facebook.github.io/react/)의 Declarative 스타일을 따라가는 것 같아요.
- 유사한 프로젝트인 [React Native](https://facebook.github.io/react-native/)와는 다르게 네이티브(OEM) 위젯을 이용하지 않고, 모든 것을 C++로 구현한 자체 그래픽 엔진(Skia+...)로 직접 렌더링해요!
  구글의 프로젝트이지만 거의 모든 위젯을 iOS 스타일로도 렌더링할 수 있게 다 구현해놨어요.
- 핫 리로드 기능을 제공해요!

<center>
<img src="https://steemitimages.com/DQmdhnP4ratHBcwh3YBgm249xwzxUaFYC7bxpdbJ64pu58q/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202017-06-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.58.56.png" style="max-width:100%;">
</center>

구글도 프로덕션 레벨에서 사용하고 있다고는 하지만, 아직 완전 초기 단계인 프로젝트로 보여요.
그래서 실사용에는 무리가 있지 않을까 싶어요. 하지만 앱 개발자 혹은 웹 개발자라면 계속 지켜봐야할 프로젝트인 것 같아요.
관심있으신 개발자 분들은 한번 찾아보시면 좋을 것 같습니다.

https://flutter.io/