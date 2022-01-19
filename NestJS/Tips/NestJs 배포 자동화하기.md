# NestJs 배포 자동화하기

### 🎊 시작하기 전에..

이 글에서 node_modules를 포함하는 것은 **nest의 build파일(dist)에서 node_modules에 의존성을 가지고있어서 포함**하고있는 것이다.

추후에 개선된 방법을 찾으면 이 글을 업데이트 할 것이다.

Github Actions를 이용해 배포할 것 이다.

### 1️⃣ 가장 먼저

Dockerfile

```dockerfile
FROM node:16.13.1
COPY ./dist /dist
COPY ./node_modules /node_modules
CMD [ "node", "dist/main" ]
```

### 2️⃣ 두 번째

Github Actions 파일

```yaml
name: Build

on:
  push:
    branches: [ main ]


jobs:
  build:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16.13.x]

    steps:
      - uses: actions/checkout@v2
      
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
            
      - name: Npm install
        run: npm install
    
      - name: Build
        run: DISABLE_ESLINT_PLUGIN=true npm run build
      
      - name: Cache node modules
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.os }}-node-modules-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-modules-
            
      - name: Docker Hub Login
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Publish on Docker Hub
        run: docker build -t {Registery 명}:{태그명} --push .
        # arm64 라면
        # run: docker buildx build --platform linux/arm64 -t {Registery 명}:{태그명} --push .
```

**Github Actions에서 build**하고, 해당 폴더를 Docker image에 넣는 구조이다.