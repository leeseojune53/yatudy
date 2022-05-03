# PV(Persistent Volumes)란?

### 📌 정의

관리자가 Provisioning하거나 스토리지 클래스를 사용하여 동적으로 Provisioning한 클러스터의 **스토리지**이다.

물리적인 볼륨

### ⚙️ Spec

- 용량 : capacity 속성에 작성
- 볼륨 모드 : volumeModes 속성에 작성
  - Filesystem(default) : 파일 시스템을 사용할 경우
  - Block : 원시 블록 장치로 사용할 경우
- 접근 모드 : accessModes 속성에 작성
  - ReadWriteOnce : **하나의 노드**에서 읽기 및 쓰기로 마운트 될 수 있다. **동일한 노드라면 여러개의 파드가 접근할 수 있다.**
  - ReadOnlyMany : **다수의 노드**에서 읽기 전용으로 마운트 될 수 있다.
  - ReadWriteMany : **다수의 노드**에서 읽기 및 쓰기로 마운트 될 수 있다.
  - ReadWriteOncePod : **하나의 파드**에서 읽기 및 쓰기로 마운트 될 수 있다. **단 하나의 파드**에서만 접근이 가능하다.
- 클래스 : storageClassName 속성에 작성, 스토리지 클래스 명 사용
- 반환 정책 : persistentVolumeReclaimPolicy 속성에 작성
  - Retain : 수동 반환
  - Recycle(Deprecated) : 스토리지 내부 파일을 전부 삭제
  - Delete : 스토리지 자산 제거