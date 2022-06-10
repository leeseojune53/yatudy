# Ingress가 계속 Progress일 때

### 🎊 시작하기 전에

이 글은 ArgoCD의 dashboard에서 Ingress의 상태가 Progress로 고정되어 있는 것을 바꾸는 방법입니다.

### ♻️ 해결방법

ArgoCD namespace에 있는 Configmap인 `argocd-cm`의 값을 변경해야한다.

여기서 data에 아래와 같이 값을 넣어주면 된다.

```
data:
  resource.customizations: |
    networking.k8s.io/Ingress:
        health.lua: |
          hs = {}
          hs.status = "Healthy"
          return hs
```

