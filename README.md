# LLM is All You Need

## 목적
- Gemini를 이용하여 PDF를 업로드 후, RAG 구현하여 PDF에 있는 내용을 질의 (1차, 2차)
- Palm2를 이용하여 RAG 구성 (3차)



## 한줄 소개
- Langchain을 사용하여 LLM 모델인 Palm2와 Gemini의 RAG가 Flask위에서 구동 가능함!



## 구성원
- DONGWEONSHIN
- Jeon0866 JinHwan
- wjseoduq
- rnansfyd


## 프로젝트 기간
- 1차 시도
- 2023.12.29 ~ 2024.1.5 (약 1주일)
- 2차 시도
- 2024.1.15 ~ 2024.1.16
- 3차
- 2024.1.17 ~



## 실행 방법 (Gemini)
1. mv .env.local .env
2. fill in GOOGLE_API_KEY
3. pip install -r requirements.txt
4. flask run



### Palm2 실행 시 추가사항
1. .env 파일에 PROJECT_ID, LOCATION 을 추가
2. .env 파일에서 FLASK_APP의 타겟을 app_PaLM2.py 로 변경
3. pip install -r requirements.txt (추가된 부분이 있음)
4. flask를 실행



### Vertex AI - gcloud CLI 확인 방법
1. gcloud CLI 설치

- https://cloud.google.com/sdk/docs/install?hl=ko#linux

````
gcloud init
````

2. auth 확인

- https://cloud.google.com/python/docs/setup#linux

````
gcloud auth application-default login
````


## 폴더 구조
```
├── IMG
├── PDF_DN_FOLDER
├── __pycache__
├── static
│   ├── chat.js
│   └── style.css
├── templates
│   ├── admin.html       (변경 - 24.1.24)
│   ├── diffusion.html   (추가 - 24.1.24)
│   ├── index.html       (추가 - 24.1.24)
│   └── museum.html      (추가 - 24.1.24)
├── .env
├── .gitignore
├── app_PaLM2.py (추가 - 24.1.17)
├── app.py
├── README.md
├── requirements.txt
├── ver1_app.py
└── ver1_requirements.txt
```



## 호출 함수
예) http://127.0.0.1:5000/chat
1. **/**
- flask를 실행하면 기본적으로 호출 되며, chat 화면을 보여 준다.
2. **/chat**
- 입력창에 질의를 입력 하면 LLM과 대화를 한다.
3. **/savePdf**
- PDF 파일을 선택 하고 저장 한다.
4. **/chatWithPdf**
- 저장된 PDF를 기반으로 질의를 한다.



## 1차 시도 - Google AI Gemini 와 Langchain으로 RAG 구성 (문제점 포함)
1. 2023년 12월에 출시된 Gemini는 Google AI Gemini와 Vertext AI Gemini 버전 2가지가 있습니다.
2. 각각의 버전은 코드가 동일하지 않고, 현재 (2024년 1월 기준) 샘플 코드가 많지 않습니다
3. LangChain도 Gemini를 대응하기 위한 샘플 코드가 적고, 그나마 기본적인 코드만 존재 합니다.
4. 1차 RAG 시도 시 버전은 아래와 같습니다.
```
langchain==0.0.353
google-generativeai==0.3.2
langchain-google-genai==0.0.5
```
5. 1차 RAG 시도시에는 PDF 업로드 후, PDF 내용을 질의 하면 10번중 1번만 응답을 합니다.
6. 1차 파일을 기록을 남기고자 아래의 파일을 접두어 'ver1_'를 추가하여 변경합니다
```
app.py
requirements.txt
```


## 2차 시도 - Google AI Gemini 와 Langchain으로 RAG 구성 (성공)
1. 2차 시도 버전은 아래와 같습니다.
```
langchain==0.1.0
google-generativeai==0.3.2
langchain-google-genai==0.0.6
```
2. 벡터 스토어를 FAISS로 변경 하고, retriever와 chain을 단순화 하여 수정하였습니다.
3. Google AI Gemini 코드와 Vertext AI Gemini를 인지하고 Google AI Gemini로만 진행 하였습니다.
4. RAG 구성을 완료 하고 PDF 업로드 후 질의 하면, PDF의 내용에 대해 답을 합니다.
5. 1차 때 구성한 내용들을 바탕으로 필요한 Gemini 샘플 코드를 분류 하여 분석 하였고, 임베딩과 질의, 벡터 스토어의 개념을 재학습 하였습니다. 그리고 Langchain의 버전이 그나마 안정화 되어 2차 때 RAG가 성공한 요인으로 파악 됩니다.
6. 테스트를 위해 postman을 사용하여 테스트 하였음



## 3차 - Palm2에 RAG 구성 (성공)
1. 아래의 사항을 추가 합니다.
```
google-cloud-aiplatform==1.39.0
langchain-google-vertexai==0.0.1
```
2. Palm2 모델을 사용시 .env의 타겟을 변경 합니다. ("Palm2 실행 시 추가사항" 부분 참고)
3. langchain-community 0.0.12에서 사용하던 VertexAIEmbeddings class가 사라질 예정이라 수정이 필요 했음
![VertexAIEmbeddings](IMG/warning_2024-01-17_16-07-10.png)




## 개발 중인 사항
1. 화면에서 PDF 업로드 후 질의를 시작하기 위해 js에서 /chatPdf로 함수를 변경해서 호출 해야 함
2. 화면 UI/UX를 변경할 예정임







