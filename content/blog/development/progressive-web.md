---
title: 'Progressive Web Apps'
date: 2022-03-24 16:21:13
category: 'development'
draft: false
---

### Progressive Web App?

Progressive Web App은 일반적인 웹 기술로 구현된 Web App에 Native Mobile App(iOS, Android)만이 <br />
제공해왔던 app push, background sync 등의 주요 기능 등을 점진적(progressively)으로 적용하여 App을 향상시키는 것을 말한다. <br />

보통 일반적인 웹 기술로 만들어진 웹 앱은 브라우저에서 실행되고, 사용자가 브라우저를 나가게 되면 앱의 기능은 상실하게 된다.<br />

그래서 app push를 이용한 알림을 사용하거나 모바일 장치 하드웨어에 접근하려면, Native Mobile App(iOS, Android) 또는 Hybird Mobile App(Web + Native)을 따로 구현했어야 했다.<br />

또한, 웹 앱은 네트워크가 항상 연결되어 있어야하며, 오프라인에서는 제대로 동작하지 않았다.<br />

그러나 이제는 크롬을 비롯한 많은 브라우저들이 PWA 기능을 지원하기 때문에 웹 앱에서도 app push, background sync, offline에서도 동작하는 등의 다양한 기능을 사용할 수 있게 되었다.<br />

물론 IE(Internet Explorer) 와 같은 구형 brower는 지원하지 않는다. <br />

### Progressive Web App의 장점

Native App을 만들려면 플랫폼 환경에 맞는 언어(Android : Java, Kotlin / iOS : objective-C, swift)를 배워야한다. <br />

물론, Hybrid App(Web + App) 형태로 개발하면, 비교적 Native App 언어 비중이 적어지기는 하나 아예 없어지는 것이 아니다. <br />

flutter 등의 프레임워크를 이용하면 플랫폼에 상관없이 개발을 할 수 있으나 javascript에 익숙한 기존의 웹 개발자에게는 접근성이 높은 편이다. <br />

PWA는 다른 언어나 프레임워크를 필요로 하는 것이 아니라 javascript 1개의 언어로 플랫폼(iOS, Android)에 상관없는 App을 만들 수 있어서 <br />웹 개발자들이 쉽게 접근할 수 있으며, 기존에 개발된 Web App에도 쉽게 적용할 수 있다. <br />

또한, Vue.js, React.js, Ember.js, Angular.js 등의 client side framework에도 모두 PWA를 지원하기 때문에 손쉽게 PWA를 사용하여 개발을 할 수 있다. <br />

Native App과 Mobile Web의 사용률을 비교한 통계 자료를 보면 Native App의 사용률이 Mobile web에 비해 6.7배 정도 높다고 한다.<br />

Native App에서 일반적인 Web App이 제공하지 않는 여러 기능을 제공하기 때문이다.<br />

하지만, Native App 사용자는 자신이 사용하는 주요한 App 몇 개만 주로 사용하며, 신규로 App을 설치하는 빈도는 매우 낮다.<br />

필요한 App을 설치하는데도 Google play store 나 Apple app store에 들어가서 찾고 설치해야 하기 때문에 여간 번거로운 작업이 아니다.<br />

그에 비해 PWA에서는 home screen icon 추가 기능(바로가기 아이콘 유사)을 제공하기 때문에 App store 접속없이 브라우저로 접근하여 손쉽게 App을 설치할 수 있다.<br />

웹서핑 도중 자연스럽게 App에 대한 노출도를 높임으로써 새로운 설치 가능성을 높여준다.<br />

또한, PWA는 Native App이 제공하는 대부분의 기능을 제공한다.<br />

따라서, PWA는 Mobile Web과 Native App의 장점을 모두 가졌으며, Mobile Web의 사용자와 Native App의 사용자를 모두 흡수할 수 있는 기술이다.<br />

### Progressive Skill

Manifest : Homescreen에 icon을 생성하는 기능을 제공함으로써 App store를 통하지 않고도 Web App을 install 가능하게 해준다.<br />

Service Worker : Service Worker는 client의 browser에 설치되며 background process로 동작하는 script code.<br /> 서버로 가는 request를 가로채는 proxy로 작동하기 때문에 web app에 다양하고 유용한 기능을 제공한다. -> background process에서 <br />작동하므로 App이 종료된 상태에서도 작동한다.<br />

Responsive Design : web page 속도 및 기능, 사용자 편의성 증대를 위해 다양한 Devices에서 반응형 Layout을 제공하도록 반응형 디자인 기술이 제공된다.<br />

Background Sync : Background Sync를 통해 인터넷이 끊긴 상태에서 발생하는 request를 저장했다가 인터넷이 활성화되면 해당 요청을 전송하는 기능을 제공한다.<br />

Push Notifications : App이 닫힌 상태에서도 Push notification을 수신할 수 있다.<br />

Media API : Device Camera와 Device Microphone에 접근 가능하게 해줍다.<br />

Geolocation API : User Location 정보에 접근 가능하게 해준다.<br />
