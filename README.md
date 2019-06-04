# FED-styling-react

## 리액트 컴포넌트 스타일링 방식 : CSS, Sass, CSS Module, styled-components 

### 1. CSS방식
- 기존의 CSS 스타일링에 있어서 불편함을 느끼지 안흥면 그대로 사용해도됨. (우리가 사용하고있는 CSS 방식임.)

### 2. Sass방식
- Sass는 CSS pre-processor 로서, 복잡한 작업을 쉽게 할 수 있게 해주고, 코드의 재활용성을 높여줄 뿐 아니라, 코드의 가독성을 높여주어 유지보수를 쉽게 해준다.
- Sass 에서는 두 가지의 확장자 (.scss/.sass)를 지원합니다.
- 보통 scss문법이 더 많이 사용된다.
- 변수 사용이 가능하다.

#### .sass 문법
```
$font-stack: Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```
  
  #### .scss 문법
 ```
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```
#### 2-1. Sass 라이브러리 설치
``` npm install node-sass``` -> npm, 
``` yarn add node-sass``` -> yarn

#### 2-2. utils 함수 분리하기.
- 자주 사용 될 수도 있는 Sass 변수 및 믹스인을 따로 파일로 분리하여 사용한다.
- .scss 파일에서 다른 scss 파일을 불러올 땐 **@import** 구문을 사용해야 한다.

#### 2-3 sass-loader 설정 커스텀마이징
- Sass 를 사용 할 때 필수적인 작업은 아니지만, 해두면 좋을 수도 있는 작업이다.(프로젝트에 디렉토리를 깊숙한 구조를 만들 때 유용함!
ex) import'../../styles/utils.scss' 이런 식으로 거슬러 올라갈 때 유용함!!)
- 위의 문제점을 해결하는 방법은 Webpack 에서 Sass를 처리하는 sass-loader 의 설정을 커스텀마징하는 것이다.
- 이를 커스텀마이징 하기 위해서는 ```npm run eject``` 또는 ```yarn eject``` 명령어를 통하여 세부설정을 다시 밖으로 꺼내주어야 함.
- **주의: 반드시 커밋을 해주고 나서 해야한다!! (커밋을 먼저 안하니까 에러가 뜬다..!!)**
- 명령어를 실행 후 config 경로가 플젝에 생기는데, 그 안에 webpack.config.js 를 오픈
- sassRegex를 찾고 **use:** 부분을 
```
  {
              test: sassRegex,
              exclude: sassModuleRegex,
              use: getStyleLoaders({
                importLoaders: 2,
                sourceMap: isEnvProduction && shouldUseSourceMap
              }).concat({
                loader: require.resolve('sass-loader'),
                options: {
                  includePaths: [paths.appSrc + '/styles'],
                  sourceMap: isEnvProduction && shouldUseSourceMap
                }
              }),
              // Don't consider CSS imports dead code even if the
              // containing package claims to have no side effects.
              // Remove this when webpack adds a warning or an error for this.
              // See https://github.com/webpack/webpack/issues/6571
              sideEffects: true
            },
            
```
 위와 같은 문자열로 교체한다! 작성 후, 서버를 껏다가 재시작.
 - scss 파일 경로가 어디에 위차하더라도 , 앞부분에 상대경로를 입력 할 필요 없이 styles 디렉토리 기준 절대경로로 불러올 수 있다.
 ex) ```@import 'utils.scss'; ```
 
 #### 2-4 classNames
 - classNames 는 CSS 클랫그를 조건부로 설정 할 때 매우 유용한 라이브러리입니다.
 - classNames 설치
 ```npm install classnames``` -> npm , ```yarn add classNames``` -> yarn
 - props의 값에 따라 다른 스타일을 주는게 쉬워진다.
 ```
 const MyComponent = ({ highlighted, theme }) => {
  <div className={classNames('MyComponent', { highlighted }, theme)}> Hello </div>
 }
 ```
 이렇게하면 위 엘리먼트의 클래스로는 highlighted 값이 true 이나 false 에 따라 highlighted 라는 클래스가 적용될 것이고, 추가적으로 theme 으로 전달받은 문자열이 그대로 클래스에 적용될 것이다.

##### classNames 를 CSS Module 과 함께 쓸 때
- classNames를 불러올때 ```classnames/bind``` 를 사용하면 사전에 미리 ```styles``` 에서 받아와서 사용하게끔 설정해두고
```cx('class1', 'class2')``` 형태로 사용 할 수있게 된다.
