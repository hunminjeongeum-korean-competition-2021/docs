# 2021 훈민정음 한국어 음성•자연어 인공지능 경진대회
## 대회 일정
|일정|내용|기타|
|---|---|---|
|9/15 ~ 9/27|참가지원|
|9/27 ~ 9/30|개별 역량 테스트|
|10/5|예선 참가자 발표|문제별 20팀 선발|
|**10/9 14:00**|**개회**|
|**10/9 ~ 10/24 23:00**|**예선**|문제별 10팀 선발|
|**11/1 ~ 11/30**|**본선**|
|**12/8**|**수상자 발표**|문제별 6팀, 특별상 3팀|
|10/9 ~ 11/30 23:00|인공지능 학습용 데이터 품질 리뷰 제출| 사무국 이메일 제출 (aihub0618@gmail.com)|
|12/16|시상식 및 수상작 리뷰|일시 장소 추후 공지|


## 대회 관련 문서 바로가기
[NSML Document 바로가기](https://n-clair.github.io/vision-docs/_build/html/ko_KR/index.html)

[Dataset 설명](#데이터셋-설명)

[Custom Docker Image 설명]()

[BaseLine Code]()

## 데이터셋 설명
### 문제 1 **[문서요약 Dataset 설명]**
- 텍스트로 구성된 대화문을 읽어 요약문 추론

|전체 크기|
|---|
|436MB|

|파일수|
|---|
|train_data(5)|
|test_data(5)|

#### Train Dataset
- `root_path/train/train_data/`(Edu, Food, Life, Per_Rel, Work)
- train_label은 따로 존재하지 않으며 train_data의 summary를 추출하여 label로 사용

  
    ```
    train_data (json): 학습용 데이터셋
    ├ numberOfItems (ex. 71408)
    ├ data
    │├ header
    ││├ dialogueInfo
    │││├ numberOfParticipants (ex. 2)
    │││├ numberOfUtterances (ex. 16)
    │││├ numberOfTurns (ex. 6)
    │││├ type (ex. 일상 대화)
    │││├ topic (ex. 개인 및 관계)
    │││├ dialogueID (ex. b6d7d466-25f2-5b3e-a55d-fe5ce1e32687)
    ││├ participantsInfo (List)
    │││├ age (ex. 20대)
    │││├ residentialProvince (ex. 부산광역시)
    │││├ gender (ex. 여성)
    │││├ participantID (P01)
    +
    │││├ ...
    │├ body
    ││├ dialogue (List)
    │││├ utterance (ex. 양치하고 올께)
    │││├ utteranceID (ex. U1)
    │││├ participantID (ex. P01)
    │││├ date (ex. 2019-11-08)
    │││├ turnID (ex. T1)
    │││├ time (ex. 13:32:00)
    +
    │││├ ...
    ││├ summary (ex. 양치를 하고 온다고 하며 커피가 치아에 좋지 않다는 이야기를 한다.)
    ```

#### Test Dataset
- `root_path/test/test_data/`(Edu, Food, Life, Per_Rel, Work)
- 5개의 json 파일로 이루어진 test_data의 summary를 추론하는 문제
  
    ```
    test_data (json): 추론용 데이터셋
    ├ numberOfItems (ex. 71408)
    ├ data
    │├ header
    ││├ dialogueInfo
    │││├ numberOfParticipants (ex. 2)
    │││├ numberOfUtterances (ex. 16)
    │││├ numberOfTurns (ex. 6)
    │││├ type (ex. 일상 대화)
    │││├ topic (ex. 개인 및 관계)
    │││├ dialogueID (ex. b6d7d466-25f2-5b3e-a55d-fe5ce1e32687)
    ││├ participantsInfo (List)
    │││├ age (ex. 20대)
    │││├ residentialProvince (ex. 부산광역시)
    │││├ gender (ex. 여성)
    │││├ participantID (P01)
    +
    │││├ ...
    │├ body
    ││├ dialogue (List)
    │││├ utterance (ex. 양치하고 올께)
    │││├ utteranceID (ex. U1)
    │││├ participantID (ex. P01)
    │││├ date (ex. 2019-11-08)
    │││├ turnID (ex. T1)
    │││├ time (ex. 13:32:00)
    +
    │││├ ...
    ││├ summary (ex. 추론 해야하는 요약문)
    ```

#### Test Label
- test_label  (DataFrame 형식, 참가자 접근 불가)

- columns - `["id", "summary"]`

  - `id` - dialogueID와 동일

  - `summary` - 추론한 요약문을 기입하여 제출

  - `id`와 `summary` column을 모두 기입한 DataFrame을 결과물로 구성(**최종 제출 format**)

---
### 문제 2 **[화자인식 Dataset 설명]**
- 매칭된 두개의 음성파일을 읽어 같은 발화자인지 다른 발화자인지 추론

|전체 크기|
|---|
|42.2GB|

|파일수|
|---|
|train_data(239,378)|
|test_data(1,221)|

#### Train Dataset
- `root_path/train/train_data/` (239,279개의 wav 파일 *확장자 없는 레이블 형태)
  
    ```
    idx_000001
    idx_000002
    idx_000003
    idx_000004
    ...
    idx_239375
    idx_239376
    idx_239377
    idx_239378
    ```

#### Train Lable

- `root_path/train/train_label`

- `train_label (DataFrame 형식, 238,822rows)`

  - columns - `["file_name", "file_name_", "label"]`

  - `file_name` - train_data 폴더에 존재하는 임의의 wav파일명 (ex. idx_000001)

  - `file_name_` - train_data 폴더에 존재하는 임의의 wav파일명 (ex. idx_000002)

  - `label` - 두 파일에 녹음된 발화자가 동일인물이면 1, 다른인물이면 0 (Binary Classification)


#### Test Dataset
- `root_path/test/test_data/wav/` (1,221개의 wav 파일 *확장자 없는 레이블 형태)

    ```
    idx_000001
    idx_000002
    idx_000003
    idx_000004
    ...
    idx_001218
    idx_001219
    idx_001220
    idx_001221
    ```

- `root_path/test/test_data/test_data`

- `test_data (DataFrame 형식, 30,493rows)`

    - columns - `["file_name", "file_name_]`

    - `file_name` - wav 폴더에 존재하는 임의의 wav파일명 (ex. idx_000001)

    - `file_name_` - wav 폴더에 존재하는 임의의 wav파일명 (ex. idx_000002)

    - test_data에 `label` column을 추가하여 추론값을 대입 (최종 제출 format)


---
### 문제 3 **[음성인식-명령어 Dataset 설명]**

- 음성 파일과 매칭되는 Text를 추론(aka.받아쓰기)

|전체 크기|
|---|
|33GB|

|파일수|
|---|
|train_data(113,347)|
|test_data(10,374)|

#### Train Dataset
- `root_path/train/train_data/`(113,347개의 wav 파일 *확장자 없는 레이블 형태)
  
    ```
    idx000001
    idx000002
    idx000003
    idx000004
    ...
    idx113344
    idx113345
    idx113346
    idx113347
    ```

#### Train Label
- `root_path/train/train_label`

- `train_label (DataFrame 형식, 113,347rows)`

  - columns - `["file_name", "text"]`

  - `file_name` - train_data 폴더에 존재하는 wav파일명 (ex. idx000001)

  - `text` - train_data 폴더에 존재하는 wav파일과 매칭되는 text 정보 (ex. 훈민정음에 스며들다)


#### Test Data
- `root_path/test/test_data/`(10,374개의 wav 파일 *확장자 없는 레이블 형태 / train_data와 파일명 형식이 다름에 주의)
  
    ```
    idx_000001
    idx_000002
    idx_000003
    idx_000004
    ...
    idx_010371
    idx_010372
    idx_010373
    idx_010374
    ```

#### Test Label
- `root_path/test/test_label` (참가자 접근 불가)

- `test_label (DataFrame 형식, 10,374rows)`

- columns = `["file_name", "text"]`

  - `file_name`- test_data 폴더에 존재하는 wav파일명 (ex. idx_000001)

  - `text` - test_data 폴더에 존재하는 wav파일과 매칭되는 Text 정보 (ex. 훈민정음에 스며들다)

  - 참가자 분들은 `file_name`과 `text` column을 모두 기입한 DataFrame을 결과물로 구성 (최종 제출 format)

---
### 문제 4 **[음성인식-자유대화 Dataset 설명]**
- 음성 파일과 매칭되는 text를 추론

|전체 크기|
|---|
|35.44GB|

|파일수|
|---|
|train_data(228,913)|
|test_data(9,436)|

#### Train Dataset
- `root_path/train/train_data/`(228,913개의 wav 파일 *확장자 없는 레이블 형태)
  
    ```
    idx000001
    idx000002
    idx000003
    idx000004
    ...
    idx228910
    idx228911
    idx228912
    idx228913
    ```

#### Train Label
`root_path/train/train_label`

- `train_label (DataFrame 형식, 228,913rows)`

  - columns - `["file_name", "text"]`

  - `file_name` - train_data 폴더에 존재하는 wav파일명 (ex. idx000001)

  - `text` - train_data 폴더에 존재하는 wav파일과 매칭되는 Text 정보 (ex. 훈민정음에 스며들다)
    

#### Test Dataset
- `root_path/test/test_data/`(9,436개의 wav 파일 *확장자 없는 레이블 형태/ train_data와 파일명 형식이 다름에 주의)

    ```
    idx_000001
    idx_000002
    idx_000003
    idx_000004
    ...
    idx_009433
    idx_009434
    idx_009435
    idx_009436
    ```

#### Test Label
- `root_path/test/test_label` (참가자 접근 불가)

- `test_label (DataFrame 형식, 9,436rows)`

  - columns = `["file_name", "text"]`

  - `file_name` - test_data 폴더에 존재하는 wav파일명 (ex. idx_000001)

  - `text` - test_data 폴더에 존재하는 wav파일과 매칭되는 Text 정보 (ex. 훈민정음에 스며들다)

  - 참가자 분들은 `file_name`과 `text` column을 모두 기입한 DataFrame을 결과물로 구성(최종 제출 format)
