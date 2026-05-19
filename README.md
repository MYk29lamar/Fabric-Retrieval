# Fabric-Retrieval
Capstone Design Project 

Introduction: 섬유 무역에 종사하시는 아버지의 업무 중 한 부분을 AI 모델을 통해 자동화 

.Buyer가 사람이 옷을 입고 있는 이미지를 보내면 해당 옷에 있는 섬유 패턴과 똑같은 섬유를 데이터 베이스에서 수동으로 찾아서 회신

AI 모델이 섬유 데이터 베이스에 있는 섬유에 대해서 특징을 학습 -> Query로 Buyer의 이미지가 들어오면 Database 검색하여 유사한 이미지 Top-20순으로 Return
(Google Lens 와 유사한 기술)

<Core Model>
SAM: 학습 데이터가 섬유 부분만 있는 것이 아님 위에 Tag, 배경을 포함한다. 
SAM을 통해서 사용자가 새로운 데이터가 들어올 때 마다 전처리 하는 것이 아닌 자동으로 전처리 하여 모델이 섬유에 대해 노이즈 없이 학습을 보장한다. 


I-JEPA: 현재 가지고 있는 데이터 셋은 약 2만장의 섬유 이미지로, 각각 서로 다른 섬유 이미지로 one - shot 상황이다. 
증강이 필요 없고, 모델이 섬유 이미지 2만장에 대해서 완전히 이해하고 있어야 하기에 고수준의 Representation을 학습할 수 있는 I-JEPA를 사용해 이미지의 특징을 학습시킨 후
Retrival이라는 Downstream 상황에 맞게 fine tuning을 진행 

<Method>
.I-JEPA
1. 섬유 이미지에 대해서 I-JEPA ViT - S/14(244x244)를 통해서 scratched pretrained
QR(Questoin Rised): 사람이 입고 있는 옷에서 섬유 부분을 추출하게 되면, 데이터 Base에 있는 원본에 비해서 크기가 줄어든다.
S(Solutoin): 모델이 Crop에 대한 강건성이 존재 해야 한다. 

2. Contrastive Fine - tune: 이미지 한장이 들어오면 I-JEPA에서 masking 대신에 이미지에 대해서 랜덤 증강 뷰 생성한다.
같은 이미지에서 나온 Crop 쌍은 가깝게 다른 이미지 Crop쌍은 멀게 학습한다.

즉 I-JEPA를 통해 섬유 이미지에 대한 전반적인 이해를 Contrastive을 통해서 증강에 대한 강건성을 확보 





