## CI와 CD란 무엇일까요?
코드 버전 관리를 하는 VCS 시스템(Git, SVN)에 PUSH가 되면  <br>
자동으로 테스트와 빌드가 수행되어 `안정적인 배포로 파일을 만드는 과정`을 CI(지속적 통합)라고 하며, <br>
이 빌드 결과를 자동으로 운영 서버에 무중단 배포까지 진행되는 과정을 CD(지속적인 배포)라고 합니다. <br><br>
일반적으로 CI만 구축되어 있지는 않고, CD도 함께 구축된 경우가 대부분이다. 

### 왜 이렇게 CI/CD란 개념이 나왔는지 
현대의 웹 서비스 개발에서는 하나의 프로젝트를 여러 개발자가 함께 개발을 진행합니다. <br>
개발자가 각자가 원격 저장소(깃허브)로 푸시 될때마다 코드를 병합하고, 테스트 코드와 빌드를 수행하면서<br>
자동으로 코드가 통합되어 `더는 수동으로 코드를 통합`할 필요가 없어지면서 자연스레 개발자들 역시 <br>
개발에만 집중할 수 있게 되었습니다.
