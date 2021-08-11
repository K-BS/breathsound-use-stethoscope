# breathsound-use-stethoscope
Breath sound(binary classification)

### History

#### 2021.08.11(수)

Audioset (https://research.google.com/audioset/) 에서 학습된 CNN14모델의 가중치를 가져와
우리의 호흡음 데이터에 Transfer learning.

그냥 트레이닝 어큐러시가 하늘을 찌름.. 바로 1에 근접해버림 ㅋㅋ

데이터 수에 비해 모델이 너무 크기도 하구.. 그래서그런가? 했는데

하이퍼 파라미터도 대충 집어넣었는데 테스트 어큐러시가 0.8 중반으로 꽤 높음.

역시 프리트레인 모델 쓰는것이 최고다 싶었음. 

#### 2021.08.04(수)

흠... 믹스업이 언더피팅을 벗어나질 못함.

일반 CNN대신 다른걸 써볼까 싶어가지구

ResNet50을 적용시켰는데 언더피팅에서 벗어나질 못함...

왜지????

이게 이상한게 믹스업을 빼고 레지넷으로 돌려버리면 진짜 3에폭만에 트레인어큐러시가

0.9 찍어버리는데 그만큼 모델이 많이 깊어서 좀 빨리 트레인어큐러시 오르는거같은데



#### 2021.08.03(화)

일단 믹스업을 적용은 했음.

트레인시간이 늘어난 것으로 보아 제대로 적용된 것으로 확인됨.

문제는 언더피팅이 일어나고 있음. 트레인셋에서 어큐러시가 안늘어나고,

테스트셋에서의 어큐러시가 더 높음.



#### 2021.08.02(월)
Pilot Model 완성을 위해 GridSearchCV로 최적의 파라미터를 찾음.

정리하면, Dataset을 8:2(Training, Test)로 쪼갬.

그 후, Training Dataset을 5-fold cross validation으로 검증함과 동시에

Grid Search로 최적의 파라미터를 탐색함.

그리고 f1 스코어 및 Imbalance Data에서 사용되는 다른 스코어들 출력, Confusion Matrix작성.

최종적으로 Testset기준 Accuracy 0.8868, F1-score 0.8125 등을 달성.

#### 2021.07.26(월)
적용한 것들을 정리해봄.

1. 제로패딩 대신 반복(repeat)패딩? 함

2. dataset을 처음부터 트레이닝, 발리데이션을 8:2로 쪼개고 시작함(휘징이랑 난휘징 비율 맞췄음)

3. 하이퍼파라미터 이것저것 만지니까 어큐러시 0.8까진올라감

4. amplitution? 필터? 디더? 스펙트로그램으루 뽑기이전 단계에서 할수있는 것들 torchaudio에 있는 패키지로 적용해봄. 어큐러시 0.82까지 올라감.



#### 06.19 ~ 07.11
#### 2021.06.19(토)
breath_sound_pilot_model.ipynb파일

호흡음 관련 Pilot Model 작성.

Feature = MFCC

Random sampling 기법 사용

Model = 간단한 CNN

train, validation set 비율 0.8 / 0.2 데이터 수가 너무 적어서 이이상은 쪼개기 힘듦..

test set까지 쪼개야한다면 데이터 수가 너무 적어져서 고민.

best validation accuracy = 0.91정도

#### 2021.06.23(수)
breath_sound_smote.ipynb파일

Healty data, Wheezing data 간 불균형이 있었음.

그래서 SMOTE 기법으로 데이터의 Imbalance를 해소하고 돌려봄.

그다지.. 성능변화는 없었음.


#### 2021.06.24(목)
breath_sound_sr.ipynb파일

치명적인 실수를 발견함. 기본 오디오 sample rate가 48000hz인데,

librosa.load로 불러오던 중 디폴트값인 22050hz로 down sampling 되었던 것!

sr=48000 주고 다시 돌려봄. 성능이 소폭 상승함.

best validation accuracy = 0.92정도

#### 2021.06.29(화)
new_pilot_model.ipynb

ㅋㅋ 그냥망함.

train, validation 셋을 random sampling 이후에 나누었는데,

교수님께서 train, validation 셋을 먼저 나누고 random sampling을 하는 것이 좋을 것이라고 조언을 해주셔서

해봤더니 validation accuracy 0.7로 급락함.

random sampling 후에 train, val 셋을 나누었을땐 비슷한 데이터가 꽤 많았을 것으로 추정됌..

.....

#### 2021.06.30(수)
지금부터 새로 시작함
망한김에 torch로 되어있는 코드를 베이스로해서 다시 시작할것
torch 폴더에서 작업함

#### 2021.07.02(금)
pytorch에서 basemodel로 삼은 코드를 기준으로,

우리의 호흡음 데이터를 적용시킴.

pytorch는 진짜로 차원맞추는것도 힘들고, loss같은거 뭐 바꿔주면 바꿔줄게 너무많음..

구글링하면서 해결해서 일단 돌리긴했는데,

accuracy가 이상하게 0이 자주나옴 에폭마다..

#### 2021.07.11(일)
07.11_torch_breath_clossentropy.pytnb

accuracy 0.92까지 나왔다. 이번에도 뭔가 잘못됬을까봐 무섭다.

일단 0.92나온 코드 기준으로 공부해보면서 잘못된게 있는지 확인해 볼 예정
그리고 bceloss가 아닌 celoss 썼는데 문제가 될런지.. 공부해야봐야겠다.
bce는 아직 차원문제나와서..ㅎㅎㅎ
