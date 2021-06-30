# breathsound-use-stethoscope
Breath sound(binary classification)

### History

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
