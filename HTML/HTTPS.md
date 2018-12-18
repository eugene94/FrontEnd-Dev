# HTTPS

언젠가 한번 제대로 알아보고 싶었던 내용이었다. 그래도 이렇게 잘 설명이 되어있는 블로그가 있어서 행복하다.
<br/>

이 글은 HTTPS에 대해서 막연히 알고 계신분들에게 도움이 되는 글이다.
<br/>

그래도 저는 적으면서 이해가 되는 사람이라서 읽는 동시에 적으면서 알아보도록 하겠습니다.
<br/>

간단하게 통신을 하는 것을 비둘기에 표현을 하여 나타냅니다. 예전에는 새의 발에 종이를 묶어서 보낸 것을 생각하고 적으신 듯 합니다.
<br/>

우리가 지금도 많이 사용하는 `HTTP( Hypertext Transfer Protocol)` 은 통신을 하는데 중간에 메시지를 가로채서 변경이 될 수 있다는 문제점을 가지고 있는데
이것은 아주 중요한 이슈입니다.
<br/>
<br/>

## 시크릿 코드

그렇다면 보내는 메시지와 받는 메시지를 암호화를 해서 보내고 받은 사람은 보낸사람의 메시지 암호를 해독해서 메시지를 확인하면 됩니다.
<br/>

이것을 **대칭 키 암호화(symmetric key cryptography)** 라고한다. 메시지를 암호화하는 방법을 알고 있으면 암호를 해독하는 방법도 알 수 있기 때문입니다.
<br/>
<br/>

## 키를 어떻게 결정할까?

보내는 사람과 받는 사람 이외의 다른 사람이 어떤 키가 사용되었는지 알지 못하면 대칭 키 암호화는 매우 안전합니다.
`Caesar cipher` 에서 중요한 것은 각 문자를 몇 자리 씩 이동했는지에 대한 오프셋 값이다. 이 예에서는 오프셋 3을 사용했지만 4 또는 12가 될 수도 있는 것이다.
<br/>

문제는 주고받는 사람간의 비둘기와 메시지를 보내기 전에 미리 만나지 않으면 키를 안전하게 만들 수 없다는 것이다.
<br/>

그들이 메시지 자체에 키를 보내면, 제 3자가 메시지를 가로 챈 다음 키를 알 수 있게 된다. 그러면 제 3자가 원하는대로 메시지를 읽거나 변경할 수 있게 된다.
<br/>

이것은 **Man in the Middle Attack(중간자 공격)** 의 전형적인 예이며, 이것을 피하는 유일한 방법은 암호화 시스템을 전부 변경하는 것이다.
<br/>
<br/>

## 상자를 나르는 비둘기

그래서 더 나은 시스템을 고안하게 된다. 메시지를 보내려고 할 때 아래 절차대로 보내도록 하는 것이다:

1. 처음 아무런 메시지없이 상대방에게 비둘기를 보낸다.
2. 나는 비둘기에게 열려있는 상자를 보내고, 열쇠는 가지고 있는다.(즉 아무것이나 넣을 수 있는 상태)
3. 상대방은 메시지를 상자에 넣고 잠근 다음 상자를 나에게 보낸다.
4. 나는 상자를 받고 키로 열어 메시지를 읽는다.(기존에 보내지 않고 가지고 있던 키)

이렇게하면 제 3자는 비둘기를 가로 채서 메시지를 변경할 수 없다.(이것 확실해짐)
<br/>

열쇠를 가지고 있지 않기 때문이다. 내가 상대방에게 메시지를 보내려고 할 때도 동일한 프로세스가 진행된다.
<br/>

일반적으로 **비대칭 키 암호(asymmetric key cryptography)** 로 알려진 것을 사용했다.
<br/>

비대칭이라고 불리는 이유는 메시지를 암호화 (상자 잠그기)해도 암호를 해독 할 수 없기 때문이다.
<br/>

기술적으로 설명하자면 상자는 **공개 키(public key)** 이며, 알려져 있으며이를 열 수있는 키를 **개인 키(private key)** 라고 한다.
<br/>
<br/>

## 상자는 어떻게 신뢰할 수 있을까?

여기에도 문제가 존재한다. 
<br/>

상대방이 열린 상자를 받으면 어떻게 나에게서 왔는지, 그리고 제 3자가 비둘기를 가로 채지 않고 상자를 바꾸지 않았다고 확신 할 수 있을까?
<br/>

나는 상자에 서명을 한다. 
<br/>

상대방이 상자를 받으면 서명을 확인해서 이 서명이 상자를 보낸 나의 것인지를 확인한다.
<br/>

여러분 중 일부는 이런 생각을 할 것이다. 
<br/>

상대방이 처음에 나의 서명을 어떻게 식별할까?
<br/>

나와 상대방도 이 문제를 인지하고 있었기 때문에 내가 상자에 서명하는 지인이 상자에 서명 할 것이라고 결정했다.
<br/>

지인은 매우 유명하고 잘 알려져 있으며 신뢰할만한 사람이다. 
<br/>

지인 모두에게 자신의 서명을 줬고, 모든 사람은 지인이 합법적인 사람에게만 서명 할 것이라고 믿는다.
<br/>

지인은 서명을 요구하는 사람이 나라고 확신하는 경우에만 내상자에 서명한다. 그래서 제 3자가 나를 대신해 지인이 서명한 상자를 얼을 수 없다. 지인은 그들이 신원을 확인한 후에만 상자에 서명을 하기 때문에 상대방이 상자가 가짜(fraud)라는 것을 알게 되기 때문이다.
<br/>

기술 용어로 지인은 일반적으로 **인증 기관(Certification Authority)** 이라고하며 이 글을 읽는 브라우저에는 다양한 **인증 기관(Certification Authorities)** 의 서명이 함께 제공된다.
<br/>

그래서 당신이 처음으로 웹 사이트에 접속하면 당신은 상자를 신뢰한다. 왜냐하면 당신은 지인을 신뢰할 것이고 지인은 상자가 합법적이라고 당신에게 얘기해주기 때문이다.
<br/>

## 상자는 무겁다

나와 상대방 이제 신뢰할 수 있는 시스템을 만들었지만 상자를 들고있는 비둘기는 메시지만 전달하는 것보다 느리다는 것을 깨닫는다.
<br/>

**그들은 대칭 암호를 사용하여 메시지를 암호화하는 키를 선택하기 위해서만 상자 방법(비대칭 암호화)을 사용하기로 결정한
이 방법으로 그들은 두 세계의 장점을 얻는다.**

<br/>

**비대칭 암호화의 신뢰성과 대칭 암호화의 효율성.**
<br/>

실제 세계에서는 이렇게 느린 비둘기가 없지만 대칭 암호화를 사용하는 것보다 비대칭 암호화를 사용하는 것이 속도가 느리기 때문에 암호화 키를 교환하는 데만 사용한다.
이제 HTTPS가 작동하는지 알게되었다. 끝
<br/>
<br/>

## 참고

- [비둘기로-설명하는-https-https-explained-with-car](https://www.vobour.com/%EB%B9%84%EB%91%98%EA%B8%B0%EB%A1%9C-%EC%84%A4%EB%AA%85%ED%95%98%EB%8A%94-https-https-explained-with-car)
