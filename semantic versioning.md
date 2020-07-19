# [NPM] semantic versioning

> [npm: about semantic versioning](https://docs.npmjs.com/about-semantic-versioning)



## 패키지 버전 앞의 `^` 는 뭐지?

![package.json dependencies screetshot](/Users/woowahan30/Documents/deep_dive/images/image-20200719231330726.png)

NodeJS 로 작업을 하면서 `npm install [패키지명]` 으로 새로운 패키지를 설치하면 설치 옵션에 따라 `package.json` 의 *dependencies* 나 *devDependencies* 에 패키지명과 버전 정보가 추가된다. 

위의 이미지에서 `"body-parser": "^1.19.0"` 과 같이 버전 정보 앞에 `^` (caret) 이 함께 기재되어 있는데 여기엔 어떤 의미가 있는걸까? (+ 검색을 하다보니  `^` 말고도 `~` (tilde) 같은 것도 있다고 한다.)

## semantic versioning?



![semantic versioning](/Users/woowahan30/Documents/deep_dive/images/image-20200719231531573.png)출처: [GeeksForGeeks](https://www.geeksforgeeks.org/introduction-semantic-versioning/) 



`^` 과 `~` 에 대해 알아보려면 **semantic versioning** 에 대한 이해가 필요하다.

**semantic versioning**  소프트웨어의 버전을 나타내기 위한 방법으로  `xx.xx.xx` 의 형식을 사용하는데, `.` 으로 구분되는 각 숫자가 서로 다른 버전 정보를 의미한다.

소프트웨어가 처음 release 될 때는 `1.0.0` 버전으로 시작하게 되고, 다음과 같은 경우에 해당 버전을 의미하는 숫자를 증가시킨다.

- *Major version* (제일 첫 번째 숫자) : 기존 버전과 호환되지 않을 정도의 변화가 있는 경우
- *Minor version* (가운데 숫자) : 기존 버전과의 **호환성을 유지**한 채로 새로운 기능이 추가된 경우
- *Patch* (마지막 숫자) : 기존 버전과 기능에서 발생하는 버그를 수정한 경우



## 다시 package.json 으로

dependencies 와 devDependencies 에서 버전을 표기할 때  `^` , `~` 를 함께 사용하면 `npm install` 을 통해 패키지를 설치할 때에 수용 가능한 버전의 폭을 넓혀준다. `^` 는 해당 버전보다 최신의 minor version, patch version 을 모두 허용하고, `~` 는 해당 버전보다 최신의 patch version 을 허용한다. `npm install` 은 이렇게 제공되는 정보에 따라 허용되는 버전 중에서 가장 최신 버전을 설치한다.

- `dependencies : { "express" : "*"}`: 모든 버전 허용



- `dependencies : { "express" : "3"}`: Major version 이 3인 모든 버전 수용
- `dependencies : { "express" : "^3"}` : 위와 동일 (minor version 이하가 표기되어있지 않기 때문)
- `dependencies : { "express" : "~3"}` : 위와 동일 (patch version 이 표기되어있지 않기 때문)



- `dependencies : { "express" : "3.9"}` : Major version 이 3, Minor version 이 9 인 모든 버전 수용
- `dependencies : { "express" : "^3.9"}` : 3.9 부터 4.0 이전 까지의 버전 수용 (3.10.x 포함)
- `dependencies : { "express" : "~3.9"}` : "3.9" 와 동일 (patch version 이 표기되어있지 않기 때문)



- `dependencies : { "express" : "3.9.2"}`: 3.9.2 버전만 수용
- `dependencies : { "express" : "^3.9.2"}`: 3.9 부터 4.0 이전 까지의 버전 수용 (3.10.x 포함)
- `dependencies : { "express" : "~3.9.2"}`: 3.9.2 부터 3.10.0 이전까지의 버전 수용



- 정리하자면, patch version 에 대해 유연성을 갖는 `~` 을 사용하는 경우,  `~a.b.c` 는 해당 버전보다 상위 patch version 은 모두 수용, minor version 에 대해 유연성을 갖는 `^` 을 사용하는 경우, `^a.b.c` 는 해당 버전보다 상위 patch version 또는 minor version 까지 수용한다는 의미가 된다. 
- [npm semver calculator](https://semver.npmjs.com/) 에서 직접 확인 가능!



## 패키지 설치할 때 버전 선택에 활용할수도!

프로젝트 내에서 패키지를 처음 설치할 때에도 설치하고자 하는 패키지의 정확한 버전 정보를 알지 못할 때에는 다음과 같이 `^` 을 사용하면 원하는 Major version 내에서 가장 최신 버전을 설치할 수 있게 된다.

- `npm install express@^3.0.0` : 3.x.x 버전 중 제일 최신 버전 설치
- `npm install express@3.0.0` : 3.0.0 버전 설치

위와 같이 별도의 옵션 없이 `npm install` 명령어를 사용해서 패키지를 설치하게 되면 `package.json` 에는 기본적으로 `^` 를 이용한 버전이 표기된다. 생각해보면 Minor version 은 기존 버전들과의 호환성이 유지되기 때문에 호환성을 깨트리지 않는 내에서 가장 최신의 패키지를 사용할 수 있기 때문에 이러한 설정이 합리적이라고 생각된다.

패키지를 설치할 때 `^` 를 사용해서 최신 버전을 설치했더라도 `--save-exact` 옵션을 함께 사용하면 `package.json`에는 내 프로젝트에 사용된 해당 패키지의 정확한 버전 정보를 기재할 수 있다고 하는데, 이 경우에는 해당 `package.json` 정보로는 프로젝트가 의존하고 있는 패키지의 상위 minor version, patch version 이 release 되었을 때 자동으로 패키지를 업데이트하기가 어려워진다.

