**● Principal Pattern Recognition 이란**
--------------------------------------

Log Data의 행들간의 거리값을 기반으로 군집화하여 가장 주요패턴그룹을 인지한다.

  

**● 목적**
--------

샘플링 1000건에서 일반화된 하나의 AD Language를 생성한다.

그런데 샘플 1000건중에 오류 데이터가 있으면 그 특성까지 반영되어 버림에 따라

샘플 1000건 중에 주요 패턴 그룹을 찾아 이를 기반으로 일반화 하여 AD Language의 품질을 높인다.

**● 행들간 거리 값 산출 공식**

Log Data를 수치로 변환하기 위해 아래와 같은 공식으로 행들간의 거리값을 계산하여,

클러스터링 입력파라메타로 활용한다.

**![](/confluence/download/attachments/44161109/image2018-3-23_13-38-11.png?version=1&modificationDate=1521779888000&api=v2 "IOT플랫폼Lab > 11. Principal Pattern Recognition on Unsupervised Learning > image2018-3-23_13-38-11.png")**

**※ 위 결과값은 '0'과 '1'사이의 값을 가지게 되고 '0'에 수렴할수록 행간 유사도 높음**

**● 클러스터링 알고리즘**
----------------

**DBSCAN (Density-Based Spatial Clustering of Applications with Noise)**

  

![](/confluence/download/attachments/44161109/5dad68_abfdb1fe2b0f4f64bfad9e03e1a70b73_mv2.gif?version=1&modificationDate=1525934913000&api=v2 "IOT플랫폼Lab > 11. Principal Pattern Recognition on Unsupervised Learning > 5dad68_abfdb1fe2b0f4f64bfad9e03e1a70b73_mv2.gif")

1.  DBSCAN은 방문하지 않은 임의의 시작 데이터 포인트로 시작합니다. 이 점의 근방은 거리 ε (ε 거리 내에있는 모든 점은 neighborhood point)을 사용하여 추출됩니다.
    
2.  이 근처에 충분한 포인트 수가 있으면 (minPoints에 따라) 클러스터링 프로세스가 시작되고 현재 데이터 포인트가 새 클러스터의 첫번째 포인트가됩니다. 그렇지 않은 경우, 포인트는 노이즈로 레이블됩니다 (나중에이 노이즈 포인트가 클러스터의 일부가 될 수 있음). 두 경우 모두 해당 지점은 "visited"로 표시됩니다.
    
3.  새로운 클러스터의 이 첫번째 점에 대해 ε 거리 인접도 내의 점도 동일한 클러스터의 일부가됩니다. 클러스터 그룹에 방금 추가 된 모든 새 점에 대해 동일한 클러스터에 속한 ε 근방에있는 모든 점을 만드는 이 절차가 반복됩니다.
    
4.  이 2 단계와 3 단계 프로세스는 클러스터의 모든 점이 결정될 때까지 반복됩니다. 즉 클러스터의 ε 근처에있는 모든 점을 방문하여 레이블을 지정합니다.
    
5.  현재 클러스터를 완료하고 나면 새로운 미확인 지점이 검색되고 처리되어 추가 클러스터 또는 노이즈가 발견됩니다. 이 프로세스는 모든 포인트가 방문으로 표시 될 때까지 반복됩니다. 이 지점의 모든 지점을 방문 했으므로 각 지점은 클러스터에 속하거나 노이즈로 표시됩니다.

  

DBSCAN은 다른 클러스터링 알고리즘에 비해 큰 장점이 있습니다. 첫째로, 그것은 클러스터 집합을 전혀 필요로하지 않습니다. 또한 데이터 포인트가 매우 다르더라도 이상 값을 단순히 클러스터에 버리는 K-Means와는 달리 이상 치를 노이즈로 식별합니다. 또한 임의로 크기가 정해지고 임의로 모양이 지정된 클러스터를 매우 잘 찾을 수 있습니다.

DBSCAN의 가장 큰 단점은 클러스터의 밀도가 다양 할 때 다른 클러스터와 마찬가지로 잘 수행되지 않는다는 것입니다. 이는 밀도가 변할 때 neighborhood point를 식별하기위한 거리 임계 값 ε 및 minPoints의 설정이 클러스터마다 다양하기 때문입니다. 이 단점은 거리 임계치 ε가 다시 추정하기가 어려워지기 때문에 매우 고차원적인 데이터에서도 발생합니다.

**● 처리흐름**
----------

![](/confluence/download/attachments/44161109/image2018-5-10_15-40-8.png?version=1&modificationDate=1525934625000&api=v2 "IOT플랫폼Lab > 11. Principal Pattern Recognition on Unsupervised Learning > image2018-5-10_15-40-8.png")
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**● 예시**
--------

*   **목적 : 입력파라메타 distance 값을 조정하면서 R3, R4를 주요패턴그룹으로 포함하고 싶다**
    
    **R2 : 16열 데이터 누락으로 데이터가 앞으로 당겨짐**
    
    **R3 : 9,10열 이상데이터 포함**
    
    **R4 : 뒤에 garbage 데이터 포함  
    **
    
*   **데이터**  
    **![](/confluence/download/attachments/44161109/image2018-3-23_13-39-41.png?version=1&modificationDate=1521779977000&api=v2 "IOT플랫폼Lab > 11. Principal Pattern Recognition on Unsupervised Learning > image2018-3-23_13-39-41.png")  
    **  
    
*   **행들간의 매트릭스**

**![](/confluence/download/attachments/44161109/image2018-3-23_17-7-41.png?version=1&modificationDate=1521792459000&api=v2 "IOT플랫폼Lab > 11. Principal Pattern Recognition on Unsupervised Learning > image2018-3-23_17-7-41.png")  
**

*   **distance 0~10 변화에 따른 클러스터 값**  
    
    ![](/confluence/download/attachments/44161109/image2018-5-10_16-35-36.png?version=1&modificationDate=1525937735000&api=v2 "IOT플랫폼Lab > 11. Principal Pattern Recognition on Unsupervised Learning > image2018-5-10_16-35-36.png")
    

**※ distance 값이 5일때 R3, R4가 하나의 그룹으로 되었다가, distance 값이 6일때 주요패턴그룹으로 포함된다.**
