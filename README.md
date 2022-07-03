# EKS-Private-Cluster

### 폐쇄망에서 EKS 구축 및 kubectl 구성 (AWS 콘솔 사용)




### Switch-Role을 통한 EKS 생성


#### EKS는 Cluster를 생성한 IAM Role 또는 User가 자동적으로 EKS cluster-admin에 등록됨 (system:masters)
#### 따라서 AWS 콘솔을 사용하여 생성한 EKS의 권한을 갖기 위해서는 Switch-Role을 사용하여 EKS 생성하면 권한 관리가 보다 간편함


#### 사용 IAM

#### EKS 클러스터 생성용 IAM
![image](https://user-images.githubusercontent.com/43159901/177040021-897ded69-0259-474e-b722-eceaf667bdae.png)


![image](https://user-images.githubusercontent.com/43159901/177040077-a41ef089-a8f1-42e8-a29e-568e9e78c221.png)

- Swich-Role을 사용해야하기 때문에 Trust Realtaionship 편집 -> 해당 IAM Role로 전환 할 IAM의 ARN을 넣어준다.
- EKS 생성 후 EC2에서 해당 IAM Role을 사용하여 권한 설정을 해주어야 하기 때문에 EC2도 허용한다.


#### Swtich-Role


![image](https://user-images.githubusercontent.com/43159901/177040206-e0467989-9c82-4376-ab96-99740c3f8e48.png)

- 위 과정이 끝나면 AWS 콘솔에 로그인 후 역할 전환을 선택한다.

![image](https://user-images.githubusercontent.com/43159901/177040272-e52b106e-457d-4b9a-a18d-6b8b5f781709.png)
![image](https://user-images.githubusercontent.com/43159901/177040348-f33e4d12-4447-4f61-9f67-c714d131579e.png)

- 역할 전환이 완료되면 해당 Swith-Role로 EKS를 생성한다.


![image](https://user-images.githubusercontent.com/43159901/177040460-77891553-b068-422b-85e9-e907ce93f9bb.png)


![image](https://user-images.githubusercontent.com/43159901/177040484-0fcefea1-cc85-4972-a16a-5f4c8150dfaf.png)
![image](https://user-images.githubusercontent.com/43159901/177040486-53876cfc-5056-4431-81ed-05dfcbcdc286.png)



