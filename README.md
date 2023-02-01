# **웹팩**

자바스크립트로 개발을 하다보면 코드의 재사용과 유지보수 측면에서 파일을 여러개로 분리해서 개발하고 한다. 분리한 파일을 모듈이라고 한다.

모듈을 계속해서 분리하다보면 브라우저에서 서버에 요청하는 개수가 많아지고 네트워크 로딩 시간이 길어져서 사용자 경험에 좋지 않다.

개발을 할 때는 모듈로 나눠서 개발을 하고 배포하기 전에 모듈들을 하나의 파일로 묶어서 배포한다.

⇒ 서버에 요청하는 개수가 줄어들기 때문에 사용자의 경험 향상

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7ca570ab-4b16-4b30-aee3-67462b511814/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095902Z&X-Amz-Expires=86400&X-Amz-Signature=72c37992d28ba9147a24e75e711d655f28c24fd3c8f538e65cf04c309a9dafc2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7ca570ab-4b16-4b30-aee3-67462b511814/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095902Z&X-Amz-Expires=86400&X-Amz-Signature=72c37992d28ba9147a24e75e711d655f28c24fd3c8f538e65cf04c309a9dafc2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

번들링: 하나의 파일로 묶는 작업

**번들러 : 여러 개의 파일을 모듈화 해서 하나의 파일로 묶어주는 도구.**

**(대표적으로 webpack, parcel, browserify가 있다. 요즘은 vite가 핫하다고 함)**

## 웹팩 이전의 모습

`import`를 통해서 모듈을 가져올 수 있다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8471c054-c2b4-43ae-8d19-2c2641cd9de4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095914Z&X-Amz-Expires=86400&X-Amz-Signature=9587dc713df2adf80b4e494f5c6f0fc26f314a679574da4d35bd289af764cf43&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8471c054-c2b4-43ae-8d19-2c2641cd9de4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095914Z&X-Amz-Expires=86400&X-Amz-Signature=9587dc713df2adf80b4e494f5c6f0fc26f314a679574da4d35bd289af764cf43&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

`index.html`에서 모듈 3개를 불러왔기 때문에 브라우저에서 3개의 모듈을 요청하게 된다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/12ca279a-f6fa-4ec4-a654-a74eb16056c0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095923Z&X-Amz-Expires=86400&X-Amz-Signature=0c3831d507015ac19c9e23af64b86e5576bdb92969919247dd8a7c37254546aa&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/12ca279a-f6fa-4ec4-a654-a74eb16056c0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095923Z&X-Amz-Expires=86400&X-Amz-Signature=0c3831d507015ac19c9e23af64b86e5576bdb92969919247dd8a7c37254546aa&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

브라우저에서 모듈이 많아지게 되면 비효율적이게 된다. 컨텐츠마다 소비되는 시간을 하나의 컨텐츠로 가져온다면 불필요한 시간을 줄일 수 있다.

웹팩은 오픈 소스 자바스크립트 **모듈 번들러**로써 여러개로 나누어져 있는 파일들을 하나의 자바스크립트 코드로 압축하고 최적화하는 라이브러리이다.

1. 여러 파일의 자바스크립트 코드를 압축하여 최적화 할 수 있기 때문에 로딩에 대한 네트워크 비용을 줄일 수 있다.
2. 모듈 단위로 개발이 가능하여, 가독성과 유지보수가 쉽다.
3. 최신 자바스크립트 문법을 지원하지 않는 브라우저에서 사용할 수 있는 코드로 쉽게 변환시켜준다.

## 웹팩 구성요소

### Entry

웹팩이 **빌드할 파일의 시작** 위치디폴트 값 : ./src/index.js

### Output

웹팩에 의해 생성된 **번들을 내보낼 위치와 이름** 지정디폴트값 : ./dist/main.js

### Loader

웹팩은 자바스크립트가 아닌 `png`나 `css` 같은것도 번들링해준다. 그것들을 다루는 것이 `loader`이다. 웹팩을 잘 다룬다는 것은 `loader`를 얼마나 많이 알고 있는가. 얼마나 잘 다루는가이다.

로더를 사용하여 webpack에 CSS 파일을 로드하거나 TypeScript를 JavaScript로 변환할 수 있습니다. 이를 위해서 필요한 로더를 설치해봅시다.

rules 프로퍼티를 정의해야 하며 그 안에 `text`와 `use`를 필수 프로퍼티로 가진다.

test : 변환해야 할 파일 식별화

use : 변환될 파일에 대해 필요한 로더를 명세

> 예시
> 

모든`.css`  파일에 `[css-loader](https://webpack.kr/loaders/css-loader)` 를 사용하고, `.ts`  파일에는 `[ts-loader](https://github.com/TypeStrong/ts-loader)` 를 사용하도록 webpack에 지시한다.

```jsx
npm install --save-dev css-loader ts-loader
```

```jsx
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/, // 확장자가 css인 파일을 나타내는 코드
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true,
            },
          },
          { loader: 'sass-loader' },
        ],
      },
    ],
  },
};
```

확장자가 css인 파일을 만나면 웹팩이 그 css인 파일을 웹팩안으로 로드시켜주는 특수한 명령이 css-lodaer이다.

index.html에서 link태그 필요없이 이제 index.js 파일에서 css를 통해 불러온다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f17a53f5-9c05-40d2-886f-77f348067c29/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095937Z&X-Amz-Expires=86400&X-Amz-Signature=580eb1cbb6e6a67895190fd074bbc29f1a24f29c10c04b8d70c0062562e8a3eb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f17a53f5-9c05-40d2-886f-77f348067c29/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095937Z&X-Amz-Expires=86400&X-Amz-Signature=580eb1cbb6e6a67895190fd074bbc29f1a24f29c10c04b8d70c0062562e8a3eb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

번들링을 하게 되면 entry파일인 index.js 파일을 해석하다가 확장자가 css인 파일을 css-loader에게 맡긴다. `css-loader`가 파일을 읽어서 css라는 변수 안에 세팅해준다.

주의! 뒤쪽에 있는 loader가 먼저 실행 된다.

### Plugins

loader는 우리가 갖고 있는 모듈을 아웃풋으로 만들어내는 과정에서 사용하는 것 플러그인은 그렇게 해서 만들어진 최종적인 결과물을 변환한다. 플러그인이 훨씬 자유롭다.

[로더](https://webpack.kr/concepts/loaders)가 할 수 없는 **다른 작업을**  수행할 목적으로 제공된다.

로더가 파일단위로 처리하는 반면 플러그인은 번들된 결과물을 처리한다.

번들된 자바스크립트를 난독화 한다거나 특정 텍스트를 추출하는 용도로 사용할 수 있다.

> 예시
> 

HtmlWebpackPlugin

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f110304f-48c8-4536-b5ca-3a934b12b376/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095950Z&X-Amz-Expires=86400&X-Amz-Signature=93a13e5f50dc66be0c4489fe9b97e9d833e32acfb55f6985839b8443fd9dae44&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f110304f-48c8-4536-b5ca-3a934b12b376/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T095950Z&X-Amz-Expires=86400&X-Amz-Signature=93a13e5f50dc66be0c4489fe9b97e9d833e32acfb55f6985839b8443fd9dae44&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

만든 파일들을 템플릿으로 해서 번들링 된 자바스크립트를 자동으로 추가해서 그 결과를 public 디렉토리 안에 넣어준다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4e90eee0-27ed-4b60-ab36-44fa8f9c306a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100002Z&X-Amz-Expires=86400&X-Amz-Signature=dc60dbf2e21e58ce9975372f0721f9088e507eac5a6fd223ec07c4d975f63d74&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4e90eee0-27ed-4b60-ab36-44fa8f9c306a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100002Z&X-Amz-Expires=86400&X-Amz-Signature=dc60dbf2e21e58ce9975372f0721f9088e507eac5a6fd223ec07c4d975f63d74&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### Loaders 와 Plugin 차이

Loaders는 번들이 생성되는 동안이나 생성되기 전에 개별 파일 수준에서 작업이되는 것이고 Plugin 은 번들이 생성 된 후에 작동한다

### Mode

**웹팩 설정 모드**로써 3가지의 파라미터 중 하나를 택할 수 있다.

production : 최적화 빌드

development : 빠른 빌드

none : 아무 기능 없이 빌드

## 웹팩 적용하기

### 웹팩 설치

```powershell
npm install --save-dev webpack webpack-cli
```

로컬 환경에 설치된 `cli`를 실행시키기 위해서 `npx` 명령어를 사용해야한다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cc5c6327-21b8-4a35-93c7-143eff138f0b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100015Z&X-Amz-Expires=86400&X-Amz-Signature=72371fa77836e7c76933a94ae6f5aa27794ae5ab7d6492b135704c916dc41606&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cc5c6327-21b8-4a35-93c7-143eff138f0b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100015Z&X-Amz-Expires=86400&X-Amz-Signature=72371fa77836e7c76933a94ae6f5aa27794ae5ab7d6492b135704c916dc41606&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

```powershell
npx webpack --entry ./src/index.js --output-path ./dist
```

### entry

웹팩의 진입점. 어떠한 파일을 기준으로 번들링을 할 것이냐!

이 파일을 기준으로 사용하고 있는 모듈을 추적한다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e902a907-da0e-4624-89af-a128c501c444/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100027Z&X-Amz-Expires=86400&X-Amz-Signature=c6098436f82a066970277e3947118ba8afa2810d8f63c3f1c707d8de6fb7dc05&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e902a907-da0e-4624-89af-a128c501c444/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100027Z&X-Amz-Expires=86400&X-Amz-Signature=c6098436f82a066970277e3947118ba8afa2810d8f63c3f1c707d8de6fb7dc05&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### output

`dist` 디렉토리를 생성한 다음 이 디렉토리 안에 번들링 할 파일을 위치하도록 한다.

`dist`폴더 안에 `main.js`가 생성이 된다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/30f83797-07ec-4464-afca-4e8f63eb8f54/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100039Z&X-Amz-Expires=86400&X-Amz-Signature=c7f1b90e895976a75e4e59bbcb9ae2debca9642b98a1abbc076364e6d0ffc4c5&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/30f83797-07ec-4464-afca-4e8f63eb8f54/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100039Z&X-Amz-Expires=86400&X-Amz-Signature=c7f1b90e895976a75e4e59bbcb9ae2debca9642b98a1abbc076364e6d0ffc4c5&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

번들링을 할 때 `produdction`이 적용이 되어서 압축이 되어있다.

```powershell
npx webpack --entry ./src/index.js --output-path ./dist --mode development
```

`development`를 적용하면 압축없이 생성이 되는것을 확인할 수 있다.

### 번들링 한 파일 가져오기

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3787f6cb-41a7-4703-89a3-fbdcf7dbad07/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100058Z&X-Amz-Expires=86400&X-Amz-Signature=8a35622e01a199d63156e32a55eb9de3958a9d7c4b9062ec797d8531efa0240a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3787f6cb-41a7-4703-89a3-fbdcf7dbad07/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100058Z&X-Amz-Expires=86400&X-Amz-Signature=8a35622e01a199d63156e32a55eb9de3958a9d7c4b9062ec797d8531efa0240a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

네트워크 탭을 보게 되면 아까와는 다르게 `main.js` 하나만을 가져온 것을 알 수 있다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ee6a6af9-e145-4dd6-b1e5-27c0c1a847a8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100112Z&X-Amz-Expires=86400&X-Amz-Signature=525208a3e87dd3d8c4cb6f8e12ea2d69dbe2b4fe91a80f8edc6161775983880d&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ee6a6af9-e145-4dd6-b1e5-27c0c1a847a8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100112Z&X-Amz-Expires=86400&X-Amz-Signature=525208a3e87dd3d8c4cb6f8e12ea2d69dbe2b4fe91a80f8edc6161775983880d&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

웹팩 이전

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7c5fbc9d-7b58-44d7-a0b6-d8b486adce29/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100131Z&X-Amz-Expires=86400&X-Amz-Signature=99382603f3ba115924308a3b355efd2176282a7c3f6aa3cc7b96bc885c06c901&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7c5fbc9d-7b58-44d7-a0b6-d8b486adce29/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100131Z&X-Amz-Expires=86400&X-Amz-Signature=99382603f3ba115924308a3b355efd2176282a7c3f6aa3cc7b96bc885c06c901&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

번들링 후

실제 운영 환경에서는 엄청난 차이가 있을것이다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8d44db22-0590-45f6-838e-02a7fb6c6ddd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100141Z&X-Amz-Expires=86400&X-Amz-Signature=d3a40b162bf21191ddad3694b932bab4d8b269669ce694a40956f1933629b569&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8d44db22-0590-45f6-838e-02a7fb6c6ddd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100141Z&X-Amz-Expires=86400&X-Amz-Signature=d3a40b162bf21191ddad3694b932bab4d8b269669ce694a40956f1933629b569&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

## 환경설정 파일을 사용해서 webpack 적용하기

webpack은 실행할 때 자동으로 `webpack.config.js` 파일을 참고한다.

```jsx
const path = require('path');

module.exports = {
	mode: 'production', // development 모드 지정가능
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
};
```

entry: 진입점을 설정하는 속성

output: 번들링 파일이 위치할 `path`와 `filename`을 지정한다.

```jsx
npx webpack
// npx webpack을 하게 되면 webpack.config.js 파일을 참고하게 된다.
```

`bundle.js` 파일이 `dist` 폴더 안에 생성이 된다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fd820fbe-b55f-4ea2-b583-0208bee92383/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100152Z&X-Amz-Expires=86400&X-Amz-Signature=8c7b149772a24c6573a27bd7204fb172ce1f8bf54bb70e124d053ac9253ff894&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fd820fbe-b55f-4ea2-b583-0208bee92383/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100152Z&X-Amz-Expires=86400&X-Amz-Signature=8c7b149772a24c6573a27bd7204fb172ce1f8bf54bb70e124d053ac9253ff894&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

`bundle.js` 파일로 `index.html`에서 다시 지정해준다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8d1e79dc-f59b-4b6f-baee-697177aaa19a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100200Z&X-Amz-Expires=86400&X-Amz-Signature=388082f7639de947fb4e2e3db6508fcc59faff994f99e21620a0ca1774efd7e9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8d1e79dc-f59b-4b6f-baee-697177aaa19a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100200Z&X-Amz-Expires=86400&X-Amz-Signature=388082f7639de947fb4e2e3db6508fcc59faff994f99e21620a0ca1774efd7e9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

번들링 된 하나의 파일로 가지고 올 수 있다.

번들링하는 명령어는 자주 사용하는 명령어기 때문에 보통 `package.json`에서 `build` 명령어로 추가하기도 한다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/86b230a8-c306-44b1-af45-aac3246c0941/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100216Z&X-Amz-Expires=86400&X-Amz-Signature=2be0c1b68ea2d840344e21aa365e6d2742ab56494dc8617873cf2489260038a6&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/86b230a8-c306-44b1-af45-aac3246c0941/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100216Z&X-Amz-Expires=86400&X-Amz-Signature=2be0c1b68ea2d840344e21aa365e6d2742ab56494dc8617873cf2489260038a6&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

이제 `npm run build`를 하면 번들링을 할 수 있다!

## CRA 속 Webpack

```jsx
npm run eject
```

CRA에서 제공하는 명령어인 `eject` 를 사용한다. `eject`는 해당 프로젝트에 걸려서 숨겨져 있는 모든 설정을 밖으로 추출해주는 명령어다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/54666203-2ec7-43f9-ac5b-cad638726642/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100227Z&X-Amz-Expires=86400&X-Amz-Signature=4b0cc91363b90cece1841b21a81267dcaba4f7a8fd2fa65b1f04e9b3644ba420&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/54666203-2ec7-43f9-ac5b-cad638726642/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100227Z&X-Amz-Expires=86400&X-Amz-Signature=4b0cc91363b90cece1841b21a81267dcaba4f7a8fd2fa65b1f04e9b3644ba420&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

## webpack.config.js

`eject` 후에 생긴 `config` 폴더 안에 들어있는 웹팩 설정 파일이다. 

파일의 상단에는 빌드에 필요한 경로들에 대한 변수를 미리 지정해놓은 `paths.js`를 부르고 있고요, `module`에서 사용할 정규표현식들을 미리 선언되어있다. 

이 설정파일에서는 기본적으로 익명 함수를 `export`하고 있는데요, 이 함수는 `webpackEnv` 인자에 따라 웹팩 설정 객체를 리턴합니다.

```jsx
// 로더에 쓰이는 정규표현식들 미리 선언
const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;

// 익스포트되는 모듈 함수로 적용 => 환경에 따라 다른 웹팩 설정 객체를 리턴
module.exports = function (webpackEnv) {
// development 모드와 production 모드의 웹팩 설정이 다름
  const isEnvDevelopment = webpackEnv === "development";
  const isEnvProduction = webpackEnv === "production";
  ...
  return {
  // webpackEnv 인자에 따라 mode가 달라짐
  mode: isEnvProduction ? "production" : isEnvDevelopment && "development",
  ...
  }
}
```

### entry

`paths.appIndexJs` 를 웹팩 빌드가 시작되는 진입점으로 설정합니다. 같은 폴더에 있는 `paths.js` 폴더에서 이 변수가 리액트 컴포넌트의 시작점이 되는 `src/index.js` 파일을 가리키고 있다는 것을 알 수 있습니다. `react-dev-utils`는 CRA에서 사용하는 웹팩 유틸리티인데, 다른 엔트리는 웹팩 dev server와 관련이 있는 것으로 보입니다. production 모드로 빌드될때는 entry 배열에 `src/index.js`만 남습니다.

```jsx
// 같은 폴더 내부의 paths.js에 빌드에 필요한 경로들 미리 설정
const paths = require("./paths");

entry: [
  // development 모드에서만 실행
  isEnvDevelopment && require.resolve("react-dev-utils/webpackHotDevClient"),
  // src/index.js를 진입점으로 설정
  paths.appIndexJs,
].filter(Boolean), // production 모드로 빌드되었을 때 false를 없애는 역할
```

### output

output의 `path`는 `build`폴더로 설정이 되어 있습니다. production 모드일때만 `build` 폴더가 생깁니다. production 모드일때는 번들링되는 파일 이름인 `[name]` 에 `[contenthash:8]`, 해쉬코드가 붙습니다. development 모드에서는 해쉬코드가 붙지 않습니다. `publicPath` 는 브라우저에서 참조될 때의 출력 파일의 공용 URL 주소를 지정합니다.

```jsx
output: {
  // paths.appBuild는 'build', 기본 빌드 폴더 이름이 'build'
  path: isEnvProduction ? paths.appBuild : undefined,

  // 모듈에 대한 정보가 포함된 주석으로 표시하는 옵션. production모드에서는 사용하지 않는다
  pathinfo: isEnvDevelopment,

  // production 빌드시에는 파일과 청크에 네임과 HashCode가 붙는다
  filename: isEnvProduction
    ? "static/js/[name].[contenthash:8].js"
    : isEnvDevelopment && "static/js/bundle.js",
  futureEmitAssets: true,
  chunkFilename: isEnvProduction
    ? "static/js/[name].[contenthash:8].chunk.js"
    : isEnvDevelopment && "static/js/[name].chunk.js",

  // public path를 설정해준다
  publicPath: paths.publicUrlOrPath,
      ...
}
```

### module

파일을 처리하는데 필요한 `loader`를 선언합니다. `loader`들은 module의 rules 배열안에 정의합니다. `test`는 로더를 적용할 파일명의 특징을 정규표현식으로 지정합니다. `exclude`에서는 처리 대상에서 제외할 파일명 규칙을 작성합니다. `loader`에서 로더를 불러옵니다. 살펴보니 CRA에서는 기본적으로 css, sass, eslint, url, babel, file 로더를 사용하고 있습니다.

```jsx
module : {
  rules [
    {
      test: /\.(js|mjs)$/,
      exclude: /@babel(?:\/|\\{1,2})runtime/,
      loader: require.resolve("babel-loader"),
      options: {
      ...
      },
    },
  ...
  ]
}
```

### plugins

플러그인을 빌드 과정에 적용합니다. 플러그인은 클래스로 구현되어 있으므로`plungins` 배열 안에다가 `new` 연산자를 사용해 새로운 익명 객체를 만들어주면 플러그인을 적용할 수 있습니다.

### CRA는 마냥 좋은 것일까?

정말 CRA는 고마운 존재..인줄 알았으나,사실 CRA의 문제점은 **사용하지 않는 설정이나 라이브러리까지 Overloading**되어 있다는 것이다.따라서 번들이 무거워져 빌드 시간이 길어질 수 있고,CRA 사용시 **웹팩을 커스텀하기 어려워진다는 단점**도 갖게 된다.

## 정리

웹팩은 배포를 하기 전에 번들링을 해주는 도구이다. 웹팩은 자바스크립트 뿐만 아니라 이미지, css등 다양한 데이터를 변환할 수 있다.

웹팩은 여러개의 파일들을 하나의 파일로 묶어주는 모듈 번들러이다.

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/154ac543-54da-4f4f-8aa9-85331a3b18fb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100248Z&X-Amz-Expires=86400&X-Amz-Signature=44dd57b23a3a9526bcbaeb1aac6c4bd305dde5b8d17aba60763049e46626e3d5&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/154ac543-54da-4f4f-8aa9-85331a3b18fb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230201%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230201T100248Z&X-Amz-Expires=86400&X-Amz-Signature=44dd57b23a3a9526bcbaeb1aac6c4bd305dde5b8d17aba60763049e46626e3d5&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

이러한 모듈 번들러는 프론트에서 서버로 요청을 할 때 http 요청 개수를 줄여줌으로써 퍼포먼스를 확장하고 리소스를 최적화한다. 그래서 이러한 모듈 번들러를 사용하게 되면 사용자 경험을 향상시킬수 있다!

웹팩은 `entry`로 설정된 시작점에서 의존성을 가진 모든 파일을 압축하고 `output` 지점에 하나의 자바스크립트 파일을 만들어 준다. 

이때, 자바스크립트가 아닌 파일은 `loaders`를 이용하여 자바스크립트에서 이용가능한 모듈로 만들어 준다.

`plugins`를 이용하여 번들된 자바스크립트를 난독화하거나 특정 텍스트를 추출하는 역할을 한다. 

`mode`는 웹팩의 사용 목적에 따라 설정을 지정하는 역할을 한다.

# References

[webpack](https://webpack.kr/)

[Create-React-App의 Webpack 기본 설정 살펴보기](https://maxkim-j.github.io/posts/cra-webpack-config/)

[[JS][WEBPACK] 1. 웹팩이란 무엇인가](https://medium.com/@woody_dev/js-webpack-1-%EC%9B%B9%ED%8C%A9%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-f29ebca31da4)

[웹팩의 기본 개념](https://jeonghwan-kim.github.io/js/2017/05/15/webpack.html)

[[React] CRA 쓰지 않고 리액트 개발환경 구축 | webpack.config.js](https://pak-fuse.tistory.com/47)

[Webpack - 6. 로더의 도입](https://www.youtube.com/watch?v=5ym8ozubCTA&list=PLuHgQVnccGMChcT9IKopFDoAIoTA-03DA&index=6&ab_channel=%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9)

[Webpack 기초 강좌 | 웹팩 이란 | 모듈번들러 | 프론트엔드 날개달기](https://www.youtube.com/watch?v=NGVc-zw2FG8&list=PLlaP-jSd-nK-nRWstWs_MvCWhVHb7Auuc&index=18&ab_channel=%EC%A7%90%EC%BD%94%EB%94%A9%EC%9D%98CODINGGYM)
