# Vite + TypeScript 환경에서 SVGR 사용하기: SVG-in-JS 
기본적으로 CRA 환경에서는 SVG를 리액트 컴포넌트로써 사용할 수 있도록 내장(built-in) 되어 구성되어 있지만, Vite 환경에서는 추가적인 패키지 설치 및 구성 설정이 필요하다.

처음 작성하려던 주제는 "Vite + TypeScript 환경에서 SVGR 사용하기"였지만  
라이브러리를 사용하여 SVG를 컴포넌트로 변환하는 방식(SVG-in-JS)이 무조건 정답이 아닐 수 있으며,  
SVG를 사용하는 다양한 대안들이 존재하며 무엇을 트레이드오프하고 있는지 고민해보며 사용할 필요가 있다는 생각이 들어 제목과는 다른 내용도 함께 작성하려고 한다. 

## SVG 그래픽 
### SVG란?
[SVG (Scalable Vector Graphics)](https://en.wikipedia.org/wiki/SVG)란 2차원 벡터 그래픽을 표현하기 위한 XML 기반의 마크업 언어이며 모던 웹 브라우저에서 사용 가능한 텍스트 기반의 웹 표준이다. 

### SVG 사용하기 
SVG는 기존 래스터(Raster) 그래픽 포멧의 이미지 포멧(`.png`, `.jpg`)에 비해 경량이며, 확대하거나 사이즈를 재조정할 경우에도 퀄리티의 손실없이 선명하다. 또한 읽기 쉬운(Readable) 코드로 조작이 용이하다. 


#### 인라인 SVG 
리액트 컴포넌트 내부에 직접 SVG 마크업을 임베드하여 사용할 수 있다.   
최적화된 SVG의 경우 `inline SVG`가 성능적으로 유리할 수 있다. (최적화되지 않은 파일의 경우 대부분 가장 느릴 수 있다.) 

```xml
<svg version="1.1"
     width="300" height="200"
     xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="red" />
  <circle cx="150" cy="100" r="80" fill="green" />
  <text x="150" y="125" font-size="60" text-anchor="middle" fill="white">SVG</text>
  <a href="https://www.naver.com">
</svg>
```

#### `<img>` 태그 사용하기 
`<img>` 태그는 SVG를 사용하는 가장 일반적인 방식이다. `<object>` 태그, `<iframe>` 태그 요소로도 사용할 수 있다. 

브라우저나 최적화와 상관없이 일관되게 가장 성능적으로 뛰어나다.   

하지만, SVG 파일은 DOM의 한 부분이 아닌 외부 파일로 취급되기에 `currentColor` CSS 값 및 CSS 커스텀 props(--variable) 사용이 어렵다. 또한 SVG에 포함된 `<a>` 태그는 클릭되지 않는다. ~~(SVG를 포함한 `<a>` 태그는 클릭이 가능하다.)~~

> :bulb: Perf tips:
> - `loading="lazy"`와 같은 속성을 사용하여 지연 로딩을 설정하거나 `importance="high"`와 같은 속성을 사용하여 fetch 우선 순위를 조정할 수 있다. ⚡️

```html
<img src={foo} className="App-logo" alt="logo" />
```

#### SVG 스프라이트 `<use/>` 태그 사용하기 
아이콘으로 사용되는 SVG 파일들을 모아 `이미지 스프라이트(Image Sprite)`로 제작하고, `<symbol>` 태그를 사용해서 아이콘을 정의하고 부여한 `id`를 통해 각 위치에서 해당하는 이미지만 사용할 수 있다.  

웹 페이지의 로딩 시간 단축 및 이미지 다운로드를 위한 서버 호출 횟수 감소, `Lighthouse` 성능 점수 상승 효과를 확인할 수 있다. 

직접 혹은 `Spritebot`, `react-svg-sprite-generator` 같은 GUI 툴이나 패키지 설치를 통해 제작할 수 있다. 

```jsx
function Icon({ id, ...props }) {
  return (
    <svg {...props}>
      <use href={`/sprite.svg#${id}`} />
    </svg>
  );
}
```

#### CSS 속성 값으로 사용하기 
CSS의 `background`, `background-image` 속성, `mask-image` 속성 값으로도 SVG를 사용할 수 있다.  

경로 사용, data URI 사용 모두 성능면에서는 가볍지 않지만 `currentColor`로 상속받거나 `background-*` 속성을 통해 세밀한 조정이 가능하다. 

```css
.container {
  background: url(foo.svg);
}
```

#### 리액트 컴포넌트로 변환하여 사용하기 
리액트에서는 [SVGR](https://react-svgr.com/)과 같은 라이브러리를 사용해 `.svg` 파일을 리액트 컴포넌트로 변환하여 사용할 수 있다.  

흔히 사용되는 방법으로 사용 측면에서 간편하며 개인적으로는 복잡해보일 수 있는 SVG 마크업을 추상화(?)하여 사용한다는 점에서 코드를 가독성 있게 만들어준다는 생각도 들었다.  

> 우아해보이는 방법이지만 최근 한 포스팅에서 `SVG-in-JS`를 **안티 패턴**으로도 언급하기도 했다. 

앞서 언급한 것처럼 기본적으로 CRA 환경에서는 SVG를 리액트 컴포넌트로써 사용할 수 있도록 내장(built-in) 되어 구성되어 있었지만,  
Vite 환경에서는 추가적인 패키지 설치 및 구성 설정이 필요하다.

```jsx
// CRA 
import { ReactComponent as FooSvg } from "./foo.svg";

// vite + ts
import FooSVG from "./foo.svg?react";


// Usage in JSX
const MyComponent = () => {
  return <FooSvg />;
};
```

<br>

## 다시 주제로 돌아와 Vite + TypeScript 환경에서 SVGR 사용하기 
```tsx
// App.tsx
import LogoSVG from "./Logo.svg?react";

  //... 
  <LogoSVG width={24} height={24}/>
```
Vite + TypeScript 환경에서 SVGR을 사용하기 위해서는 [vite-plugin-svgr](https://www.npmjs.com/package/vite-plugin-svgr) 플러그인 설치가 필요하다. 

```shell
# npm
npm install --save-dev vite-plugin-svgr
```

이후 프로젝트의 `devDependencies`에 추가 된 플러그인을 `vite.config.js` 설정 파일의 `plugins` 배열에 해당 플러그인을 포함시켜준다. 

```ts
// vite.config.ts
export default defineConfig({
    ...
    plugins: [
        svgr({
          include: ["**/*.svg?react"] 
        }),
    ...
})
```

> vite는 기본적으로 Node.js API 기반의 타입 시스템을 차용하고 있습니다.

`vite-env.d.ts` [Client Types](https://ko.vitejs.dev/guide/features.html#client-types)에 `d.ts` 선언 파일을 아래와 같이 추가한다. 

```ts
//vite-env.d.ts

/// <reference types="vite/client" />
/// <reference types="vite-plugin-svgr/client" /> // ✅ 
```
혹은 `tsconfig.json`의 `compilerOptions.types`에 타입을 추가하는 방법도 있다. 

```json
// tsconfig.json
{
  "compilerOptions": {
    // ... 
    "types": ["vite-plugin-svgr/client"] // ✅ 
  },
}
```

ESLint 오류가 없어지지 않아서인지 구글링 하다 직접 별도의 `d.ts` 파일을 작성했던 것 같다!  
작성 후 `tsconfig.json` `include` 배열에 타입 선언 파일을 추가해 주었다. 

```ts
// svg.d.ts
declare module "*.svg" {
  const content: React.FC<React.SVGProps<SVGElement>>;
  export default content;
}
```

```json
// tsconfig.json
{
  "compilerOptions": {
    // ... 
    "types": ["vite-plugin-svgr/client"] // ✅ 
  },
  "include": ["src", "svg.d.ts"], // ✅ 
}
```

## 더 나아가 
### SVG 최적화 
벡터 기반 디자인 프로그램(Adobe Illustrator)에서 SVG 코드로 내보내면 실제로 렌더링을 위한 정보 이외에도 불필요한 속성들을 포함하고 있어 성능을 저해할 수 있다. [SVGO](https://github.com/svg/svgo)와 같은 라이브러리를 사용하여 SVG 파일을 최적화할 수 있다. 

[SVG 스프라이트](#svg-스프라이트-use-태그-사용하기), 상단에서 언급한 `Spritebot`은 SVGO 라이브러리를 포함하는 설치형 GUI SVG 스프라이트 생성기이다. 

### 지불하는 비용 고려하기
[퍼포먼스 딥 다이브: 왜 SVG-in-JS는 안티 패턴일까?](https://kurtextrem.de/posts/svg-in-js#performance-deep-dive-why-svg-in-js-is-an-anti-pattern)

위의 필자의 말처럼 "자바스크립트 파싱과 컴파일이 공짜가 아니다."  

> Byte-for-byte, 자바스크립트는 동일한 크기의 이미지나 웹 글꼴보다 브라우저에서 처리하는 데 더 많은 비용이 든다 — Tom Dale [web.dev](https://web.dev/articles/optimizing-content-efficiency-javascript-startup-optimization#parsecompile)

다운로드가 완료된 후 자바스크립트의 가장 큰 비용 중 하나는 JS 엔진이 이 코드를 파싱/컴파일하는 시간이라고 한다.  
이와 같은 맥락에서 한 번 읽어볼 필요가 있는 아티클이라는 생각이 든다. 


### References 
[Which SVG technique performs best for way too many icons?](https://cloudfour.com/thinks/svg-icon-stress-test/)    
[The "best" way to manage icons in React.js](https://benadam.me/thoughts/react-svg-sprites/)  
[Breaking Up with SVG-in-JS in 2023](https://kurtextrem.de/posts/svg-in-js)  
[(번역) 2023년 SVG-in-JS와 결별](https://github.com/yeonjuan/dev-blog/blob/master/JavaScript/breaking-up-with-svg-in-js-in-2023.md)
