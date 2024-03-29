# 2023-02-25 김진중CTO 발표



AI의 뜻은 사람마다 생각하고있는게 다르다.

김진중 CTOAI는 AI가 지금 하지 못하는 것을 하게하는 것이다. 라고 이야기하심.

### Rule-base AI Vs Machine Learning

Rule-base AI : (바나나는) 길고 노랗고 약간 휘었다.

Machine Learning : 바나나 이미지를 많이 넣어줌. 기계가 알아냄.

Rule-base AI는 다른 특징을 하려면 인간이 분석해서 더 넣어줘야함. 머신러닝은 기계가 알아서 해줌.

Rule-base : 인간의 개입 높고, 복잡해질수록 만들기 어려움.

비지도 학습 : 이미지의 특징을 알아내고 인간에게 무엇인지 물어봄.



### Deep Learning?

뇌의 작동방식을 일부 모방해서 만든 것.

Input * Weight = OutPut이다.



### Traditional Machine Learning Vs Deep Learning

딥 러닝은 인간의 개입이 더욱 적어짐.

데이터가 충분한 경우 Deep Learning이 좋고, 양이 적을 경우에는 룰 베이스 또는 전통적인 방식의 머신러닝을 한다.

서비스를 만들 때 룰베이스 또는 전통적인 방식의 머신러닝으로 서비스를하면서 데이터를 얻고, 얻은 데이터를 기반으로 Deep Learning으로 개선하는 방향으로 간다.



## Deep Learning의 발전 역사

1980년대에 현재 뉴럴 넷 방식의 시초가 되었음.

1998년에 비전인식 기술을 개발해냈음. 현재 모든 비전인식 기술의 토대가되었다고 볼 수 있다.

2006년에 Deep Belief Network라는 딥러닝 기술을 개발하고 가능성을 찾았다.

2009년에 ImageNet이라는 dataset이 나왔고, 추후에 사용되는 기술들의 양분이 되었다.

2012년에 유튜브의 모든 데이터를 넣었더니 고양이 사진이 나옴. 이유는 모르지만.

2014년에 seq2seq와 GAN이 나왔고, 텍스트를 생성하는 기법은 현재도 seq2seq를 사용한다고 볼 수 있다. GAN은 이미지를 생성하는 기술이다.

2015년에는 ResNet이 나왔는데 비전인식이 인간을 뛰어넘었다.

2015년에는 TensorFlow가 나왔고

2016년에는 PyTorch가 나왔다. 누구나 쉽게 뉴럴네트워크를 만들 수 있게 되었다.

2016년에 알파고(DQN)가 나왔다. Google NMT & WaveNet로 사람을 뛰어넘는 기술이 대거 등장하기 시작함.

2017년에 Deepfakes 기술이 나오게 되었고 Transformer 모델이 나왔다. GPT와 같은 기술의 기반이 되는 모델이다. NLP가 크게 발전함

2018년에 BERT와 GPT기술이 나온다. BERT가 BigModel의 시초이다. GLUE라는 데이터 셋을 인간 언어 모델 평가를 위해서 만들었는데, 순식간에 인간을 뛰어넘었다.

2019년에는 EfficientNet은 뉴럴네트워크 AI가 만든 뉴럴네트워크다.. Super GLUE라는 데이터 셋을 만들었지만 1년 후 T5에 의해 깨지게되었다.

2020년에는 GPT-3가 나왔습니다. 얘는 BERT는 비교도 안될만큼 엄청 큰 모델이다.

2021년에는 DALL-E가 나왔는데 요청이 들어온 것과 유사한 이미지를 그려주게 된다. Copilot도 나오게되었다.

2022년에 Stable Diffusion을 오픈소스로 풀게되었다. 논란이 많았지만 성능이 너무 뛰어나서 계속 발전중, InstructGPT는 GPT-3가 만들어낸 결과를 사람이 평가를하고 그걸 AI가 학습하는 모델이다. 마지막으로 12월에 ChatGPT가 나오게되었다.



**핵심은 말귀를 알아먹는다는 것이 중요하다.**

## 원리

### 이미지 생성 모델

GAN은 무에서 유를 창조한 것인데 Diffusion은 이미지에서 노이즈로 가는 것을 학습 시키고, 노이즈에서 이미지를 만들어내게 한다. CLIP은 이미지를 텍스트로 설명하는 모델이었는데 이것을 역으로 하여서 텍스트에서 이미지를 만들어냄.

LAION-5B은 인터넷의 이미지를 긁어 만든 모델

### Chat GPT

Generate next word 모델은 여러 단어를 넣고, 다음 단어를 예측해서 반환을 한다. 이게 우리가 사용하는 ChatGPT가 말을 하는 방법이다.

Transformer로 모델크기를 무지막지하게 크게 만드니까 작동함 ㅋㅋ.

모델만 큰 것이 아니고, 데이터도 인터넷데이터와 채팅 데이터 등을 모아서 학습을 시킴. 흔히 하는 말이 지금까지 쌓았던 인터넷의 데이터는 언어모델 학습을 위해서 쌓은 것이 아닌가 라는 말도 함.

RLHF(RL from Human Feedback)

## 어떻게 사용할까?

Prompt Engineering이라고 하는데 AI에게 일을 잘 시키는 방법론. 코딩의 새로운 시대라고 생각한다. 자연어로 일을 시키는 방법이다.

Negative Prompt도 있다. (이렇게 만들지 마라)



GPT-3

- 한 줄씩 데이터를 주는 것이 One-shot, 여러 줄 주는 것이 Few-shot이다.

- 아무 데이터도 주지 않는 것은 Zero-shot인데 빅 모델에서는 된다. 하지만, 정확한 대답이 나올 확률은 조금 낮다.

- Chain of Thought(CoT)는 A는 A다, B는 무엇일까 와 같은 방식이다.

- Zero shot Chain of Thought





