# NODEJS DEPLOY

## Before

### 설정 코드
* config file 을 환경별로 만듬 (local, dev, integration, production)
* script 에 환경별로 start 구성 (start:local: "NODE_ENV=local pm2 start ...")

### CI
* build, test

### Deploy
* fabric 으로 remote 서버별 timestamp 폴더에 tar 복사
* cpu 수에 맞춰서 pm2 instance 설정 후 재시작

## After

### 폴더 구조 변경
* app 
    * nginx - docker
    * node - docker
    * mongodb - docker
    * docker-compose-local.yml, docker-compose-dev.yml ...

### 설정코드
* NODE_ENV 및 server 정보를 모두 compose environment 로 구성

```
environment:
    - NODE_ENV=local
    - DBIP=blahblah
```

* 컨테이너 link 와 개별 서버 구성에 대한 고려

    * 개발시 db 는 docker 로 사용하고 node 는 직접 띄우는 등의 조합이 가능함
    * brew 로 local 환경 운영할때 보다 편해짐

```
const DBIP = process.env.DBIP ? process.env.DBIP : 'db_1';
const DBURL = `${DBIP}:${DBPORT}/${DBCOMPONENT}`
```

### CI
* build, test, docker build, docker push

### Deploy

* pm2 intance 1로 고정 - 저렴한 인스턴스를 필요한 수만큼 띄움
