---
title: 'Node_modules 없이 node를 사용할 수 있도록 해주는 yarn berry'
date: 2022-03-15 16:21:13
category: 'development'
draft: false
---

<h3 style="color:#0230">yarn berry를 사용하는 이유가 뭘까?</h3>

<br />

<h4 style="color:#000">1. node_modules의 거대함과 depth의 문제</h4>

간단한 패키지를 몇 개 설치하지 않아도 node_modules의 depth와 용량은 엄청나게 커지게 된다. <br />
더군다나 next.js등과 같이 다양한 기능을 제공하는 프레임워크, 라이브러리를 사용하게 된다면 <br />
최소 몇백메가에서 기가 단위의 디스크 용량을 사용하게 됩니다. <br />
거기 더해서 depth는 점점 깊어지는 문제가 발생하게 되고, 이것은 설치, 삭제, 수정, 실행 등의 속도에 영향을 끼치게 되고 <br />
패키지의 내용은 확인하지 못하고 기본적인 의존성 트리만 검증하게 된다.

<h4 style="color:#000">2. 유령 의존성</h4>

유령 의존성(phantom dependency)는 중복 라이브러리의 위치 때문에 발생하는 현상이다.<br />
유령 의존성은 package.json에 명시하지 않은 라이브러리를 사용할 수 있는 것이다. 이 라이브러리는 버전이 명확하지 않기 때문에, 다른 <br />사용자에게는 버전 호환 실패로 인한 오류를 발생시킬 수 있다<br />

<h4 style="color:#000">3. 모듈 재설치의 번거로움</h4>

협업을 하는 개발자라면 항상 느끼는 문제..보통 프로젝트 진행시, node_modules은 용량이 커서 gitignore에 포함하여 github에는 올리지 <br />않을 것이다. 이 때문에 협업시 git pull 하여 작업을 이어하게 되면 매번 npm 또는 yarn 명령어를 사용하여 node_modules 파일을 <br />재설치해야하는 번거로움이 있다.

<h3 style="color:#0230">yarn berry의 기능</h3>

Plug n Play 라는 개념을 사용하여 기존 문제점을 해결한다. pnp는 버전, 위치, 의존성 등을 담고 있다.<br />
node_modules를 생성하지 않고 기본 캐시 폴더에 zip파일로 저장해서 경로를 .pnp.js에 명시<br />

pnp제공하는 자료구조를 이용하여 위치를 찾는다. <br />

패키지 중복설치가 되지 않는다.<br />

zero-install을 사용하면 라이브러리 설치 없이 사용가능<br />
Yarn Berry에서 의존성을 버전 관리에 포함하는 것을 Zero-Install, 용량과 파일의 숫자가 적기 때문에 의존성을 git으로 관리 가능<br />

(https://yarnpkg.com/features/zero-installs)

<br />

https://toss.tech/article/node-modules-and-yarn-berry
