# EKS-Private-Cluster

### 폐쇄망에서 EKS 구축 및 kubectl 구성 (AWS 콘솔 사용)

![image](https://user-images.githubusercontent.com/43159901/177040716-66f2f893-d806-4d5a-a7be-84f8b12883d6.png)



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


![image](https://user-images.githubusercontent.com/43159901/177040940-8f1dc318-0ebd-407d-9856-17dba5a582ba.png)

- EKS Cluster 생성 시간 동안 EKS Cluster의 서브넷과 통신할 수 있는 Bastion EC2 생성 후 kubectl 설치(본 과정에서는 생성 과정 생략)
- kubectl 설치 링크 (https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/install-kubectl.html)
- 해당 EC2의 InstanceProfile에는 sts:AssumeRole 필수


![image](https://user-images.githubusercontent.com/43159901/177041052-ee618684-30ff-497f-a57b-145e7fe71d90.png)

- 폐쇄망이기 때문에 kubeconfig 수동 설정 (https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/create-kubeconfig.html#create-kubeconfig-manually)

![image](https://user-images.githubusercontent.com/43159901/177041244-139a8f11-1ea9-4c5d-9ac7-3b4e2f1f5103.png)


![image](https://user-images.githubusercontent.com/43159901/177041370-9133f758-03f1-4efe-95ca-95c281f78c0c.png)
![image](https://user-images.githubusercontent.com/43159901/177041328-16feb429-5f66-48dd-958f-b3eecd9ec36b.png)


- 생성한 EKS Cluster를 참고하여 Cluster Name, API Endpoint, CA Data 참고 후 kubeconfig 설정




- EKS Cluster의 Securit Gruops 확인

![image](https://user-images.githubusercontent.com/43159901/177041094-e00e7438-fec8-46ad-8371-3ec1b3f3daef.png)
![image](https://user-images.githubusercontent.com/43159901/177041141-866d8169-2df0-4b88-8685-ccf6256c237f.png)
- 443 인바운드 추가 (VPC 내부 통신)



![image](https://user-images.githubusercontent.com/43159901/177041488-8de2b2ae-6def-4eb8-8989-b7659fc74be9.png)

- 현재 EC2의 IAM Role이 Cluster에 대해 권한이 없으므로 확인 불가
- 따라서 AssumeRole을 사용하여 권한 설정 필요


![image](https://user-images.githubusercontent.com/43159901/177040021-897ded69-0259-474e-b722-eceaf667bdae.png)
![image](https://user-images.githubusercontent.com/43159901/177042279-baef0d40-2e2d-4a22-ae4a-906389ec9dc0.png)



- 해당 IAM Role의 ARN 복사 후 aws sts 커맨드 실행
- 1시간동안 유효한 Access Key, Secret Key, Session Token 발행 
- 해당 키 값들을 사용하면 EKS에 접근가능

![image](https://user-images.githubusercontent.com/43159901/177041895-7a2ce12b-8e2f-4b70-b1bd-3e786f2665a3.png)

- 권한 설정을 위해 클러스터에 aws-auth configmap 적용 (https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/add-user-role.html#aws-auth-configmap)

![image](https://user-images.githubusercontent.com/43159901/177042100-efaf69cb-cc6e-43fb-b6d1-5b0c75f8973a.png)
![image](https://user-images.githubusercontent.com/43159901/177042159-9cd5e5dc-3d7b-44ec-87f4-e79d3ebf008f.png)

- configmap을 통한 EKS 권한 지정


![image](https://user-images.githubusercontent.com/43159901/177042287-c99714fb-fdd7-4306-89c0-4606213c6dd5.png)

- 앞서 적용한 Access Key, Secret Key, Session Token 무효화

![image](https://user-images.githubusercontent.com/43159901/177042304-05a51366-6cec-4c76-b66a-670beead7923.png)

- EC2의 기본 InstanceProfile로도 kubectl 명령어 가능 확인

![image](https://user-images.githubusercontent.com/43159901/177042415-94f02d81-e95a-4bf7-b25c-88b0a327282b.png)


