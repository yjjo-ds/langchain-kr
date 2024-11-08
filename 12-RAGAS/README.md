# RAGAs (Retrieval-Augumented Generation Assessment)

<br>

## Introduction
* RAG application 을 적용하는 것은 다소 간단하지만, 이것을 실제 운영환경에 적절하게 구축하는 것은 어렵습니다. 

<br>

## What is RAGAs

<br>

## Evaluation 

<br>

### (1) 신뢰성(Faithfulness)

---
* 정의 : 생성된 응답이 context로부터 생성되었는지 사실적 정확성을 평가하는 목적입니다. 평가하려는 방법은 생성된 Claim 들이 context로 추론 가능한가?
* 요소 :
  - 질문(question): 사용자로부터 받은 질문. 프롬프트.
  - 문맥(context): 질문에 대한 배경 정보 또는 참고문서 
  - 생성된 응답 (response): 모델이 생성한 답변
  - 주장 집합 (claim): 생성된 응답에서 식별된 주장
 
* 평가방법 :
    $$
      () = \frac{(추론 가능한 주장 수) }{(총 주장 수 )} 
    $$

<br>
    
### (2) 답변 적합성(Answer Relevancy)

---
* 정의 : 생성된 응답이 질문과 얼마나 관련이 있는가
* 요소 :
  - 질문(question): 사용자로부터 받은 질문. 프롬프트.
  - 문맥(context): 질문에 대한 배경 정보 또는 참고문서 
  - 생성된 응답 (response): 모델이 생성한 답변
  - 인공 질문들 : 생성된 응답으로 부터 생성된 여러 질문들
 * 평가방법 :
  - Answer 로 부터 generated Question 생성 → generated Question과 original Question 과의 유사성 판별
  - 생성된 답변에서 여러 인공 질문 생성.
  - 생성된 질문들과 원래 질문 간의 평균 코사인 유사도 계산
<br>

### (3) 문맥 정밀도(Context Precision)

---
* 정의 : RAGAs에서 Context Precision은 검색된 컨텍스트와 기준 답변을 비교
  - 검색된 컨텍스트(retrieved contexts): 질문에 대해 검색 시스템이 반환한 텍스트 조각들입니다. 이 컨텍스트들이 실제로 질문에 대한 답변과 관련이 있는지 평가합니다.
  - 기준 답변(reference answer): 질문에 대해 미리 정의된 정답 텍스트입니다. 이 텍스트는 질문에 대해 기대되는 올바른 정보를 포함하고 있어, 이를 기준으로 컨텍스트의 관련성을 판단합니다.

* 요소 :
  - 질문: 사용자로부터 받은 원래 질문
  - 문맥: 질문에 대한 배경 정보 또는 참조 문서
  - 생성된 응답: 모델이 생성한 답변
  - 관련 항목들: 문맥에서 질문에 대한 답변에 직접적으로 관련된 항목들

* 평가방법 :
  - 
$$
\text{Context Precision@K} = \frac{\sum_{k=1}^{K} (\text{Precision@k} \times v_k)}{\text{Total number of relevant items in the top K results}}
$$

$$
\text{Precision@k} = \frac{\text{true positives@k}}{(\text{true positives@k} + \text{false positives@k})}
$$

Where \( K \) is the total number of chunks in `contexts` and \( v_k \in \{0, 1\} \) is the relevance indicator at rank \( k \).

<br>

### (4) 문맥 재현율(Context Recall)

---
* 정의 :
  - RAG 시스템이 실제 답변과 비교했을 때 얼마나 많은 문장을 정확히 검색해냈는지를 평가하는 것이 목표입니다.
* 요소 :
  - 질문: 사용자로부터 받은 원래 질문.
  - 문맥: 질문에 대한 배경 정보 또는 참조 문서.
  - 생성된 응답: 모델이 생성한 답변.
  - 관련 항목들: 문맥에서 질문에 대한 답변에 직접적으로 관련된 항목들.
* 평가방법
  - Ground truth를 sentence로 쪼갬 → sentence 가 context에 속하는지 판단
  - 정답과 검색된 문맥 간의 관련성을 비교하여 계산
<br>

## Reference
* https://towardsdatascience.com/evaluating-rag-applications-with-ragas-81d67b0ee31a
* https://datainsider.tistory.com/194
* 