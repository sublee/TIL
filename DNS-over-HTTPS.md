# 👷 WIP

# Table of Contents

- [DNS over HTTPS (DoH)](#dns-over-https-doh)
  - [Cloudflare 1.1.1.1](#cloudflare-1111)
  - [PC](#pc)
    - [Simple DNSCrypt](#simple-dnscrypt)
  - [Router](#router)
    - [Asuswrt-Merlin](#asuswrt-merlin)
  - [Android](#android)
  - [iOS](#ios)
  - [Firefox](#firefox)
  - [DoH 적용 테스트](#doh-적용-테스트)
- [ESNI (Encrypted Server Name Indication)](#esni-encrypted-server-name-indication)

# DNS over HTTPS (DoH)

HTTPS를 사용한다고 해도 DNS 쿼리는 암호화되지 않은 평문으로 전송이 된다. 그래서 어떤 사이트를 방문하고 있는지 감청할 수 있다.

DNS over HTTPS(DoH)는 DNS 쿼리를 HTTPS 프로토콜을 이용하여 암호화된 방법으로 주고받는 방법이다. 이 방법을 통해 DNS 요청을 통해 접속하는 서버를 감청하는 사생활 침해에서 벗어날 수 있다. (하지만 감청을 하고자 마음먹는다면 SNI를 감청해서 접속하는 서버를 알아내는 방법이 있으며, 이 경우에는 [ESNI](#esni-encrypted-server-name-indication)를 적용해야만 감청에서 자유로울 수 있다.)

SEE: https://en.wikipedia.org/wiki/DNS_over_HTTPS

## Cloudflare 1.1.1.1

DoH를 제공하는 업체는 많이 있지만, 가장 유명하고 요청 로그를 영속적으로 보관하지 않는 업체로 Cloudflare가 있다.

Cloudflare는 `1.1.1.1` 주소로 DNS를 제공한다. 하지만 DNS 주소를 변경한다고 해서 DoH가 적용되는 것은 아니다.

Cloudflare는 자체적으로 만든 Mobile Apps를 제공하며, 이 앱을 통해 DoH가 적용된 DNS를 사용할 수 있다.

- [iOS](https://itunes.apple.com/us/app/1-1-1-1-faster-internet/id1423538627?mt=8)
- [Android](https://play.google.com/store/apps/details?id=com.cloudflare.onedotonedotonedotone)

## PC

### Simple DNSCrypt

https://simplednscrypt.org/

PC에서는 Simple DNSCrypt를 이용해 DoH를 사용할 수 있다. DNS 제공 업체를 고를 수 있으며, 개인적으로는 DNS 쿼리 로그를 영속적으로 저장하지 않는 업체를 추천한다. (Cloudflare 등)

로그 저장? theregister라는 곳의 [기사](https://www.theregister.co.uk/2018/04/03/cloudflare_dns_privacy/)에서 Cloudflare는 로그를 24-48시간 보관하지만, Google은 장기간 보관한다고 한다. 아무리 Google이라지만 나의 요청 기록이 없어지지 않고 장기간 남는다는 것은 꺼림직할 것 같다.

> // ref: https://www.theregister.co.uk/2018/04/03/cloudflare_dns_privacy/
>
> In this Cloudflare's venture is similar to Google's Public DNS (8.8.8.8), which claims that it keeps some data for just 24 to 48 hours. Google, however, keeps other non-personally identifiable information for longer periods.

## Router

### Asuswrt-Merlin

👷 WIP

## Android

~~Android P에서 DoH를 지원하려는 [움직임](https://android-developers.googleblog.com/2018/04/dns-over-tls-support-in-android-p.html)이 보이지만, 현재 최신버전의 Android Oreo에서는 쓸 수 없다.~~

Android Pie 부터는 `Private DNS Mode`가 추가되어 DoH를 사용할 수 있다. 자세한 것은 [링크](https://blog.cloudflare.com/enable-private-dns-with-1-1-1-1-on-android-9-pie/) 참고

Android Pie 미만에서는 서드파티 앱을 통해 DoH를 사용할 수 있다, 대표적으로 Cloudflare 공식 앱과 Intra 앱이 있다.

Cloudflare DNS를 사용할 것이라면 Cloudflare 공색 앱을 사용하는 것을 권장한다.

- [Cloudflare 공식 앱](https://play.google.com/store/apps/details?id=com.cloudflare.onedotonedotonedotone)
- [Intra](https://play.google.com/store/apps/details?id=app.intra&hl=en_US)

둘 다, 내부적으로 VPN을 만들어서 모든 연결에 대해 DoH를 적용하는 방식으로 동작한다. 앱을 활성화 시키면 Andorid에서 VPN 연결중이라고 뜨게 된다.

Infra앱에서는 Cloudflare와 Google, 두 가지의 DoH 서버를 설정할 수 있다.

[Simple DNSCrypt](#simple-dnscrypt) 항목해서 설명한 것 처럼, 보통은 DNS 로그를 영속적으로 저장하지 않는 Cloudflare를 추천한다.

## iOS

iOS에서는 Cloudflare 공색 앱과 DNSCloak 앱을 사용해서 DoH를 사용할 수 있다. Android와 마찬가지로 VPN 기능을 활용하는 방식이다.

- [Cloudflare 공색 앱](https://itunes.apple.com/us/app/1-1-1-1-faster-internet/id1423538627?mt=8)
- [DNSCloak](https://itunes.apple.com/kr/app/dnscloak-dnscrypt-doh-client/id1330471557?mt=8)

## Firefox

Firefox는 자체적인 DoH 기능을 가지고 있다. Firefox 60부터 사용할 수 있다. 이 글을 작성하는 시점에서는 Android와 Windows용 Firefox가 60 버전 이상인 것을 확인했다.

주소창에 `about:config`를 입력해 고흡 관경 설정 페이지로 이동한다.

상단의 `검색`창에 `network.trr`을 입력해서 `network.trr`로 시작하는 설정들을 모아 볼 수 있게한다.

그리고 아래 항목의 값을 설정한다.

- `network.trr.bootstrapAddress`: 1.1.1.1
- `network.trr.mode`: 2
- `network.trr.uri`: https://mozilla.cloudflare-dns.com/dns-query

각 설정값의 의미를 설명해보자면 이렇다.
관심이 없다면 넘어가도 된다.

참고로 TRR은 Trusted Recursive Resolver의 약자.

`network.trr.uri`: 사용할 DoH 서버의 URI를 설정한다. 반드시 HTTPS 주소여야 한다.

`network.trr.bootstrapAddress`: `network.trr.uri`에서 설정한 호스트의 IP 주소를 설정한다. 이 값을 설정하면 시스템에서 호스트 IP를 얻어내는 것을 무시하고 설정한 값을 사용하게 된다. Cloudflare DoH를 설정했기 때문에 `1.1.1.1`을 사용했다.

`network.trr.mode`: 값에 따라 DoH의 동작을 설정한다.

- 0 - (기본값) DoH 기능을 끈다
- 1 - 시스템 기본 방식과 DoH에 동시에 요청을 보낸다. 빨리 응답이 오는 쪽을 사용
- 2 - DoH를 기본으료 사용하고, 응답이 실패할 경우에 시스템 기본 방식을 사용
- 3 - DoH만 사용한다. 시스템 기본 방식을 사용하지 않는다
- 4 - 타이밍 측정을 위해 DoH와 시스템 기본 방식을 병렬로 실행한다. 하지만 시스템 기본 방식의 응답만 사용한다
- 5 - 0과 같다. 0은 기본값, 5는 선택으로 인한 값을 표시하기 위해 사용한다

DoH만 사용하고 싶다면 `network.trr.mode`를 `3`으로 설정하면 된다.

이제 별다른 도구 없이 Firefox에서 자체적으로 DoH를 적용할 수 있다.

## DoH 적용 테스트

DoH가 잘 적용되었는지 확인하기 위해서는 https://dnsleaktest.com/ 등의 사이트를 이용할 수 있다.

위 URL로 접속해서 `Standard Test` 버튼을 눌러보자. 공급자가 `Cloudflare`로 표기되면 성공, 본인이 사용하는 인터넷 사업자 (KT, SKT, ...)가 표기된다면 제대로 적용되지 않은 것이다.

# ESNI (Encrypted Server Name Indication)

👷 WIP

- https://blog.cloudflare.com/encrypted-sni/
- https://www.cloudflare.com/ssl/encrypted-sni/