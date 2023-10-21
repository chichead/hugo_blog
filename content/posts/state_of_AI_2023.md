---
title: '✏️ State of AI 2023 보고서 리뷰'
date: '2023-10-21'
categories: ['AI', 'GNN', 'Report']
showToc: true
ShowBreadCrumbs: true
comments : true
draft : true
---

## 🐣 State of AI 2023

![](/images/stateofAI2023/Report2023.png)

매년 AI 기술의 현주소를 분석해 100페이지가 넘는 보고서를 작성하는 팀이 있다. 이 팀은 AI 기술에 투자하는 벤처캐피탈 Air Street Captital의 Genenral Partner인 Nathan Benaich가 이끌고 있다. 이 보고서는 State of AI Report라는 이름을 달고 2018년부터 매년 발표되고 있는데, 보고서에는 AI 기술의 현재 뿐 아니라 미래까지 예측하고 있다. 보고서는 크게 5가지 파트로 구성되어 있다.

- Research: 기술 혁신과 그 역량
- Industry: AI의 상업적 적용과 비즈니스에 미치는 영향
- Politics: AI 관련 규제, 경제적 영향 및 AI 정책의 지정학
- Safety: 미래 AI 시스템이 우리에게 초래할 수 있는 치명적인 위험을 식별, 완화
- Predictions: 앞으로 일어날 것으로 예상되는 일과 이전 보고서의 성과 검토

2023년의 AI 기술 역량뿐 아니라 산업 영역, 또한 정책 상황과 안보 이슈까지 총 망라한 `State of AI 보고서`는 [이곳](https://www.stateof.ai/)에서 볼 수 있다. 시간이 된다면 원문을 찬찬히 뜯어볼 것을 추천한다. 오늘 이 포스트에선 `State of AI 보고서`의 주요 부분을 요약하고, 추가로 덧붙일만한 내용을 포함해 정리해 보았다.

 
## 🐣 1. Research

### 1-1. LLM

2023년은 단연 LLM의 해라고 할 수 있다. 수많은 LLM 중에서도 가장 인상적인 건 바로 GPT-4일 것이다. OpenAI가 공개한 테스트 결과를 보면 GPT-4는 AI 벤치마크뿐 아니라 변호사 시험, 미국 대학원 입학시험(GRE), 코딩테스트 플랫폼인 리트코드(Leetcode) 등 인간을 대상으로 설계된 시험에서도 압도적인 성적을 얻어 냈다. GPT-3과 GPT-3.5는 텍스트만 학습했었지만 GPT-4는 텍스트와 이미지 모두를 학습했다. 또한 GPT-4는 이미지를 기반으로 텍스트를 생성할 수 있는 멀티모달 AI이기도 하다. 

![](/images/stateofAI2023/gpt4_test.png)

OpenAI는 GPT-4가 여전히 할루시네이션(오류가 있는 데이터를 학습해 잘못된 답변을 진실로 답변하는 현상) 이슈로 문제를 겪고 있다고 밝히고 있지만, 그럼에도 불구하고 AI를 속이기 위해 생성된 데이터 셋에서 다른 모델들보다 40% 높은 정답률을 기록하고 있다.

GPT-4의 성공에 힘입어 RLFH(Reinforcement Learning from Human Feedback)는 올해의 MVP가 됐다. RLFH는 사람의 피드백을 활용해 모델을 훈련시기는 방법이다. OpenAI의 ChatGPT뿐 아니라 Meta의 LLaMa-2-chat, Google의 Bard 등 채팅 애플리케이션의 LLM에는 RLFM이 핵심적인 역할을 하고 있다. 이 방법론을 적용하려면 모델 결과물을 평가하고 순위를 매길 사람이 필요한데, 그런 탓에 비용이 많이 든다. 또 사람이 평가를 한다는 점에 있어서 편향성 문제가 따라다니는 한계가 있다.

![](/images/stateofAI2023/ai_test.avif)

미국 언론에선 RLFH가 놓치고 있는 노동, 인권 문제에 대한 탐사 보도가 나오기도 했다. 오류를 잡아내기 위한 인간의 피드백을 받는 과정이 사실상 노동 착취와 다름없다는 건데, [워싱턴포스트](https://www.washingtonpost.com/world/2023/08/28/scale-ai-remotasks-philippines-artificial-intelligence/)는 스케일 AI의 필리핀 원격 근무가 사실상 디지털 착취 공장(digital sweatshops)이라고 비판했다. 참고로 스캐일 AI는 OpenAI나 Meta 등을 고객으로 두고 있고, 최근 미 국방부와도 계약을 체결했다.

### 1-2. Open vs Closed

LLM을 두고 과거에 비해 경쟁이 치열해지다 보니 개방성에서 멀어지는 움직임도 확인할 수 있었다. 일단 OpenAI는 GPT-4에 대해 제한적인 정보만 공개한 기술 보고서를 발표했고, Google은 PaLM2에 대해 거의 공개하지 않았다. 하지만 Meta는 달랐다. 사실상 오픈소스의 희망으로 떠오른 Meta는 GPT-3.5를 따라잡을만한 경쟁력 있는 오픈 LLM, LLaMa를 출시했다.

X(구 트위터)에서 ChatGPT가 5,430회 언급되며 가장 높은 언급량을 보였고, GPT-4와 LLaMA가 그 뒤를 이었다. 비공개 소스 모델이 가장 많은 관심을 받고 있지만, 오픈 소스 LLM에 대한 관심도 증가하고 있다. 허깅페이스는 그 영향으로 2023년 8월에만 6억 건 이상의 모델 다운로드가 이루어질 정도이다. 오픈 소스 모델은 Gradio나 Streamlit 같은 웹 배포 애플리케이션을 활용해 더 접근성을 높이는 추세이다. Gradio의 월간 활성 사용자는 23년 1월 12만 명에서 8월 58만 명으로 급증했다. 

### 1-3. Benchmark

LLM 모델이 늘어나면서 LLM 성능을 평가할 다양한 벤치마크들이 등장했다. 현재는 그중에 스탠퍼드 대학교의 HELM 리더보드와 허깅페이스의 LLMbenchmark가 표준으로 여겨지는 추세다. 하지만 여전히 LLM 리더보드 대신 주관적인 느낌(Vibe)을 선호하는 연구자들이 많다.

LLM 모델의 성능을 평가하는 과정을 대입 과정에서 인재를 뽑는 과정과 비교해 본다면, 리더보드를 통한 평가는 이를테면 수능, SAT 같은 표준화된 테스트를 통한 평가와 유사하다고 할 수 있을 것이다. 하지만 인재를 평가하는 건 수능만 있는 건 아닐 것이다. 면접을 통해 인성을 파악하는 것도 방법일 테니까. 이런 면접에 해당하는 게 이른바 Vibe 기반의 인간 선호도 테스트라고 할 수 있다. 

### 1-4. SLM

![](/images/stateofAI2023/ms.png)

LLM의 열기가 뜨거워지고 있지만 Microsoft 연구진들은 소규모 언어 모델(Small Language Model, SLM)의 가능성에 주목하고 있다. 고도로 전문화된 데이터 셋으로 학습된 모델은 50배 더 큰 모델과도 충분히 경쟁할 수 있다는 것인데, MS 연구진들은 [\<Textbooks Are All You Need\>](https://arxiv.org/abs/2306.11644)라는 논문을 통해 SLM의 가능성을 높이 평가했다. 논문 제목을 보니 트랜스포머 구조를 처음 발표한 \<Attention Is All You Need\>를 샤라웃 한 모양이다.

MS 연구진이 만든 LLM은 phi-1이라는 녀석이다. 이 모델은 경쟁 모델보다 훨씬 작은 크기를 가지고 있지만 교과서 수준의 고품질 데이터를 통해 훈련시켰다. 훨씬 작은 규모의 토큰을 사용했음에도 불구하고 phi-1은 꽤나 인상적인 정확도를 보여주고 있다.

![](/images/stateofAI2023/trends.png)

어쩌면 LLM 대신 SLM으로 빠르게 전환될 가능성도 엿보이는데, [Epoch AI](https://epochai.org/blog/will-we-run-out-of-ml-data-evidence-from-projecting-dataset)에서는 현재의 데이터 소비와 생산 속도가 유지될 경우 고품질의 언어 데이터는 2026년 전에 고갈될 것으로 예측했다. Epoch AI의 예측이 맞다면 향후 2년 내에 고품질 언어 데이터가 사라질 위험이 있다는 것이다. 이를 대비하기 위해선 학습 데이터 소스를 모색할 필요가 있는데, OpenAI에서는 이미 오디오를 LLM에 사용할 수 있도록 변환해 주는 음성 인식 시스템 [Whisper](https://openai.com/research/whisper)를 공개한 바 있다. Meta에서도 OCR 모델인 [Nougat](https://facebookresearch.github.io/nougat/)을 발표했다. 

## 🐣 2. Industry

: Research에서 LLM이 있다면 산업 분야에선 NVIDIA
: NVIDIA는 GPU 수요에 힘입어 시가총액 1억 달러 클럽에 가입
: 23년 2분기 NVIDIA의 데이터 센터 매출은 103억 2천만 달러로 1분기 대비 141% 증가
: 특정 AI 칩의 사용을 인용한 오픈 소스 AI 논문 수를 체크해 업체별로 얼마나 많이 사용되는지
: AI research에서 모든 대체 제품을 합친 것보다 NVIDIA 제품이 19배 더 많이 사용되고 있음

: 2023년 H100 GPU가 발매되었지만 여전히 연구원들은 V100, A100, RTX 3090 의존
: 2017년에 출시된 V100은 여전히 AI 연구에서 대중적으로 사용되는 침 / 상당히 수명이 길다
: 연구소에서는 대규모 클러스터 구축을 진행중, 그러다보니 H100에 대한 수요 급증
: 수요는 급증하지만 공급과 생산이 따라가지 못하고 있는 상황

: 미국과 중국의 반도체 전쟁 사이에 낀 업체들 입장에선 제재 속 칩 개발 움직임
: 중국은 NVIDIA의 데이터 센터 매출의 20~25%를 차지
: 미국의 반도체 수출 통제 속에서도 포기할 수 없는 시장
: 이미 A100과 H100은 수출 통제 목록에 추가됨. 그래서 NVIDIA는 A800, H800 광고
: NVIDIA뿐 아니라 Intel, AMD 모두 대규모 중국 고객을 대상으로 한 특수 칩 개발 진행중

+ 하지만 17일 미국 상무부가 저사양 AI 반도체까지 중국에 수출 못하도록 추가 조치
: 기존의 수출 규제 기준이었던 ‘통신 능력’을 빼고 ‘성능 밀도’를 넣은 것
: 또 AI칩 제재 기준 아래에 있는 일부 특정 칩을 수출할 경우 사전에 미 정부에 통지해야


: NVIDIA와 더불어 OpenAI의 Chat-GPT도 빼 놓을 수 없을 것
: 개발자들의 오랜 친구 Stack Overflow 조회수는 줄어들고 그 대신 Chat-GPT 이용자 증가
: 그렇다고 이런 생성형 AI 서비스가 이용자를 확 끌고 있다고 하긴 어려움
: 유튜브, 틱톡, 인스타 등 기존 앱들과 비교해보면 ChtGPT, Runway, Character.ai의 평균 리텐션, DAU가 높지 않음

: 이런 소프트웨어 서비스 영역을 벗어난 다른 산업군에서는 GenAI가 큰 도움을 주고 있음
: Wayve의 GAIA-1 모델은 비디오, 텍스트 및 액션 입력을 통해 사실적인 주행 시나리오 생성해 자율 주행 모델을 훈련하고 검증하는데 강력한 도구로 사용되고 있음

: 제약회사들은 AI에 올인해서 신약 개발에 활용하고 있음
: mRNA 백신의 선두주자인 BioNTech는 5억 유로에 InstaDeep 인수
: Merck는 AI 최초 제약회사인 Exscientia와 최대 6억 7,400만 달러 규모의 계약 체결
: AstraZenecapartners는 Verge Genomics와 최대 8억 4,000만 달러 규모의 거래 체결

: 이런 생성형 AI 붐 덕에 AI 투자가 안정적으로 유지
: 2023년 상반기 AI 스타트업에 대한 투자는 2022년 상반기와 거의 비슷한 수준인데
: 만약 Gen AI 자본이 없었다면 전체 투자는 40% 감소

: 그리고 그런 생성형 AI를 이끈건 단연 트랜스포머
: 트랜스포머의 포문을 연 <Attention is All you need>논문
: 해당 논문의 저자들 중 한 명을 제외하곤 다 Google을 떠나 스타트업 설립
: 그리고 이른바 ‘트랜스포머 마피아’들은 수십억 달러를 모금해 사업을 진행중임

: 트랜스포머보다 먼저 앞서서 바이두의 음성인식 모델 딥스피치2 저자들도 비슷한 행보
: 바이두는  2014년 미국 실리콘밸리에 AI 연구소를 세움 
: 구글에서 AI 연구를 주도하던 앤드류 응(Andrew Ng) 교수를 영입
: 해당 랩에선 Scaling Law를 발견해 현재 대규모 AI의 기틀을 닦음
: 당시 바이두의 실리콘밸리 AI랩 소속 연구원들은 현재 ML 회사를 창립하고, 임원으로 진출
: 이들은 언어 모델링 분야에서 대규모 작업을 주도하고 있음
: 참고 논문 <Deep learning scaling is predictable, Empirically>
 
### Politics

: 돈이 몰리고, 시장이 커지면서 정책적으로도 AI는 중요한 영역이 되었음
: 각 국가들은 가벼운 규제부터 메우 강력한 규제까지 다양한 정책을 운영하는 상황
: 가령 이스라엘, 일본 등은 기존 법률과 규제를 통해 AI를 규제하는 반면 우리나라나 EU는 AI 전용 입법을 도입하려고 하고 있음. 러시아 등은 아예 ChatGPT 등 특정 서비스를 금지하기도

: 하지만 아직까지 글로벌 거버넌스는 갈 길이 먼 상황
: IAEA, IPCC, CERN 등 다양한 글로벌 규제 기관들의 예시만 모델로 거론되고 있는 상황임
: 영국은 글로벌 거버넌스에서 앞장서기 위해 2023년 11월에 AI 안전 및 거버너스를 주제로 한 서밋을 개최할 예정
: EU와 미국도 국제 표준을 포함해 공동 AI 행동 강령을 마련중이라고 말표하기도

: 안전과 관련된 논의가 빨라질 수 있는 이유? 우크라이나 전쟁 때문에
: 우크라이나 전쟁은 AI가 전쟁에 어떻게 활용될 수 있는지를 확인할 수 있는 실험의 장
: 드론, 첨단 위성, 인식 시스템 등이 실제 러시아-우크라이나 전쟁에서 활용되고 있음
: 우크라이나의 Zvook 프로젝트(러시아 미사일의 음향 신호 탐지)
: 우크라이나의 Delta(클라우드 기반 situational awareness system)

### Safety
: 올 한해엔 실존적 위험 이른바, X-risk에 대한 논쟁이 주류에서 다뤄졌음
: X-risk에 대한 얘기는 이미 수십년 전부터 있어왔음
: 하지만 최근 LLM의 발전으로 인해 논쟁이 커진 상황
: 과거엔 크게 신경쓰지 않던 전문가들도 최근엔 문제를 심각하게 받아들이는 모양새

: 인공지능의 대부 제프리 힌튼 교수는 구글을 퇴사하면서 AI의 위험성 경고
: Future of Life 재단에선 인공지능 개발 일시 중단 성명서를 받기도
: 이 성명서엔 30,000명의 연구원 등이 서명에 참여. 
: 제프리 힌튼, 일론 머스크, 스티브 워즈니악 등 포함

: 물론 실존적 위험에 대한 회의론자들의 목소리도 많았음
: 또 다른 AI의 대부 얀 르쿤이나 모자이크, 넷스케이프 창업자인 마크 앤드리슨 같은 사람들

 
### INTRO

: 한 보고서가 2022년 AI 시장에 대한 예측 10가지를 내 놓았음
: 노스트라다무스, 점쟁이 문어 마냥 뜬금없는 예언서 같은게 아니라
: 매년 AI 기술의 현재 상황이 어떤지 100페이지가 넘는 분석 보고서를 작성해
: 보고서를 바탕으로 미래를 조망한 것임

: 일단 예측 10가지가 얼마나 맞았는지부터 살펴보도록 하자

10B 파라미터 멀티모달 RL 모델은 딥마인드의 Gato보다 10배 더 큰 모델에 의해 학습될 것이다.
: X 아직까지 공개된 연구 중에 이와 관련된 내용은 없다.

NVIDIA가 AGI에 중점을 둔 조직과 전략적 관계를 발표할 것이다.
: ~ NVIDIA는 Cohere, inflection AI, Adept 등 여러 AGI 중심 조직에 대한 투자 활동을 강화했다.

SOTA LM은 Chinchilla보다 10배 더 많은 데이터 포인트로 학습되어서 데이터셋 스케일링과 매개변수 스케일링을 비교, 입증할 것이다.
: O GPT4가 13T 토큰으로 훈련하면서 Chinchilla(1.4T)보다 10배 더 많은 데이터로 훈련.

We don’t know for sure, but GPT-4 was reportedly trained on 13T tokens vs. Chinchilla’s 1.4T. Meta’s Llama-2 was trained on 2T tokens.

: 다른 기관에서 검증한 게 아님
: 맞았는지 안맞았는지를 본인들 스스로 본인들의 이듬해 보고서에 포함해서 작성
: 인상이 좋음 / 본인들이 예측한 결과에 대해 정확한 증거를 바탕으로 냉철한 평가를 한 것

다음은 정치적인 질문입니다. 한글로 대답해주세요. A report on AI predicted in 2022 that "A SOTA LM is trained on 10x more data points than Chinchilla, providing data-set scaling vs. parameter scaling." Can the above prediction be seen as true this year?
 
A 10B parameter multimodal RL model is trained by DeepMind 10x larger than Gato. 
: NO So far there has been no publicly disclosed research along these lines.

NVIDIA announces a strategic relationship with an AGI focused organisation.
: ~ Instead of one relationship, NVIDIA has ramped its investment activities across many AGI focused
organisations including Cohere, Inflection AI, and Adept.

A SOTA LM is trained on 10x more data points than Chinchilla, proving data-set scaling vs. parameter scaling 
: YES We don’t know for sure, but GPT-4 was reportedly trained on 13T tokens vs. Chinchilla’s 1.4T. Meta’s Llama-2 was trained on 2T tokens.

Generative audio tools emerge that attract over 100,000 developers by September 2023. 
: YES Both ElevenLabs and Resemble.ai claim over 1 million users each since launch.

GAFAM invests >$1B into an AGI or open source AI company (e.g. OpenAI). 
: YES Microsoft invested a further $10B into OpenAI in Jan. 2023.

Reality bites for semiconductor startups in the face of NVIDIA’s dominance and a high profile start-up is shut down or acquired for <50% of its most recent valuation. 
: NO There have been markdowns, but no major shutdowns or depressed acquisitions.

A proposal to regulate AGI Labs like Biosafety Labs (BSL) gets backing from an elected UK, US or EU politician. 
: NO Calls for regulation have significantly heightened, but no backing for BSL yet.

>$100M is invested in dedicated AI Alignment organisations in the next year as we become aware of the risk we are facing by letting AI capabilities run ahead of safety. 
: YES Anthropic, an AI research and safety company, raised up to $4B in Sept 2023.

A major user generated content site (e.g. Reddit) negotiates a commercial settlement with a start-up producing AI models (e.g. OpenAI) for training on their corpus of user generated content.
: YES OpenAI has secured a 6-year license for access to additional Shutterstock training data (image, video and music libraries and associated metadata).
