---
layout: post
title: WebAssembly - 웹이 더 빨라집니다
date: 2017-06-28 11:45:33 +0000
---

<center>
<img src="https://steemitimages.com/DQmSt9apzm6oXLTFYiyyVyKYSrypQBCcyiaRJ4g4mxuYehH/image.png" style="max-width:100%;">
</center>

제가 최근에 관심있게 보고 있는 새로운 웹 프로그래밍의 패러다임, [WebAssembly](http://webassembly.org/)에 대해서 소개해드리겠습니다.

간단하게 얘기하면 **WebAssembly는 인터넷 홈페이지에서 네이티브(C/C++)에 가까운 성능의 프로그램을 실행할 수 있게 해주는 기술입니다.**

보통 웹사이트는 HTML와 CSS 그리고 JavaScript라는 프로그래밍 언어를 사용해서 만들게 됩니다. 우리는 잘 인지할 수 없지만, 사실 웹사이트는 데스크탑 어플리케이션과 같은 하나의 프로그램입니다.
하지만 웹사이트라는 프로그램은 HTML과 CSS 그리고 JavaScript만 이용해서 만들어야 한다는 단점을 가지고 있습니다.
HTML과 CSS는 사실상 문서의 모양과 형태를 표현하는 언어이기에 프로그래밍 언어로 보기 어렵고, 웹사이트의 프로그램적인 요소는 JavaScript가 담당하고 있습니다.
JavaScript가 [JIT](https://ko.wikipedia.org/wiki/JIT_%EC%BB%B4%ED%8C%8C%EC%9D%BC) 기술로 인해서 이전보다 훨씬 빨라졌지만 네이티브 언어(C/C++)들에 비해서는 많이 느립니다.
3D 게임이나, 동영상 편집기 등의 어플리케이션을 JavaScript만으로 구현하기에는 부족한거죠.

그래서 Microsoft의 `ActiveX`나 Google의 [PNaCl](https://developer.chrome.com/native-client/nacl-and-pnacl), Netscape의 [NPAPI](https://ko.wikipedia.org/wiki/NPAPI) 등의 기술들을 이용해서 네이티브 프로그램의 힘을 웹에서 빌려 쓰는 방식을 많이들 사용했습니다.
하지만 이런 방식들은 모두 보안에 취약했고, 통합된 표준 기술이 아니었기 때문에 각 기술을 사용하려면 그 기술에 맞는 웹브라우저를 사용해야 한다는 문제가 있었습니다.

<center>
<img src="https://steemitimages.com/DQmT3wTRWWzxn7priRd48FjEk2kkGB8dRJeFdKYkd65FFfJ/image.png" style="max-width:100%;">
</center>
2013년, Firefox를 만드는 [Mozilla](https://mozilla.org)에서 [asm.js](http://asmjs.org/)을 발표했습니다.
asm.js의 기본적인 아이디어는 말 그대로 저수준 언어인 Assembly를 JavaScript로 작성하고 그것을 웹브라우저에서 바로 실행하는 것입니다.
이로 인해서 네이티브 언어(C/C++)로 작성한 프로그램을 Assembly로 변환하고, 그걸 asm.js를 이용해서 JavaScript 형태로 변환하면 웹브라우저에서 네이티브 프로그램을 실행할 수 있게 됩니다.
사실 이건 성능 면에서는 굉장히 비효율적인 방식입니다. 바이너리로 구성된 네이티브 프로그램을 텍스트 형태인 JavaScript 파일로 변환하기 때문에 스크립트 파일의 용량이 많이 증가하고, 대부분의 브라우저에서 이런 형태의 JavaScript 파일을 실행하게끔 최적화되어 있지 않기 때문에 느릴 수 밖에 없는거죠.
ActiveX나 PNaCl 등과 달리 별도의 작업 없이 모든 브라우저에서 사용 가능하다는 장점이 있긴 했지만 사용의 어려움, 성능
 문제때문에 asm.js 자체는 크게 흥하지 못했습니다.

그러다가 HTML5의 발전과 보안을 이유로 PNaCl, NPAPI, ActiveX에 대한 지원이 점점 사라지게 됩니다.
하지만 웹에서 네이티브 수준의 성능을 낼 필요성은 점점 증가하게 되죠. (게임의 영향이 제일 크다고 봅니다)
그래서 브라우저 회사들이 웹에서 네이티브 수준의 성능을 낼 수 있게 하는 하나의 통합된 표준 기술을 개발하기로 합니다.
Google의 `PNaCl`과 Mozilla의 `asm.js`가 그 후보로 선정되고 결과적으로 asm.js이 승리했고 asm.js를 바탕으로 WebAssembly가 만들어지게 됩니다.
PNaCl의 경우 JavaScript와 HTML로부터 격리된 환경에서 돌아가게끔 구성되어 있기 때문에 다른 브라우저 업체들이 구현하기에 까다로운 편이었고, asm.js의 경우에는 JavaScript  환경에서 돌아가게끔 되어있어서 다른 브라우저들도 쉽게 구현할 수 있었기 때문으로 보입니다.

<center>
<img src="https://steemitimages.com/DQmQTvW9wyDuNHR629hpSeT7GkGp8mjtRAcavvJJS8ZfLTi/image.png" style="max-width:100%;">
WebAssembly를 이용한 게임 데모
</center>

현재 대부분의 최신 브라우저(Chrome, Edge, Firefox, Safari)에서 WebAssembly를 지원하는 상태입니다.
WebAssembly를 이용하면 네이티브 수준의 성능을 웹에서 활용할 수 있습니다.
저는 WebAssembly가 활성화되면 지금까지와는 다른 새로운 웹이 열릴 것이라고 봅니다. 어쩌면 데스크탑 어플리케이션을 다운로드할 필요가 없게 될 수도 있습니다.
Google이 꿈꾸었던 웹 브라우저가 OS가 되는 세상이 정말로 다가올지도 모릅니다.

WebAssembly가 어떤 일을 할 수 있는지 궁금하신 분은 Demo를 실행해보셔도 좋을 것 같습니다 :)
3D 게임을 웹에서 실행하는건 JavaScript만으로는 어렵습니다. 하지만 WebAssembly를 이용하면 가능합니다.

[WebAssembly 3D 게임 Demo](http://webassembly.org/demo/)

저도 지금 WebAssembly를 활용해서 간단한 프로그램을 만들어 보고있습니다. 완성되면 올리도록 하겠습니다 :)
직접 사용해본 입장에서 아직은 WebAssembly를 이용해서 제대로 된 개발을 하기에는 공식 문서도 엉망이고, 어려운 상황인 것 같습니다. 하지만 곧 상황이 좋아질 거라고 믿습니다.
그리고 WebAssembly를 사용하고 싶은 개발자 분이 계시면 공식 홈페이지보다 [emscripten](http://kripken.github.io/emscripten-site/) 페이지를 보시는 걸 추천해드립니다.

---
## 참고 자료
http://webassembly.org/roadmap/
https://blog.chromium.org/2017/05/goodbye-pnacl-hello-webassembly.html
http://robert.ocallahan.org/2017/06/webassembly-mozilla-won.html