# Promptfoo for prompt/model performance evaluation

-------
Promptfoo에 평가를 등록하기 위한 기본 설정 파일을 구성합니다.
## config.yaml

---
평가를 등록하는 데에 가장 핵심이 되는 구성 요소입니다. 사용자는 어떤 프롬프트, 모델, 테스트 데이터에 대해 평가를 진행할 것인지 해당 파일에서 정의합니다. \
config.yaml의 각 필드는 다음과 같습니다. 


### owner

---
사용자의 컬리 email을 입력하세요. \
(예시)
```
owner: sujin.han@kurlycorp.com
```
<br>

### name

---
평가할 테스크 이름을 입력하세요. \
실제 브라우저상에서 평가 항목을 구분짓는 유니크값에 해당하기 때문에 테스크를 대표하는 이름을 권장합니다.\
(예시)
```
name: google-search-translation
```
<br>

### schedule

---
평가를 주기적으로 실행할 배치 작업 스케줄을 cron 형식으로 지정합니다. Cronjob이 생성되며 job 이름은 promptfoo-{name}입니다.\
배치가 필요없다면 None을 입력하세요. \
(예시)
```
schedule: '*/30 * * * *'
```
<br>

### prompts

---
프롬프트를 입력하세요. .txt, .json 등의 파일 혹은 raw text로도 입력 가능합니다.\
(예시) 
```
prompts:
  - id: prompt1.txt
    label: p1
  - id: prompt2.txt
    label: p2
```
<br>

### providers

---
사용할 모델을 입력하세요. 각 프롬프트별로 모델을 다르게 지정할 수도 있습니다. 모델의 파라미터는 config 항목에서 직접 정의할 수 있습니다. \
[모델 종류 확인하기](https://www.promptfoo.dev/docs/providers/)\
(예시)
```
providers:
  - id: openai:gpt-3.5-turbo
    config:
      temperature: 0
      max_tokens: 1024
    prompts:
      - p1
  - id: openai:gpt-4o-mini
    prompts:
      - p2
``` 
<br>

### tests

---
평가할 테스트셋을 불러오는 쿼리 파일을 입력하세요. \
(예시)
```
tests: query/product_name.sql
```
<br>

### defaultTest

---
테스트셋의 pass/fail 기준을 정의하기 위해 공통으로 적용할 assertion을 입력하세요. custom Python function을 사용한다면 ```file://``` prefix를 붙여 파일명을 입력하세요. 그 외에도 deterministic, model-graded assertion도 사용 가능합니다. ([공식문서](https://www.promptfoo.dev/docs/configuration/expected-outputs/)) \
(예시)
```
defaultTest:
  assert:
    - type: python
      value: file://assertion.py
```
prompts, providers, tests, assertion 등에 대한 더욱 다양한 활용 방법은 [promptfoo 공식문서](https://www.promptfoo.dev/docs/configuration/parameters/)를 참조해주세요.

<br>
위 예시에서 설명한 폴더(example) 구성 예시는 다음과 같습니다. config.yaml에 기입된 파일 경로는 모두 동일 경로에 위치해야합니다. 

```
example/
├── query/
│   └── product_name.sql
├── assertion.py
├── config.yaml
├── prompt1.txt
└── prompt2.txt
```



## Unit test

---

- **check_config**: config.yaml에 필요한 필드가 모두 정의되었는지 검사합니다.
- **check_file_path**: config.yaml에 입력된 파일 경로를 검사합니다.
- **check_eval_duplication**: 다른 프로젝트와의 evaluation name이 중복되는지 검사합니다.


## Workflow

---
1. 평가에 사용할 빅쿼리 데이터를 S3에 저장한 후 이를 로드하여 평가를 등록합니다
2. 평가가 완료되면 결과가 빅쿼리에 적재됩니다 (테이블: llmops.promptfoo_result)
3. 


