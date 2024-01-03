# LAB 2 : 기존 웹앱에 Amazon Textract 추가하기

LAB 2는 여러분들이 이미 LAB 1을 완료했다는 가정 하에 진행되기 때문에, 상대적으로 짧은 실습입니다.

만약 이전 LAB을 완료하지 못하셨다면, 적어도 이전 LAB의 1단계~5단계까지는 완료를 하셔야 LAB2 진행에 문제가 없을 겁니다.

***LAB1을 끝마친 후 LAB2를 진행하셔야 합니다!***

## Textract 모듈과 DetectDocumentTextCommand 명령 Import하기
Amazon Rekognition과 마찬가지로, 이 과정 역시 이전에 진행했던 `npm install` 명령을 통해 설치가 되었을 겁니다. 만약 위 모듈들을 수동으로 설치하고 싶으시다면 `npm i @aws-sdk/client-textract` 명령을 입력하시면 됩니다.

더 많은 정보들은 [AWS SDK for JavaScript](https://github.com/aws/aws-sdk-js-v3) 레포를 참고하시길 바랍니다.

설치가 완료되었다면, 여러분들은 설치한 모듈들을 `AmazonML.js` 파일 내에 import 하셔야 합니다.<br>
아래 코드와 같이 수정해주세요. (기존 코드들을 지우지 말고 모듈 추가만 해주시면 됩니다.)
```jsx
import { Buffer } from "buffer";
import { RekognitionClient, DetectLabelsCommand } from "@aws-sdk/client-rekognition";
// 아래 코드에 TextractClient 모듈 추가
import { DetectDocumentTextCommand, TextractClient } from "@aws-sdk/client-textract";

const creds = {
  region: import.meta.env.VITE_AWS_REGION,
  credentials: {
    accessKeyId: import.meta.env.VITE_AWS_ACCESS_KEY_ID,
    secretAccessKey: import.meta.env.VITE_AWS_SECRET_ACCESS_KEY,
    sessionToken: import.meta.env.VITE_AWS_SESSION_TOKEN, // leave out if not in hosted workshop
  },
};

let rekognitionClient = null;
// 아래 코드 추가
let textractClient = null;

export async function analyzeImageML(type, imageData) {
  ...
```

## Amazon Textract 클라이언트 추가하기
아래 코드와 같이 `else if`문을 `if (type == "labels") {` 조건문 블록에 추가해주세요.
```jsx
  ...
  if (type == "labels") {
      // If the client has not been initalized yet, create it
      if (!rekognitionClient)  
        rekognitionClient = new RekognitionClient(creds); // pass in the creds as parameter
      const query = new DetectLabelsCommand(params);
      let response = await rekognitionClient.send(query);
      returnData = {
        type: "success",
        text: response,
      };
  } else if (type == "text") {
      if (!textractClient) 
        textractClient = new TextractClient(creds);
      const params = {
        Document: { Bytes: uimage_bytes },
      };
    }
  } catch (error) {
    ...
```
이 코드는 클라이언트를 생성하고 `params` 값들을 명령 인자로 입력합니다. `params` 변수가 오직 image bytes만 필요로 하기 때문에, `DetectLabels`보다 덜 복잡하다는 것에 주목해주세요.

## 명령 실행
마지막 2줄의 코드는 인자와 함께 명령을 생성하고 클라이언트에게 전송합니다. 모든 것이 정상적으로 동작한다면, JSON response가 리턴됩니다.
```jsx
    ...
      const params = {
        Document: { Bytes: uimage_bytes },
      };      
      const query = new DetectDocumentTextCommand(params);
      let response = await textractClient.send(query);
      returnData = {
        type: "success",
        text: response,
      };  
    }
    ...
```

## Command response 테스트하기
브라우저 내의 개발자 도구를 통해 `console.log(response)` 코드가 출력하는 결과를 확인해봅시다. response의 전체 구조와 더불어, 텍스트에 해당하는 데이터를 response의 어느 부분에서 찾을 수 있는지 확인해봅시다.

완료되었으면, App 페이지에서 `Detect Text` 버튼을 클릭했을 때의 기능들을 테스트해봅시다.
1. 왜 특정 이미지들이 다른 이미지들보다 텍스트 추출 성능이 좋을까요?
2. 여러분들의 자체 이미지로도 시도해보셨을 때도 정상적으로 동작하나요?
3. 여러분들의 손글씨 이미지도 인식하나요?

아래는 2개의 LAB을 통해 작성했던 `AmazonML.js`의 전체 코드입니다.
```jsx
import { Buffer } from "buffer";
import { RekognitionClient, DetectLabelsCommand } from "@aws-sdk/client-rekognition";
import { DetectDocumentTextCommand, TextractClient } from "@aws-sdk/client-textract";

const creds = {
  region: import.meta.env.VITE_AWS_REGION,
  credentials: {
    accessKeyId: import.meta.env.VITE_AWS_ACCESS_KEY_ID,
    secretAccessKey: import.meta.env.VITE_AWS_SECRET_ACCESS_KEY,
    sessionToken: import.meta.env.VITE_AWS_SESSION_TOKEN, // leave out if not in hosted workshop
  },
};

let rekognitionClient = null;
let textractClient = null;

export async function analyzeImageML(type, imageData) {
  const uimage_bytes = base64ToUint8Array(imageData.split("data:application/octet-stream;base64,")[1]);
  const params = {
    Image: { Bytes: uimage_bytes },
    MaxLabels: 10,
    MinConfidence: 80,
  };
  
  var returnData = null;
  try {
    if (type == "labels") {
      // If the client has not been initalized yet, create it
      if (!rekognitionClient)  
        rekognitionClient = new RekognitionClient(creds); // pass in the creds as parameter
      const query = new DetectLabelsCommand(params);
      let response = await rekognitionClient.send(query);        
      returnData = {
        type: "success",
        text: response,
      };
  } else if (type == "text") {
      if (!textractClient) 
        textractClient = new TextractClient(creds);
      const params = {
        Document: { Bytes: uimage_bytes },
      };
      const query = new DetectDocumentTextCommand(params);
      let response = await textractClient.send(query);
      returnData = {
        type: "success",
        text: response,
      };  
    }
  } catch (error) {
    returnData = {
      type: "error" /* success info warning error */,
      text: error.message,
    };
  }
  return JSON.stringify(returnData);  
}

// imageData is string with data:application/octet-stream;base64,...
function base64ToUint8Array(base64Data) {
  const decoded = Buffer.from(base64Data, "base64");
  const bytes = new Uint8Array(
    decoded.buffer,
    decoded.byteOffset,
    decoded.byteLength
  );
  return bytes;
}
```

LAB2를 모두 끝마치셨습니다! <br>
이번 실습이 만족스러우셨다면 [워크샵 Github 레포지토리](https://github.com/aws-cloud-clubs/2024-aws-ml-camp-workshop-guide)에 Star를 눌러주세요 ✨ 