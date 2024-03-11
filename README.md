# Proxy Chain 설정하기

<br/>

반갑습니다! <br/>

오늘은 웹 모의해킹을 하기 앞서 Proxy Chain을 설정하는 법에 대해 기술하겠습니다. <br/><br/>

## ⚠️시작하기 앞서...

해당 Repository에 기술된 내용 및 컨텐츠는 "모의해킹" 및 "버그 바운티" 유저들을 위해 제공되었습니다.<br/>

이 글은 웹 해킹 및 기타 컴퓨터 기반의 불법 행위를 조장하는 글이 아니며, <br/>

특정 기업이나 시스템이 지정한 scope 밖에서의 침투 행위는 명백한 불법임을 사전에 밝힙니다.<br/>

제가 기술하는 모든 방법론과 configuration은 윤리적 모의해킹 및 버그 바운티를 통해 <br/>

조금 더 안전하고 윤택한 IT 환경을 만들고자 시작하게 되었음을 알립니다. <br/>

또한 해당 리포의 모든 컨텐츠 저작권은 "HamsterJikJik" 본인에게 있음을 고지드립니다.<br/><br/>


## 프록시란??

프록시란 특정 네트워크에서 클라이언트로부터 요청을 받아 중계하는 "서버"입니다. <br/>

한마디로 프록시는 일종의 서버라는 것이죠. <br/>

서버는 클라이언트로부터 온 요청에 응답해주는 일종의 전화 교환원이라고 생각하면 됩니다. <br/>

프록시 서버의 특징으로는 요청된 내용을 캐시 형태로 저장하는데, <br/>

여기서 캐시란 컴퓨터 기억장치에 관한 기술로, 컴퓨터가 최근에 사용한 메모리 값 일부를 저장하는 것입니다. <br/>

캐시를 활용하면 통신 시간을 절약할 수 있고 이미 저장되어 있는 값이 있기 때문에 <br/>

불필요한 외부 통신을 하지 않아도 되는 장점이 있습니다. <br/><br/>


## 웹 모의해킹에 프록시가 왜 필요하죠??

이 프로젝트롷 우리가 얻고자 하는 바는 우리가 모의침투를 할 때 추적 당하지 않기 위함입니다. <br/>

흔히 Directory Fuzzing이나 다양한 Brute Forcing을 사용할 때, 견고한 웹 서버라면<br/>

짧은 시간 안에 많은 요청을 보내는 특정 IP 주소에 Flag를 세우고 결국 차단하도록 설계 되어 있을 겁니다. <br/>

하지만 우리가 한 개, 혹은 여러 개의 프록시 서버를 통해 침투자의 IP 주소를 변조한다면 어떻게 될까요? <br/><br/>

- 첫째, 우리 머신이 현재 사용 중인 Legitimate한 IP 주소를 웹 서버로부터 숨길 수 있습니다.

- 둘째, 설령 변조한 IP 주소에 Flag가 세워지더라도 다른 IP 주소를 손쉽게 할당하여 침투를 이어나갈 수 있습니다.

<br/>

물론 작정하고 Law Enforcement에서 수사하면 여전히 감옥 갈 수 있습니다. <br/>

그니까 제발 scope 안에서 버그 바운티만 하세요. <br/>

뻘짓하다 잡혀가면 책임 안 집니다. <br/>

<br/>
<br/>

## Diagram

![Image 1](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/c6e478af-e6fd-4888-b364-8b7dae812cab?raw=true)

<br/>

일반적인 경우에서 클라이언트가 서버와 통신하는 방법입니다.


















