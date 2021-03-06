# QUIC (Quick UDP Internet Connection)

![](https://blog.cloudflare.com/content/images/2018/07/QUIC-Badge-Dark-RGB-Horiz.png)

* GIGA시대가 대중화된 2018년 최근 KT에서 10GIGA 인터넷 서비스를 시작했습니다. 

  ![](https://www.netmanias.com/ko/?m=attach&no=33190)

  * 2.5Gbps, 5Gbps, 10Gbps 속도로 제공되는 이 서비스는 KT에서 9월부터 시작했고, SK, LG U+도 서비스를 시행 혹은 준비중에 있습니다. 

    ![](https://www.netmanias.com/ko/?m=attach&no=33192)

  * 아직 기가 인터넷도 충분하다고 생각하는 사람들이 대부분일텐데 ISP 업자들은 경쟁적으로 10Gbps 인터넷 서비스에 열정적으로 참여하고 있을까요? 

    * 이는 내년 3월에 서비스 될 것이라고 예상되는 5G 서비스가 주 이유입니다. 
    * 5G는 무선으로 단말당 최대 20Gbps속도를 낼 수 있습니다. 
    * 단, 유선은 아직 1Gbps에 불과 하기 때문에 유선 백본망으로 미리 구축하면 5G 백본으로도 사용할 수 있으며, 속도경쟁에 우위를 볼 수 있기 때문입니다. 
    * 아직 10Gbps의 대역폭을 사용가능한 서비스들이 거의 없기 때문에 시기상조라는 말도 돌고 있지만 일단 대역폭만 확보한다면 추후 각종 애플리케이션이 그 자리를 채우게 될 것입니다. 



* 이렇게 인터넷 속도는 점점 빨라지고 있는 상황에서 구글에서 QUIC라는 새로운 프로토콜을 국제 표준화 기구(IETF)에서 표준화를 진행하고 있습니다. 
* 그럼 본질적인 의문이 들기 시작합니다. 왜 구글은 전통적인 TCP, UDP를 두고 새로운 프로토콜을 개발하게 된 것일까요? 



### QUIC (Quick UDP Internet Connection)의 탄생 

* QUIC는 2013년 6월 처음공개되어 현재는 구글 서비스의 약 절반 정도가 QUIC 프로토콜로 처리되고 있습니다. 

* 우리나라에서 QUIC를 굳이 사용하지 않아도, 기존의 프로토콜을 사용해도 만족할 만한 서비스를 사용하고 있다고 생각하시는 분이 있으실 것입니다. 

  * 국내는 이미 충분한 인프라가 갖춰져 있으며, 대부분 아무리 느려도 10ms 이하의 지연시간을 가지고 있기 때문입니다. 
  * 따라서 국내 인터넷을 사용할 때 지연시간으로 인한 답답함을 느낀다는 것은 거의 없다고 해도 무방합니다. 

* 하지만 전세계가 인터넷에 연결되어 있는 지금 우리가 모두 국내 서비스만 사용하고 있을까요? 

  ![](https://t1.daumcdn.net/cfile/tistory/2446824557700FC815)

  * 우리가 사용하는 페이스북, 유투브, 인스타그램 등의 다양한 SNS와 대부분의 애플리케이션과 웹 서비스등은 해외의 서버를 두고 있거나 클라우드를 통해 서비스를 제공하고 있습니다. 
  * 이런한 환경에서 기존 TCP를 이용한 서비스는 **지연**이 발생할 수 밖에 없는 구조를 가지고 있습니다. 

* TCP는 무결성을 전제 조건으로 만들어진 프로토콜입니다. 

  * 데이터의 병목 지점이 없어도 데이터의 유실을 막기 위해서 작은 단위로 쪼개서 전송하게 되며, 전송이 성공했는지 계속해서 판단합니다. 

  * 이러한 TCP의 특성상 물리적 거리가 멀어질 수록 점점 지연시간이 늘어날 수밖에 없는데 이것을 RTT(Round Trip Time)이라고 합니다. 

    * 간단하게 ICMP를 활용한 ping을 통해서도 확인할 수 있습니다. 

  * 자세히 들어가서 TCP 기반에서 연결을 수립할 때에는 3-Way-Hadshake 과정이 필요하며, 여기에 암호화를 위해 TLS까지 적용된다면, 실제 통신이 이루어 지기 이전에 연결 수립만을 위한 지연이 약 4.5 RTT가 발생하게 됩니다. 

    ![](https://lh3.googleusercontent.com/o62Ohn1Ppxna6zz0NtavqRyetjryOj-81Sz4bRt3U8lURVblk5RKOaCcf57i6BkmprremePJpq_sQcxfJiuA4wJBmRp3pR4BS1P-yiT6UNUPvnBeP_rLz9bvHxFE15kuNBM2hpE)

### QUIC의 특징 

![](https://blog.cloudflare.com/content/images/2018/07/http-request-over-quic@2x.png)

* QUIC (Quick UDP Internet Connection)는 기본적으로 Zero RTT가 핵심입니다.

* 다른 프로토콜에 비해 가볍고, 성능과 보안성을 모두 고려해서 설계되었으며, 암호화된 전송을 통해 다중화된 스트림을 제공하는 UDP 기반 전송 프로토콜입니다. 

* 일반적인 TCP 연결에 비해서 대기시간이 훨씬 단축되고 스트림 다중화 지원이 향상됩니다. 

* 실제 구글은 QUIC를 적용한 이후 구글 검색에서 약 3%의 로딩 시간 개선, 유투브 시청시 약 30% 정도 버퍼링이 감소했다고 발표한바 있습니다.

  * 이는 절대 무시할 수 없는 수치이며, 대기시간이 긴 애플리케이션일 수록 체감으로 느껴지는 성능은 더 높아질 것으로 예상됩니다. 
  * 이는 사용자에게 향상된 혼잡제어와 UDP 손실 복구를 통해서 단 1초라도의 빠른 로딩을 체감할 수 있을 것입니다.  

* 국내에서는 네이버 및 몇몇 게임회사가 QUIC를 통한 통신성능을 테스트하고 있다고 알려져 있으나 미미한 수치이며, 전 세계적으로는 20억 개의 서버에서 QUIC 서버 모듈이 구동 중이라고 알려져 있습니다. 

  ![](https://www.codavel.com/wp-content/uploads/2018/09/Captura-de-ecra%CC%83-2018-09-17-a%CC%80s-10.38.37-1024x463.png)



  * 전송 프로토콜로 UDP를 사용하기 때문에 새로운 응용계층 개발시 빠른 배치가 가능합니다. 

  * 새로운 버전이 개발되는 경우 버전 확인을 통한 호환성을 제공합니다. 

  * 헤더의 인증 및 암호화(AES-GCM 및 ChaCha20과 같은 AEAD 알고리즘 내장)를 통해서 패킷의 변조를 막을 수 있습니다. 

  * 재전송 패킷도 새로운 순서번호를 사용하여 수신측에서 패킷이 새로운 것인지 아니면 아니면 재전송된 패킷인지 구분할 수 있습니다. 

  * 다중 스트림 제공 및 개별 스트림 내에서의 흐름을 제어할 수 있는 기능을 제공하여 **HoL 블로킹** 문제를 해결할 수 있으며, 공유 혼잡 제어 및 오류 복구를 제공합니다. 

    * HoL (Head-Of-Line) 블로킹 : 네트워크에서 같은 큐에 있는 패킷이 첫번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상 

      ![](https://t1.daumcdn.net/cfile/tistory/9957274F5B5DB5A129)

      * 위의 그림과 같이 input 패킷 1과 3이 동일한 인터페이스로 보내려고 할 때 Switching fablic에서 3번째 패킷을 처리해 버리면 1번 input 패킷이 처리되지 못하는 현상  
      * 이러한 문제로 인하여 패킷의 처리속도가 저하되고 최악의 경우 드랍까지 발생할 수 있습니다. 

### 1초의 경쟁력 

* 다양한 아이디어로 무장한 업체들 속에서 어떻게 하면 안정적인 서비스가 가능할지 최대의 고민이 될 것입니다. 

* 다양한 스타트업 업체들 중에서 온프레미스로 운영할지 오프프레미스로 운영할지에 대한 선택은 기업의 결정권에 달려있습니다. 하지만, 서비스의 특성을 고려하지 않고 무조건적인 클라우드 서비스는 오히려 부정적인 영향을 끼칠 수 있습니다. 

* 글로벌 서비스 제공과 다양한 기능의 무장으로 본인의 컨텐츠의 특성을 생각하지 않고 서비스를 운영하는 것은 최악의 선택입니다. 

  * 실제로 국내 동영상 서비스를 해외 클라우드 서비스로 런칭한 업체는 TCP 연결의 문제로 인하여 잦은 끊김과 장애로 국내로 서버를 이전하고 CDN을 사용하는 등의 대응을 조치했으나 결국 서비스를 종료해야 했습니다. 

  * 또한 모 소셜커머스 회사의 경우 이미지가 전체 트래픽에 90%가 차지하는데도 불구하고 해외 클라우드 서비스를 운영한 결과 다른 업체의 비해 1초가량 늦은 로딩 속도로 인해서 경쟁력을 잃게 되었고 추후 국내로 서버를 옮긴 결과 해외 운영시에 비해 갑작스러운 트래픽 피크에도 안정적인 대응이 가능 했다는 후일담은 인프라 업계에서 널리 퍼져 있습니다. 


* 1초는 매우 짧은 시간일 수도 있으나 사용자 입장에서 1초의 지연은 결과적으로 서비스의 큰 타격을 입힐 수 있다는 것을 반드시 기억해야 할 것입니다. 
* 이러한 상황속에서 QUIC의 등장은 앞으로 **지연**에 대한 해결책으로 등장할 수 있을 것이지만 QUIC가 글로벌 클라우드 환경에서 100% 안전성을 보답하지는 않을 것입니다. 
* 스타트업은 이러한 타켓 소비자층이 누구인지를 명확하게 직시하여 서비스 런칭시 이러한 우를 범하지 않도록 고민해야 할 것입니다. 