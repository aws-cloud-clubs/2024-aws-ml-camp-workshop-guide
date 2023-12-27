# LAB 1 : 기존 웹앱에 Amazon Rekognition 추가하기

AcadeML Research Tool은 이제 우리나라 모든 대학교에 배포할 준비가 거의 되어 있습니다.

연구원들과 학생들은 AcadeML을 사용하기를 계속 기다리고 있죠.

그러나, 아직 더 구현해야할 몇 가지 기능들이 여전히 남아있습니다.

1. Object Detection (객체 탐지)
2. Labeling in Images (이미지 레이블링)

우리는 위의 2가지 기능을 구현하기 위해 아래와 같은 요구사항을 따라줘야 합니다.

- 대학은 AWS 계정을 가지고, AcadeML 앱은 Amazon Machine Learning Services를 사용할 것이다.
- 먼저 추가되어야 하는 서비스는 Amazon Rekognition이다.
- AcadeML 인터페이스는 다른 팀원에 의해 개발되었고, 우리는 이 인터페이스를 수정할 수 없다.
- IAM 계정의 세부적인 정보들은 우리에게 주어질 것이다.

이 실습은 우리에게 단계별 과정을 제시해줍니다.

1. Github에서 기존 AcadeML 웹앱을 clone
2. Dependencies 설치 후 앱 실행
3. 브라우저에서 앱 테스트
4. 정확히 어떤 코드에서 API를 호출해야하는 것인지 알아내기
5. AWS IAM 환경변수 추가
6. Amazon Rekognition 호출 코드 작성
7. Amazon Rekognition을 통해 나오는 결과들 확인 후, 서비스들의 현재 가능한 기능들과 한계를 이해하기
8. AcadeML에서 새로운 기능 테스트해보기