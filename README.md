# stclab_project

## 과제1

- 해당 레포지토리의 소스코드

## 과제2

1. 기본적으로 크게 5개의 directory로 구성.
   이중 실질적으로 기능을 하는 파트는 4개. - api-server : back-end api server - data-layer : DB 연동 모듈 - wave-autoscale : 메인모듈 - web-app : front-end react&next.js - utils : 공통 설정파일에 대한 정의(wave-config.yaml)
2. 각 repository에 대한 기능
   1. wave-autoscale : 메인이 되는 모듈. log설정 및 config의 내용을 기반으로 data-layer, api-server, web-app과 main application을 실행시키고 있다.
      → data-layer가 실행될 때 Arc를 통해 동시성 제어를 하고있다(_shared_data_layer)_. 또한
   2. api-server : back-end api 서버. web-app에서 사용할 API들을 구현.
      컨트롤러 파트만 구현되어있음.
   3. data-layer : 실질적인 비지니스 로직이 구현되어있는 모듈. DB로는 postgres나 sqlite를 사용(default 기준 sqlite)
      → struct에 ts-rs 패키지를 사용하여 web-app에서도 같은 구조를 쓰도록 연동되는것으로 보인다…
   4. web-app : next기반 SSR 웹
      infra/data-layer에 axios.create가 정의되어있음. 해당 인스턴스를 통해 api-server로 Request
3. 구조
   - 모놀리식 구조. wave-autoscale내 web_app_runner에서 노드의 설치여부를 확인한 후 node 커맨드로 실행하고 있다는 점, web-app에서 axios의 baseURL이 없다는 점으로 볼때 결국 wave-autoscale가 같은 서버에서 모든 application을 실행하고 있음이 확실하다.
   - aws 및 azure 등 클라우드 서비스에서 제공해주는 모듈을 이용하여 제어.
4. 의견
   - log파일에서 set_info, set_debug, set_quiet 세개의 함수가 log level에 따라 나눈것이며 log level에 따라 특별히 실행할 기능을 존재하지 않는다면, level값만 인자값으로 받고 하나의 함수에서 실행하는것도 괜찮다고 생각합니다.
   - data_layer.rs파일이 너무 방대해보입니다. 도메인에 따라 모듈화시키는게 좋지않을까 생각합니다.
