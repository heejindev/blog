---
layout: post
title: C/C++ 프로젝트를 WebAssembly로 빌드하기
date: 2017-07-01 01:24:21 +0000
---

안녕하세요 @heejin입니다.

최근에 [WebAssembly에 대한 소개글](https://steemit.com/kr-dev/@heejin/webassembly)을 올린적이 있는데요.
오늘은 제가 WebAssembly를 사용한 과정에 대해서 올려보려고 합니다.

저는 보통 완전한 학습 목적의 코딩보다는 실용적인 목적으로 코딩을 할 때가 많은데요.
이번에 WebAssembly를 사용해보려고 한 것도 그런 의도가 있었습니다.

인터넷에는 이미지를 압축해주는 사이트가 많은데요.  특히 저는 GIF를 압축해주는 사이트를 자주 이용합니다.
그런데 기존의 GIF 압축 사이트는 단점이 하나 있어요.
바로 내 GIF 파일을 업로드하고, 서버에서 압축한 다음, 내가 다시 내려받아야 한다는 점이죠.
서버가 내 GIF 이미지를 몰래 빼돌릴 가능성도 있고, 업로드/다운로드 과정이 필요하기 때문에 시간이 많이 걸립니다.

저는 WebAssembly를 이용해서 이런 문제들을 해결할 수 있을거라 생각했습니다.
WebAssembly를 이용하면 서버에 업로드하지 않고, 사용자의 브라우저 단에서 네이티브의 속도로 GIF를 압축할 수 있기 때문에 서버에 올리는 방식에 비해 다음과 같은 이점이 생깁니다:

- 업로드/다운로드 트래픽이 없기 때문에 서버 유지비가 굉장히 많이 줄어든다.
- 업로드/다운로드가 없기 때문에 사용자 입장에서도 빠른 속도로 서비스를 이용할 수 있고, 보안 이슈도 거의 없다.

WebAssembly를 이용하는게 업로드/다운로드 방식보다 거의 모든 면에서 이득인 방식으로 보였습니다.
그래서 바로 만들어보기로 결정했습니다.

원래 GIF 압축 서비스를 WebAssembly를 이용해서 어떻게 만들었는지, 그 과정을 다 설명해드리려고 했는데, 엄청나게 길어질 것 같아서 이번에는 간단하게 WebAssembly 사용법에 대해서만 알려드리겠습니다.

## WebAssembly 사용하기

저는 C로 구성된 GIF 라이브러리를 WebAssembly로 빌드해보기로 했습니다.
[giflossy(fork of gifsicle)](https://github.com/pornel/giflossy)라는 라이브러리를 이용했습니다.

**아래 과정은 모두 `macOS`에서 진행했습니다.**

### Emscripten 설치
<center>
<img src="https://steemitimages.com/DQmUvsDBB8dPzgnw4R6ejgKJnxKtJu8vpEns8fj5Sya2oW2/image.png" style="max-width:100%;">
</center>
기존 C/C++ 프로젝트를 WebAssembly로 빌드하기 위해서는 [Emscripten](http://kripken.github.io/emscripten-site/)을 사용하면 됩니다.
저는 http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html 문서의 사용법을 정확하게 따라서 `emsdk`를 통해서 Emscripten을 설치했습니다.

### giflossy 프로젝트 받기
먼저 Git을 이용해서 giflossy 프로젝트를 다운받습니다.
```
git clone https://github.com/pornel/giflossy.git
```

### giflossy 프로젝트 컴파일
다운받은 giflossy의 폴더에서 아래 명령어를 통해서 giflossy 프로젝트를 `.bc` 파일로 아주 간단하게 빌드할 수 있습니다.
오류가 굉장히 많이 발생할 것으로 예상했으나, 의외로 하나의 오류도 없이 진행할 수 있었습니다.

```
emconfigure ./configure
emmake make
```
&nbsp;
 이렇게 하면 giflossy의 `src` 폴더에 `gifsicle` 파일이 생깁니다. **이게 원래 확장자가 `.bc`가 되어야 하는데, 아무 확장자가 붙지 않은채로 컴파일되었기 때문에 아래 명령어를 통해서 확장자를 `.bc`로 바꿔줍니다.**

```
mv src/gifsicle src/gifsicle.bc
```

> **`.bc` 확장자가 아닌 경우 `emcc`에 의해서 빌드되지 않습니다.**

그리고 마지막으로 `emcc`를 통해서 `gifsicle.bc` 파일을 `gifsicle.wasm` 형태의 WebAssembly 파일로 빌드할 수 있습니다.

```
emcc src/gifsicle.bc -s WASM=1 -s "MODULARIZE=1" -s "EXPORT_NAME='Gifsicle'" -o src/gifsicle.js
```
> output 파일을 `.js`로 정하면 `.wasm` 파일과 `.js` 파일이 동시에 생성됩니다. 브라우저에서는 `.wasm` 파일을 직접 이용하기보다는 `.js`를 이용하게 됩니다.

## 브라우저에서 생성된 WebAssembly 파일을 이용하기

HTML에서 아래와 같은 코드를 사용해서 WebAssembly를 쉽게 브라우저 단에서 사용할 수 있습니다.
```
<script type="text/javascript" src="./src/gifsicle.js"></script>
<script type="text/javascript">
var gifsicle = new Gifsicle();

setTimeout(function() {
  // Gifsicle 내부적으로 .wasm 파일을 다운로드하기 때문에 약간의 시간을 기다린 후,
  // gifsicle 오브젝트를 사용할 수 있습니다.
}, 1000);
</script>
```
> `new Gifsicle()`에서 `Gifsicle`은 WebAssembly로 빌드할 때 지정한 `EXPORT_NAME`과 같아야 합니다.

---
이번 글은 조금 많이 어려운 것 같습니다ㅠ 일반인 분들이 보기에는 적합하지 않고, WebAssembly에 관심 있으신 개발자 분들에게만 아주 약간의 도움이 될 것 같네요.
혹시 WebAssembly를 사용하시다가 어려운 점 있으시면 질문 주세요. 제가 아는 선에서 답변 드리도록 하겠습니다 :)