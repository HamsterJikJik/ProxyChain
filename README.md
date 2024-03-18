# Proxy Chain 설정하기

<br/>

반갑습니다! <br/>

예전에 해뒀던 프로젝트였지만 당시엔 깃허브를 사용하지 않고 있었기 때문에

깃허브 업데이트를 기념해서 오늘은 웹 모의해킹을 하기 앞서 Proxy Chain을 설정하는 법에 대해 기술하겠습니다. <br/>
<br/>
<br/>


## ⚠️시작하기 앞서...

해당 Repository에 기술된 내용 및 컨텐츠는 "모의해킹" 및 "버그 바운티" 유저들을 위해 제공되었습니다.<br/>

이 글은 웹 해킹 및 기타 컴퓨터 기반의 불법 행위를 조장하는 글이 아니며, <br/>

특정 기업이나 시스템이 지정한 scope 밖에서의 침투 행위는 명백한 불법임을 사전에 밝힙니다.<br/>

제가 기술하는 모든 방법론과 configuration은 윤리적 모의해킹 및 버그 바운티를 통해 <br/>

조금 더 안전하고 윤택한 IT 환경을 만들고자 시작하게 되었음을 알립니다. <br/>
<br/>
<br/>


## 프록시란??

프록시란 특정 네트워크에서 클라이언트로부터 요청을 받아 중계하는 "서버"입니다. <br/>

한마디로 프록시는 일종의 서버라는 것이죠. <br/>

서버는 클라이언트로부터 온 요청에 응답해주는 일종의 전화 교환원이라고 생각하면 됩니다. <br/>

프록시 서버의 특징으로는 요청된 내용을 캐시 형태로 저장하는데, <br/>

여기서 캐시란 컴퓨터 기억장치에 관한 기술로, 컴퓨터가 최근에 사용한 메모리 값 일부를 저장하는 것입니다. <br/>

캐시를 활용하면 통신 시간을 절약할 수 있고 이미 저장되어 있는 값이 있기 때문에 <br/>

불필요한 외부 통신을 하지 않아도 되는 장점이 있습니다. <br/>
<br/>
<br/>


## 웹 모의해킹에 프록시가 왜 필요하죠??

이 프로젝트롷 우리가 얻고자 하는 바는 우리가 모의침투를 할 때 추적 당하지 않기 위함입니다. <br/>

흔히 Directory Fuzzing이나 다양한 Brute Forcing을 사용할 때, 견고한 웹 서버라면<br/>

짧은 시간 안에 많은 요청을 보내는 특정 IP 주소에 Flag를 세우고 결국 차단하도록 설계 되어 있을 겁니다. <br/>

하지만 우리가 한 개, 혹은 여러 개의 프록시 서버를 통해 침투자의 IP 주소를 변조한다면? <br/><br/>

- 첫째, 우리 머신이 현재 사용 중인 Legitimate한 IP 주소를 웹 서버로부터 숨길 수 있습니다.

- 둘째, 설령 변조한 IP 주소에 Flag가 세워지더라도 다른 IP 주소를 손쉽게 할당하여 침투를 이어나갈 수 있습니다.

<br/>

물론 작정하고 Law Enforcement에서 수사하면 여전히 감옥 갈 수 있습니다. <br/>

그니까 제발 scope 안에서 버그 바운티만 하세요. <br/>

뻘짓하다 잡혀가면 책임 안 집니다. <br/>

<br/>
<br/>

## Diagram

![Image 1](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/247375f1-0c2f-4e44-9681-4b581033708a?raw=true)

<br/>

일반적인 경우에서 클라이언트가 서버와 통신하는 방법입니다. <br/>

왼쪽의 클라이언트가 만약 프록시 서버를 거치지 않고 서버와 통신한다면 <br/>

10.1.51.13의 IP 주소를 그대로 서버에 노출시키며 요청에 대한 결괏값을 해당 IP 주소로 받게 되겠죠. <br/>

<br/>

### 하지만 프록시를 거친다면 어떻게 될까요? <br/>

![Image 3](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/8988c6d0-6a2f-41d5-8aad-9678f6c1f1f7?raw=true)

<br/>

이 경우 웹 서버는 중계된 요청의 원래 IP 주소 (10.1.51.13)를 확인할 수 없습니다. <br/>

대신, 46.2.71.73라는 IP 주소로 요청을 받아들이고, 그 요청에 대한 답 또한 프록시 서버로 주게 됩니다. <br/>

한 개의 프록시 서버를 운용하는 것만으로도 클라이언트의 IP 주소를 숨길 수 있지만 <br/>

이는 여전히 웹 서버로 부터 추적 및 차단에 노출될 수 있습니다. <br/>

왼쪽의 클라이언트가 46.2.71.73라는 프록시 서버를 통해 웹 서버에 큰 피해를 끼쳤다고 가정해 봅시다. <br/>

웹 서버 운영자는 화가 단단하 나겠죠. <br/>


<br/>


아마 수사 기관에 의뢰할 겁니다. <br/>

"경찰 아저씨, 쟤 도대체 누구에요?" <br/>

만약 수사가 진행되면 경찰 등 수사 기관은 프록시 서버의 로그를 확인하게 될 것이고, <br/>

궁극적으로 우리의 원래 IP 주소 (10.1.51.13)을 어렵지 않게 알 수 있을 것입니다. <br/><br/><br/>



## 그래서 어쩌라고?

그래서 우리는 여러개의 프록시 서버를 사용할 겁니다. <br/>

한 개만 사용할 거였다면 프록시 "체인"이 아니라 그냥 프록시라고 했겠죠. <br/>

여러개의 프록시 서버들을 순서대로 연결하여 수차례 IP 주소를 변조한다면, <br/>

웹 서버 입장에서도 마땅히 트래픽을 차단하기 어렵기 때문입니다. <br/>

<br/>

### 클라이언트 -> 프록시 A -> 프록시 B -> 프록시 C -> 프록시 D -> 웹 서버

![Image 3](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/2fe949c0-6cea-4fb6-ade6-d143b96392f2?raw=true)

<br/>

예를 들어 이런 식의 연결을 구성한다면 <br/>

웹 서버의 도달하는 IP 주소는 프록시 D의 주소일 것입니다. <br/>

혹여 웹 서버에서 프록시 D의 주소를 차단하더라도, <br/>

우리는 아직 3개의 프록시가 남아있고 필요시 다른 프록시 서버를 추가하여 체인을 더 견고하게 만들 수 있습니다. <br/>

<br/>

이게 프록시 체인의 개념입니다. <br/>

자, 이제 프록시 체인을 실제로 설정해봅시다. <br/>

![Image 4](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/673b1268-17ec-4cc4-b661-be0f27a0e6e1?raw=true)

<br/>

우선 Kali 에서 proxychains4.conf 파일의 위치를 찾아 줍시다. <br/>

대개 /etc/ 에 있지만, 다를 수도 있으니 안전하게 locate 커맨드를 사용하는 것을 추천합니다. <br/>

저의 경우, /etc 에 잘 있었습니다. <br/>


![Image 5](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/5e4da74f-684b-4d57-9c36-6be8bd1e88d9?raw=true)
<br/>

.conf 파일을 뜯어보면 크게 4가지 타입이 있습니다. <br/>

Dynamic, Strict, Round_Robin, 그리고 Random이라 나오네요. <br/>

솔직히 말하자면 Tor를 사용할 거라, 뭘 고르던 크게 상관은 없습니다. <br/>

다만 크게 Dynamic과 Strict를 구분 지을 수 있는데, <br/>

Dynamic은 체인 중 하나가 동작 불능이 되어버리면 자연스럽게 스킵하는 방식입니다. <br/>


![Image 6](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/26b584be-0bed-44a8-be04-efe56a480a0a?raw=true)
<br/>

이렇게 하나의 프록시가 망가지더라도 연결을 지속할 수 있습니다. <br/>

<br/>

이에 반해 Strict는 하나의 프록시가 망가질 경우, <br/>

![Image 7](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/593aee86-3d90-4b7f-a354-7657587d1f78?raw=true)
<br/>

유동적으로 스킵하지 않고 연결이 종료되는 방식입니다. <br/>
<br/>

마지막으로 Random 방식은 프록시 체인의 순서를 결정할 때, 하나의 방식을 고집하지 않는 방식입니다. <br/>

![Image 8](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/2caacbaf-d6b4-45c6-8c1f-15f290091f3b?raw=true)
<br/>

이렇게 프록시 서버들만 구성을 해놓고 연결 순서는 매 연결마다 자동으로 달라지게 구성하는 방식을 뜻합니다. <br/>


![Image 9](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/8ef837d3-9fd7-4ea6-9647-fc1ac08dd0a0?raw=true)
<br/>

딱히 상관은 없었지만, 저는 여기서 Dynamic 모드를 선택했습니다. <br/>

모드를 변경하는 방법은 원하시는 모드를 Uncomment 해주시면 됩니다. <br/>

그럼 당연히 나머지 3개의 모드는 Comment 처리 해주셔야겠죠? <br/>

<br/>

![Image 10](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/203bf2fd-f743-47d2-be88-fb8406371213?raw=true)
<br/>

또한 proxy_dns 값을 꼭 Uncomment 해주셔야 됩니다. <br/>

DNS Leak으로 인해 위치가 노출되는 것을 막아주기 때문입니다. <br/>

제 경우 이미 활성화가 되어있었지만 만약 여러분들은 안 되어있다면 <br/>

Uncomment 처리 해주시면 됩니다. <br/>
<br/>

![Image 11](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/a99e8fd8-052b-4cff-abba-61e3cd2c75b0?raw=true)
<br/>

마지막으로 프록시 리스트를 하나 추가해주시면 됩니다. <br/>

conf 파일에서 스크롤을 쭉 내리다보면 금방 찾을 수 있을텐데, <br/>

여기에 저는 socks5를 추가해줬습니다. <br/>

주소는 Loop Back에 onion port 9050을 똑같이 추가해주시면 됩니다. <br/>

socks4랑 socks5랑 무슨 차이가 있냐구요?? <br/>

<br/>

이거 설명하면 너무 길어져요... <br/>

HTTPS가 HTTP 보다 짱짱인 거 알죠? <br/>

그런 느낌s... <br/>

<br/>

그 후 만약 여러분들의 Kali에 Tor가 설치되어있지 않다면 설치해주시면 됩니다. <br/>


```
sudo apt install tor
```

<br/>
위 커맨드로 어렵지 않게 설치할 수 있습니다. <br/>


![Image 12](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/0b0f588f-4038-4c2d-89cc-94aac6008663?raw=true)
<br/>

설치를 완료하셨다면 <br/>
<br/>


```
sudo service tor start
service tor status
```

두 개의 커맨드를 활용하여 <br/>

Tor가 정상적으로 동작하는지 확인해주시면 됩니다. <br/>


<br/>
<br/>

### 끝

진짜에요 ㅋㅋㅋㅋ <br/>

간단하죠? <br/>

이제 여러분들이 Kali에서 무언가 커맨드를 입력할 때, <br/>

제일 앞에다 proxychains을 붙히고 입력하시면 <br/>

프록시 체인으로 IP 주소를 변조하여 웹 등에 요청을 보낼 수 있습니다. <br/>
<br/>

![Image 13](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/f854d9fe-b39f-4519-a7e0-ba8006bf9299?raw=true)
<br/>

예를 들어, 파이어폭스를 실행한다 했을 때, <br/>

위 사진 처럼 proxychains 커맨드를 덧붙혀 주시기만 하면 됩니다. <br/>

<br/>

프록시가 잘 되는지 확인해볼까요? <br/>

우선 프록시 체인을 활성화 한 상태에서 파이어폭스로 접속 후, <br/>

dnsleaktest.com 에 접속합시다. <br/>

이후, 아무 테스트나 돌려보시면, <br/>

<br/>

![Image 14](https://github.com/HamsterJikJik/ProxyChain/assets/97205557/766f7c24-d29d-40b3-b94e-0616dcbff286?raw=true)
<br/>

제가 버지니아 주의 Manassas에 위치해 있다고 나오네요! <br/>

성공입니다! <br/>
<br/>


## 마치며...

다시 한 번 말씀드리지만, 프록시 체인을 구성하는 방법은 하나도 어렵지 않습니다. <br/>

아주 쉬운 방법으로 IP를 변조하여 웹 서버에 요청을 보낼 수 있다는 뜻입니다. <br/>

이 방법을 사용한다 하더라도 웹 해킹을 통해 불법적인 행위에 가담하신다면 <br/>

법령에 지정하는 처벌을 받을 수 있음을 알려드립니다. <br/>

해당 포스트는 모의해킹 및 버그 바운티를 위해 클라이언트 사이드 IP 주소를 숨기는데 그 목적이 있을 뿐, <br/>

그 어떠한 불법적 행위를 조장 및 권장하는 것이 아님을 강하게 밝힙니다. <br/>

<br/>
<br/>

### 감사합니다. 

<br/>
<br/>
