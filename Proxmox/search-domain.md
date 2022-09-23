# proxmox 설치시 search domain 입력 주의

search domain 은 도메인의 ip찾을때 기본으로 입력되는 도메인이다.

예를 들어 abc 라는 도메인을 찾을때 search domain을 home.zsoo.net 으로 했다면, abc.home.zsoo.net 도메인으로 연결을 시도한다.

proxmox 서버에 설정한 search domain 정보는 vm 에 상속된다. 이 값은 다시 vm 안에서 생성되는 k8s pod에 상속된다. vm 설정, k8s 설치판에 따라 다를 수 있으나 ubutun vm 기본 설정, k3s 에서는 상속되었다.

k8s pod 안에서 google.com을 연결시도할 때 google.com.home.zsoo.net 에 대해서도 연결 시도를 시도하게 된다. 물론 google.com.home.zsoo.net 에 대한 dns record가 없다면 아무 문제가 없겠지만, *.home.zsoo.net 에 대한 레코드가 존재하는 경우 모든 트래픽이 엉뚱한 곳으로 가게 된다.
