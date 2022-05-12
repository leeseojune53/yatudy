# Dynamic Volume Provisioning

### 📌 정의

PVC로 볼륨 요청이 왔을 때 Storage Class에 볼륨(PV)을 생성해주는 것. 즉 온-디맨드 방식으로 스토리지 볼륨(PV)을 생성하는 것이다.

K8S node에 CSI driver를 올리면 PVC 요청이 오면 해당 컨트롤러가 Storage Class에 볼륨 생성 요청을 발행한다.
