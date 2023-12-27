# 2024-AWS-ML-Camp-Workshop-Guide
2024 AWS ML camp Workshop Studio Documentation (Translated in Korean)

> AWS Cloud Club in South Korea에서 진행하는 **2024 AWS ML Camp** 워크샵 한글 번역 문서입니다.

## 준비 사항
### 1. 하드웨어
모든 OS가 가능

### 2. 소프트웨어
#### Node.js
1. Node includes npm, but you can install it manually too if you want: [NPM](https://www.npmjs.com/get-npm)
2. Verify your installation by running:
    1. `node -v` and `npm -v` 를 터미널/윈도우 콘솔에서 실행했을 때 잘 작동해야.
#### Visual Studio Code
다른 IDE들도 사용가능하지만 워크샵 모든 단계에서 VS Code를 기준으로 설명합니다.

### 3. AWS 계정
공식으로 열린 워크샵(Hosted workshop)이기 때문에 여러분들은 개인 계정이 필요하지 않습니다.

### 4. IAM 액세스 키 & 시크릿 구성
#### Hosted workshop
- 이미 필요한 권한이 있는 IAM 역할이 만들어져 있어요.
- 캡틴이 필요한 키에 액세스하는 방법을 알려드릴 예정입니다.
- 워크샵 동안 동일한 키를 사용할 수 있습니다.
- 이 창의 왼쪽 표시줄에 있는 AWS 계정 액세스 섹션을 확인하고 AWS CLI 자격 증명 가져오기를 선택하면 언제든지 키를 받을 수 있습니다.

##  시나리오

> 우리는 다음 시나리오의 상황에 처해있습니다! 🤣

모든 학과의 연구자와 학생들이 사진, 이미지, 문서 스캔을 검토하는 데 사용하는 AcadeML이라는 웹 앱을 개발해야합니다!

AcadeML은 다양한 방식으로 사용될 수 있어야 해요. 예를 들면,

- 요리 연구자는 레시피(요리 순서, 완성된 음식 사진 등)를 검토하는 데 사용할 수 있습니다.
- 역사학 교수님은 오래된 손글씨 편지를 분석하는 데 사용할 수 있습니다.
- 식물학 학생은 현장 탐험에서 찍은 사진을 검토하는 데 사용할 수 있습니다.

**AcadeML의 개발자들 중에서도, 우리는 ML 기능을 추가하는 역할을 맡게 되었습니다.**

현재 앱은 이미지 목록을 표시하고 새 이미지를 업로드하여 표시할 수 있습니다. ML로 이미지를 분석할 수 있는 몇 가지 버튼이 있지만 **현재는 아무 기능도 수행하지 않습니다.** 필요한 기능은 모든 이미지를 머신러닝으로 분석할 수 있도록 하고 이 버튼을 클릭할 때 결과를 표시하는 것입니다. 우리가 사용할 두 가지 기술은 객체 라벨링과 광학 문자 인식입니다.

다음 실습에서는 이 두 가지 기술을 추가할 것입니다:

- [Lab 1](./LAB1_Rekognition.md): **Rekognition**으로 분석하기: 이미지를 Rekognition API로 보내면 이미지에서 감지한 객체에 Confidence Score를 매겨 레이블을 지정해보아요.

- [Lab 2](./LAB2_Textract.md): **Textract**로 분석하기: 이미지를 Textract API로 보내면 이미지에서 텍스트를 감지해보아요.