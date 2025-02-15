# 🐶 강아지킴
![Logoview](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/ef567dce-5d1a-4dd2-9b41-2ab90c23fea7)

## 😊 Members
#### 곽채원, 김민수, 박예은, 류홍규, 박지환, 이윤열, 이태훈

## 📜 서비스 내용

**반려견의 피부 질환을 AI가 진단하고, 수의사에게 물어볼 수 있는 서비스입니다.**

🌱 **반려인 시나리오**

1. 반려견의 피부 질환 의심부위를 찍고 업로드한다.
2. 해당 의심부위의 피부 질환을 AI가 진단한 결과를 알려준다.  
    2-1. AI가 예상한 질환에 대한 ChatGPT의 설명을 볼 수 있다.
3. 추가적인 질문사항이 있는 경우 Q&A 게시판에 질문을 올린다.  
    3-1. 질문당 하나의 ChatGPT의 답변이 작성된다.
4. 수의사의 답변을 확인한다.
5. 자신이 작성한 질문 내역을 확인할 수 있다.

🌱 **수의사 시나리오**

1. AI 피부 질환 진단 서비스는 마찬가지로 사용할 수 있다.
2. Q&A 게시판에 올라오는 질문들에 답변을 작성한다.
3. 답변 개수와 퀄리티를 바탕으로 메인페이지에 병원 광고를 게시할 수 있으며,  
    사용자들의 AI 진단 후 추천 병원으로 광고할 수 있다.

## 🛠 기술 스택

🌟 **[Front](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project-front)** 

- Android Studio (Java)

🌟 **[Backend](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project-back)**

- Django
- AWS (EC2, RDS, S3)
- MySQL
- NGINX

## 🖥 개발 내용

**Frontend**는 **Android Studio**를 사용하여 **Android APP**을 구현했습니다.  
**Frontend**에서 **Backend**로의 통신은 **Retrofit2**를 이용하여 구현했습니다.  

**Backend**는 **Django REST Framwork** 를 사용하여 **REST API**를 구현하였고,  
**Backend 서버**의 경우 **AWS EC2**를 사용하고, DB와 Storage를 **RDS**와 **S3**로 분리해서 구현했습니다.  
DB는 **MySQL**을 사용했고, API 서버는 **NGINX**로 배포 하였습니다.

![전체 아키텍처](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/13626f8b-9a5f-4337-9477-b857a13bda9c)


### ✅ API 명세서

API에 대한 내용들은 Notion에 API 명세서를 작성하여 관리했습니다. [[API 명세서 링크]](https://www.notion.so/957e66a93eee468b9ad01613f041ea0a?pvs=21)
![api 명세서 캡처 다크모드](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/7b2391ad-112e-486f-9b08-aa2d63bf23e1)

### ✅ 로그인 관련 구현

Django REST Auth를 활용하여 기본적인 회원가입, 로그인, 로그아웃 등을 구현했습니다.

아래와 같은 Field들을 추가하여 Custom User를 구현했습니다.

- Email을 로그인 시 사용 (기본은 Username)
- 수의사 여부 Field 추가
- 프로필 이미지 Field 추가 : Default로 pydenticon 이미지 사용

✔️ **JWT Token**

로그인할 때에는 `AccessToken` 과 `RefreshToken` 을 발급해 해당 토큰으로 사용자 정보를 확인할 수 있도록 구현했습니다.

✔️ **기타 User 관련 API**

회원가입 시 Email 중복 확인을 하는 API와, 비밀번호를 까먹었을 때 해당 아이디로 Email을 전송하여 비밀번호를 초기화할 수 있는 API를 구현했습니다. Front에서와 마찬가지로 비밀번호는 `SHA256` 암호화를 수행한 후 전달되도록 구현했습니다.

|비밀번호 초기화 메일|비밀번호 초기화 화면|
|---|---|
|![image](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/ea45c4c8-2a2a-4c51-bb27-4bf21f1f8c64)|![image](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/2899f85b-1dea-4f25-b664-033b187f4f4f)|


### ✅ Pet, Hospital API

User 별로 Pet 정보를 등록하는 API와, 수의사일 경우 Hospital 정보를 등록하는 API를 구현했습니다.

Pet과 Hospital 은 UserID와 외래키로 연결되어있어, 로그인한 정보를 바탕으로 데이터 생성 시 자동으로 UserID를 가져오도록 구현했습니다.

**✔ Hospital 광고 API**

메인 화면에 답변 작성이 우수한 병원들을 광고하고 있습니다. 지금은 답변에 대한 Rank 알고리즘이 없어 ChatGPT를 제외한 병원들에서 랜덤하게 보여주고 있습니다. 

### ✅ Picture, Question, Answer API

우리의 핵심 기능인 사진을 찍어서 AI 진단을 받는 부분과 Q&A 게시판 부분을 담당하는 API 입니다.

**✔ Picture**

Picture는 유저의 PetID를 외래키로 가지기 때문에 사진을 찍어서 올릴 때 자신의 Pet만을 선택하도록 구현했습니다. 사진을 올린 후에는 AI 모델의 결과가 DB에 저장되도록 했습니다.

이후 해당 병명에 대한 설명을 ChatGPT API를 통해 받아와 DB에 저장하고 사용자에게 보여줍니다.

**✔ Question**

Question의 경우 마찬가지로 PictureID를 외래키로 가지기 때문에 해당 유저의 사진에만 접근하여 질문을 등록하도록 하였고, 조회는 누구나 가능하지만 쓰기, 수정, 삭제 기능은 유저 본인만 가능하도록 구현했습니다.

그리고 질문 제목과 내용에 대한 검색기능을 구현해 게시글을 검색할 수 있도록 했습니다.

또한, Question이 등록될 때 해당 질문 내용을 바탕으로(질문 제목, 내용, AI 모델 예상 질환, AI 모델 예상 Confidence) ChatGPT에게 답변을 받을 수 있도록 했습니다. ChatGPT의 경우 응답이 오는 시간이 대략 10초 이상 걸리기 때문에 쓰레드로 구현하여 질문이 등록되는 것에 지연이 발생하지 않도록 했습니다.

**✔ Answer**

Answer의 경우 수의사의 경우에만 답변을 달 수 있도록 구현하였습니다.

### ✅ AWS 서버 배포

AWS 서버 배포는 uwsgi와 Nginx를 통해 배포했습니다. Nginx를 통해 배포했음에도 AI모델이 돌아가고 동시에 다른 API 요청이 들어오면 서버가 죽는 경우가 발생하여, uwsgi 서버 부하 관련 설정을 변경하여 서버가 안정적으로 돌아갈 수 있도록 했습니다.


### ✅ 아키텍처, ERD, Service Flow, UI/UX 흐름도 

|아키텍처|ERD|Service Flow|UI/UX 흐름도|
|---|---|---|---|
|![architecture](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/30362867/44f10a37-b3cb-4b75-b071-91c8d7165565)|![erd](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/b04ee526-a2e9-48a4-86de-5c6ad67e061d)|![서비스 플로우](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/4c5aac06-ddcb-4ef2-bc6b-1dc822403556)|![UI/UX](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/30362867/62fc0f89-6c0c-4285-867f-97055b0bc3c4)|


### ✅ AI
무증상 및 6가지의 피부 질환을 포함하여 7 Class로 분류하는 Flow를 가지고 있습니다.

![ppt4](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/124108688/50c97e0d-654f-4874-816f-b8a67f0dee26)


✔️ **데이터**  
[AI HUB 반려동물 피부 질환 데이터](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=561)를 사용했습니다.  
~~AI HUB 데이터에서 무증상 데이터를 제공하지 않고 있어 **DALLE API**를 사용해 ***무증상 데이터를 생성***하여 학습을 진행했습니다.~~  
- 뒤늦게 반려동물 무증상 데이터가 AI HUB에 업데이트되었고 이를 6월 23일에 확인하여 이후 해당 데이터로 학습을 진행하였습니다.
- 모델의 성능이 좋지 않아 데이터를 확인해본 결과, 중복되는 데이터가 여러 클래스에 들어가있고 개수를 채우기 위해 같은 이미지가 반복되는 등의 문제를 확인했습니다.
- 중복되는 데이터를 삭제하고 각 피부 질환의 특질에 따라 데이터를 직접 분류하는 정제 과정을 거쳤습니다.


✔️ **모델**  
모델은 pretrained InceptionV3를 사용했습니다.<br>


**✔ 모델 선정과정**
- Unet

![ppt3](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/124108688/bdca5b23-8c5f-4a9c-9c15-505effc30062)


마스킹 이미지가 작은 경우 환부로 인식하기 어려운 문제가 있었습니다.<br>
unet에서는 픽셀 주위의 local region(패치)를 입력으로 각 픽셀의 label을 예측하는데, 패치가 작아 환부에 대한 localization 성능은 향상되었으나(98.81%의 accuracy), 네트워크는 매우 작은 context만 보게 되어 context 파악 성능이 매우 떨어지는 것을 확인할 수 있었습니다. <br>
(Unet의 경우 localization accuracy와 context(sementaic information) 사이는 trade off 관계)



- YOLOv5

![ppt7](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/124108688/10d68a2b-7670-4d6b-b229-661c915dbe29)

마찬가지로 바운딩 박스가 작은 경우 객체 인식 자체를 잘 못하는 문제가 있었습니다.<br>
데이터 특성상 세그멘테이션 모델보다는 환부 주변 부위까지 함께 고려하는 분류 모델이 적합​하다고 판단했습니다.


- 사전학습 분류모델 검토

VGG16, MobileNetV3, EfficientNet-B0, Resnet-50, InceptionV3, Inception-resnetV2모델을 검토하였습니다.<br>
![ppt8](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/124108688/32346055-04b9-4afb-a377-f5b620610f2a)

InceptionV3, Inception-resnetV2의 성능이 가장 좋은 것을 확인하였습니다.<br>
모바일 환경을 고려하여 좀 더 가벼운 모델인 InceptionV3로 최종 결정하였습니다.<br>


**✔ 고도화 및 최적화**

- 고도화

데이터셋 정제과정을 거치다보니 줄어든 데이터를 보강하기위해 Data augmentation기법을 활용하였습니다.<br>
모델 학습속도를 높이고 과적합를 방지하기 위해 213layer까지 동결하되 Batchnormalization layer는 동결하지 않았습니다.<br>

- 최적화

기존의 313layer중 layer갯수를 줄여가면서 성능변화를 확인하였습니다.<br>
256layer까지는 밑단을 삭제하여도 유사한 성능을 내는 것을 확인하였습니다.<br>
이를 채택한 결과 기존의 Parameter기준 약 42% 경량화했습니다.(2219만-->1278만)<br>

![ppt11](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/124108688/c7d81b7b-db1d-476b-be63-fbc7fa564a0a)

![ppt12](https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/124108688/9d5f36f7-cab1-49b1-bbef-fda07efbef42)





## 👀 서비스 화면  

<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/4b99a6be-bd6e-4aab-8190-e6d3ca9404c4" width="200"/><br>
어플리케이션을 설치하면 위와 같은 아이콘이 나타납니다.<br><br>
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/453031bb-dfc6-40e4-bbfc-7a384078aa4a" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/3823bb12-4e47-4cc7-be66-c7b934c31d60" width="200"/><br>
아이콘을 눌러 실행하면 로딩화면을 거치고 로그인 화면으로 이동합니다.<br><br>
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/6831d554-d51a-408c-b547-580ba1814a9e" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/f68bbec1-ca2a-4ad0-9c78-f1c211e31c3d" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/cb6b7305-88ff-44b7-b899-0cdc5cbc0156" width="200"/><br>
로그인 화면에서 회원가입 버튼을 누르면 위와 같은 화면으로 이동합니다. <br>
수의사 버튼을 누르면 병원 정보를 입력할 수 있는 화면이 나타나고, <br>
약관 확인하기 문구를 클릭하면 약관을 확인 할 수 있는 화면으로 이동니다.<br><br>
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/c2b0865f-a1df-4cfd-a43f-cd71d4dc6ff0" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/c827ef3c-9d2b-4237-8ada-5f2629fcc256" width="200"/><br>
로그인 화면에서 비밀번호 초기화 문구를 클릭하면 이메일을 입력할 수 있는 화면이 나타나고 <br>
초기화 이메일 보내기 버튼을 누르면 위와 같은 메일을 받을 수 있습니다.<br><br>
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/e004ce20-450d-4ba0-b02a-9f17fa9225f3" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/8be44c4d-68e8-4ef6-88c5-8be4fde07b02" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/72ff5aee-0ddb-4725-b214-e89d2b0267d9" width="200"/><br>
로그인이나 회원가입이 성공하면 메인화면으로 이동합니다.<br>
답변을 많이 한 병원들 중 하나가 하단에 노출됩니다.<br>
설문조사 버튼을 누르면 구글폼으로 이동합니다.<br>
전화상담 버튼을 누르면 휴대전화의 전화화면으로 이동하고 병원 번호로 다이얼을 걸어둡니다.<br><br>
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/456ecb12-efa8-443e-b28e-6ec6f56494cd" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/01d5c984-8e11-467d-9e98-ffb9a0f3095b" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/48c2b3cd-8f2c-4fdc-b6e1-ee0979f2198c" width="200"/><br>
메인화면에서 피부 질환 진단하기 버튼을 누르면 해당하는 동물을 선택하고 <br>
사진을 촬영하거나 갤러리에서 선택하여 이미지를 업로드하는 화면으로 이동합니다.<br>
첫번째 사진은 선택 전 화면, 두번째 사진은 사진촬영 버튼으로 넘어간 화면, <br>
세번째 사진은 사진선택 버튼으로 넘어간 화면, 마지막은 선택 후 화면입니다.<br><br>
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/914a38d9-9988-425b-8b5b-f8488c078c36" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/77db1b3d-5f7c-4964-9262-0d9d0d958894" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/de1d4abe-57b2-4bc8-b883-0ba515a472ff" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/86b4fae0-4844-40a4-9e28-a6b4ed5b9e48" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/899930f3-c76e-4dbe-b37b-56e48c1c67dc" width="200"/><br>
사진을 선택한 후 사진 등록 버튼을 누르면 진단 결과를 보여주는 화면으로 이동합니다. <br>
기다리면 첫번째 사진 처럼 AI 진단 결과를 보여주고 <br>
진단 결과가 나온 후 더 기다리면 두번째 사진처럼 설명이 도착했다고 메시지를 보여줍니다.<br>
이후 AI 진단 버튼을 누르면 진단 결과에 대한 GPT의 설명을 보여줍니다. <br>
진단 결과가 나온 뒤 질문등록 버튼을 누르면 마지막 사진처럼 질문을 등록할 수있는 화면을 보여줍니다. <br><br>
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/f22d2252-0d53-42e1-9bef-7cc96609905f" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/8183778f-0e24-4c42-8e94-d3f3c7165719" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/2c7b2f79-35d2-44f4-98cc-8206dbd01add" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/ca14a236-b88c-48b2-a950-220d69b08c6f" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/f0920683-b443-4a8d-9cf1-9ce93a1e26a1" width="200"/>&nbsp;&nbsp;<br>
질문등록을 마치면 메인화면으로 돌아오게 됩니다.<br>
하단 네비게이션의 질문게시판을 클릭하면 게시판화면이 나타나게됩니다.<br>
게시물을 클릭하면 찍은 사진, AI진단결과, GPT의 설명, 질문이 보여지게됩니다.<br>
답변 또한 하단에 달리게 되는데 우선 GPT가 진단결과와 질문을 기반으로 답변을 해주고 <br>
추가적으로 수의사 분들이 답변을 해주실 수 있습니다.<br>
또한 마지막 사진 처럼 게시글을 검색 할 수 있습니다.<br><br>

<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/5779bca9-2236-43ff-b11d-2f87782a2374" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/d7f5bb1b-8f5a-4702-9910-e4778021ff9c" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/5aefb230-0f73-4436-ab83-ba962bd0b7bf" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/230b3500-9d88-4276-aaaf-e4011de83940" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/6f9ce39d-7506-41b9-9be6-4c8a9afcb963" width="200"/>&nbsp;&nbsp;<br>

메인화면이나 Q&A게시판에서 하단 네비게이션의 내 정보 버튼을 누르면 첫번째 사진과 같은 화면으로 이동합니다.<br>
해당 화면에서 프로필 사진 변경 버튼을 누르면 유저 프로필 사진을 바꿀 수 있고<br>
플러스 모양을 눌러 내 반려동물 추가화면으로 이동하여 반려동물을 추가 할 수 있습니다.<br>
세번째와 다섯번째 사진처럼 수정버튼을 눌러 반려동물, 병원정보 수정화면으로 이동 할 수 있습니다.<br>
이 때 병원 정보는 일반회원이면 보이지 않습니다.<br>
삭제 버튼, 로그아웃 문구, 회원탈퇴 문구의 경우 누르면 확인화면으로 재확인 후 기능을 수행합니다.<br><br>

<img src="https://github.com/AIVLE-School-Third-Big-Project/Team11-Project/assets/76936390/833bf7b7-de8a-428b-8df2-25f3e203a0db" width="200"/><br>
내 정보 화면에서 게시글 작성 내역 문구를 누르거나 메인화면에서 나의 질문 버튼을 누르면<br>
내가 작성한 질문글 목록을 볼 수 있고 클릭하면 QnA게시글 화면으로 이동하여 해당하는 게시글을 보여주게 됩니다.
