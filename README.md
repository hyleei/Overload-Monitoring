# 🚀 Spring Boot 애플리케이션 성능 테스트 프로젝트

Spring Boot 애플리케이션의 성능을 테스트하고 모니터링하는 과정을 설명합니다. JMeter를 사용한 부하 테스트, Prometheus를 이용한 메트릭 수집, 그리고 Grafana를 통한 시각화를 포함합니다.

## 👨‍👨‍👧‍👧 참여 인원 
| <img src="https://avatars.githubusercontent.com/u/83341978?v=4" width="150" height="150"/> | <img src="https://avatars.githubusercontent.com/u/129728196?v=4" width="150" height="150"/> | <img src="https://avatars.githubusercontent.com/u/104816148?v=4" width="150" height="150"/> |
|:-------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------:|
|                     **박지원** <br/>[@jiione](https://github.com/jiione)                     |                      **최나영**<br/>[@na-rong](https://github.com/na-rong)                      |                     **박현서**<br/>[@hyleei](https://github.com/hyleei)                        |



## 📌 개요

현대의 웹 애플리케이션은 수많은 동시 사용자를 처리하며 안정적인 성능을 유지해야 합니다. 이러한 요구사항을 충족시키기 위해 부하 테스트는 필수적입니다. JMeter, Grafana, Prometheus를 결합한 부하 테스트 방식은 다음과 같은 이유로 중요성이 높아지고 있습니다:

1. **실제 사용 환경 시뮬레이션**: JMeter를 통해 다양한 사용자 시나리오와 부하 패턴을 시뮬레이션할 수 있어, 실제 운영 환경과 유사한 조건에서 애플리케이션의 성능을 평가할 수 있습니다.

2. **종합적인 성능 메트릭 수집**: Prometheus는 애플리케이션과 인프라의 다양한 성능 지표를 실시간으로 수집합니다. 이를 통해 CPU 사용률, 메모리 소비, 응답 시간 등 다각도의 성능 데이터를 확보할 수 있습니다.

3. **직관적인 데이터 시각화**: Grafana는 Prometheus에서 수집한 데이터를 실시간으로 시각화합니다. 이를 통해 성능 트렌드를 쉽게 파악하고, 잠재적인 병목 현상을 신속하게 식별할 수 있습니다.

4. **선제적 문제 대응**: 이러한 종합적인 접근 방식은 실제 사용자가 문제를 경험하기 전에 잠재적인 성능 이슈를 발견하고 해결할 수 있게 해줍니다.

5. **지속적인 성능 최적화**: 정기적인 부하 테스트를 통해 애플리케이션의 성능 변화를 추적하고, 지속적인 개선을 위한 인사이트를 얻을 수 있습니다.

이러한 도구들을 결합한 부하 테스트는 단순한 성능 측정을 넘어, 애플리케이션의 전반적인 건강 상태를 종합적으로 평가하고 개선하는 데 필수적인 과정입니다.


## 🎯 기획

### 프로젝트 목표
- Spring Boot 애플리케이션의 성능 테스트 및 모니터링 환경 구축
- 부하 테스트를 통한 애플리케이션 성능 분석
- 실시간 메트릭 모니터링 시스템 구현

### 필요 도구
- Java JDK 17
- Spring Boot 3.3.4
- Apache JMeter 5.6.3
- Prometheus 2.30.3
- Grafana

## 🏗 설계

### 시스템 아키텍처
1. Spring Boot 애플리케이션: 성능 테스트 대상
2. JMeter: 부하 테스트 도구
3. Prometheus: 메트릭 수집 및 저장
4. Grafana: 메트릭 시각화 및 대시보드

### 테스트 시나리오
- 동시 사용자 수 증가에 따른 응답 시간 측정
- 장시간 부하 테스트를 통한 시스템 안정성 확인
- CPU, 메모리 사용량 모니터링

## 🛠 구성

### Spring Boot 애플리케이션 설정

1. 의존성 추가 (build.gradle):
```gradle
implementation 'org.springframework.boot:spring-boot-starter-actuator'
implementation 'io.micrometer:micrometer-registry-prometheus'
```

2. application.properties 설정:
```properties
management.endpoints.web.exposure.include=prometheus
management.endpoint.prometheus.enabled=true
server.port=8080
```

3. 애플리케이션 실행:
```bash
java -jar SpringApp-0.0.1-SNAPSHOT.jar
```

### JMeter 설정

1. JMeter 설치:
```bash
wget https://downloads.apache.org/jmeter/binaries/apache-jmeter-5.6.3.tgz
tar -xzf apache-jmeter-5.6.3.tgz
```

2. 테스트 계획 (test_plan.jmx) 생성:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<jmeterTestPlan version="1.2" properties="5.0" jmeter="5.5">
  <hashTree>
    <TestPlan guiclass="TestPlanGui" testclass="TestPlan" testname="Spring Boot App Stress Test Plan" enabled="true">
      <stringProp name="TestPlan.comments"></stringProp>
      <boolProp name="TestPlan.functional_mode">false</boolProp>
      <boolProp name="TestPlan.tearDown_on_shutdown">true</boolProp>
      <boolProp name="TestPlan.serialize_threadgroups">false</boolProp>
      <elementProp name="TestPlan.user_defined_variables" elementType="Arguments" guiclass="ArgumentsPanel" testclass="Arguments" testname="User Defined Variables" enabled="true">
        <collectionProp name="Arguments.arguments"/>
      </elementProp>
      <stringProp name="TestPlan.user_define_classpath"></stringProp>
    </TestPlan>
    <hashTree>
      <ThreadGroup guiclass="ThreadGroupGui" testclass="ThreadGroup" testname="Stress Test Thread Group" enabled="true">
        <stringProp name="ThreadGroup.on_sample_error">continue</stringProp>
        <elementProp name="ThreadGroup.main_controller" elementType="LoopController" guiclass="LoopControlPanel" testclass="LoopController" testname="Loop Controller" enabled="true">
          <boolProp name="LoopController.continue_forever">false</boolProp>
          <stringProp name="LoopController.loops">100</stringProp>
        </elementProp>
        <stringProp name="ThreadGroup.num_threads">500</stringProp>
        <stringProp name="ThreadGroup.ramp_time">60</stringProp>
        <boolProp name="ThreadGroup.scheduler">false</boolProp>
        <stringProp name="ThreadGroup.duration"></stringProp>
        <stringProp name="ThreadGroup.delay"></stringProp>
        <boolProp name="ThreadGroup.same_user_on_next_iteration">true</boolProp>
      </ThreadGroup>
      <hashTree>
        <HTTPSamplerProxy guiclass="HttpTestSampleGui" testclass="HTTPSamplerProxy" testname="HTTP Request" enabled="true">
          <elementProp name="HTTPsampler.Arguments" elementType="Arguments" guiclass="HTTPArgumentsPanel" testclass="Arguments" testname="User Defined Variables" enabled="true">
            <collectionProp name="Arguments.arguments"/>
          </elementProp>
          <stringProp name="HTTPSampler.domain">localhost</stringProp>
          <stringProp name="HTTPSampler.port">8080</stringProp>
          <stringProp name="HTTPSampler.protocol">http</stringProp>
          <stringProp name="HTTPSampler.contentEncoding"></stringProp>
          <stringProp name="HTTPSampler.path">/test</stringProp>
          <stringProp name="HTTPSampler.method">GET</stringProp>
          <boolProp name="HTTPSampler.follow_redirects">true</boolProp>
          <boolProp name="HTTPSampler.auto_redirects">false</boolProp>
          <boolProp name="HTTPSampler.use_keepalive">true</boolProp>
          <boolProp name="HTTPSampler.DO_MULTIPART_POST">false</boolProp>
          <stringProp name="HTTPSampler.embedded_url_re"></stringProp>
          <stringProp name="HTTPSampler.connect_timeout"></stringProp>
          <stringProp name="HTTPSampler.response_timeout"></stringProp>
        </HTTPSamplerProxy>
        <hashTree/>
      </hashTree>
    </hashTree>
  </hashTree>
</jmeterTestPlan>
```

### Prometheus 설정

1. Prometheus 설치:
```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.30.3/prometheus-2.30.3.linux-amd64.tar.gz
tar xvfz prometheus-*.tar.gz
```

2. prometheus.yml 설정:
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'spring_boot_app'
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ['localhost:8080']
```

3. Prometheus 실행:
```bash
./prometheus --config.file=prometheus.yml
```

### Grafana 설정

1. Grafana 설치:
```bash
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install grafana
```

2. Grafana 서비스 시작:
```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

3. Grafana 대시보드 설정:
   - Prometheus 데이터 소스 추가
   - 대시보드 생성 (요청 처리량, 응답 시간, CPU 사용량, JVM 메모리 사용량 등)

## 🧪 테스트

### JMeter 과부하 테스트 실행

1. 테스트 계획 수정 (test_plan.jmx):
   - Thread Group 설정:
     - Number of Threads (users): 500
     - Ramp-up period (seconds): 60
     - Loop Count: 100

2. 과부하 테스트 실행:
```bash
jmeter -n -t test_plan.jmx -l results.jtl
```

3. 실시간 모니터링:
   - Grafana 대시보드에서 다음 메트릭 관찰:
     - 요청 처리량 (Throughput):
       ```
       rate(http_server_requests_seconds_count{job="spring_boot_app"}[1m])
       ```
     - CPU 사용량:
       ```
       system_cpu_usage{job="spring_boot_app"}
       ```
     - JVM 메모리 사용량:
       ```
       sum(jvm_memory_used_bytes{job="spring_boot_app"}) by (area)
       ```

## 📊 검증

### JMeter 결과 분석

```bash
jmeter -g results.jtl -o html_report
```
![image](https://github.com/user-attachments/assets/66b43697-5ae9-4e5d-b6c2-ec9f56443b0a)

### Prometheus 데이터 확인
웹 브라우저에서 `http://your_server_ip:9090` 접속

![image](https://github.com/user-attachments/assets/4e0f942d-9f69-478f-8fb0-ac2acab0036c)

### Grafana 대시보드 분석
- 요청 처리량, 응답 시간, 오류율 확인
- CPU, 메모리 사용량 추이 분석
- 병목 지점 식별 및 성능 개선 포인트 도출

![image](https://github.com/user-attachments/assets/543742b1-a784-4fa5-aac8-bfa0ec988246)



## 🔧 트러블슈팅

1. Spring Boot 애플리케이션 포트 문제
   - 문제: 애플리케이션이 8999 포트에서 실행되어 접근 불가
   - 해결: `application.properties`에서 `server.port=8080`으로 설정

2. Prometheus 타겟 접근 문제
   - 문제: Prometheus가 Spring Boot 애플리케이션 메트릭에 접근 불가
   - 해결: Spring Boot 애플리케이션의 Actuator 설정 확인 및 수정

3. Grafana 데이터 소스 연결 문제
   - 문제: Grafana에서 Prometheus 데이터 소스 연결 실패
   - 해결: Prometheus URL 및 접근 설정 확인

4. JMeter 실행 시 XStream 보안 예외
   - 문제: JMeter 실행 시 XStream 관련 보안 예외 발생
   - 해결: JMeter의 `user.properties` 파일에 `jmeter.save.saveservice.allowlist=*` 추가

5. 과도한 부하로 인한 시스템 불안정
   - 문제: 높은 부하 테스트 중 시스템 리소스 고갈
   - 해결: 테스트 계획의 스레드 수와 반복 횟수 조정, 서버 리소스 모니터링 강화

## 🏁 결론

JMeter, Grafana, Prometheus를 활용한 부하 테스트는 현대 웹 애플리케이션 개발에 있어 필수적인 과정임이 입증되었습니다. 이러한 접근 방식의 주요 이점은 다음과 같습니다:

1. **포괄적인 성능 인사이트**: 다양한 부하 상황에서 애플리케이션의 동작을 종합적으로 이해할 수 있게 되었습니다. CPU, 메모리, 응답 시간, 처리량 등 다각도의 성능 지표를 통해 애플리케이션의 강점과 약점을 명확히 파악할 수 있었습니다.

2. **사전 예방적 성능 관리**: 실제 사용자가 문제를 경험하기 전에 잠재적인 성능 이슈를 식별하고 해결할 수 있는 능력을 갖추게 되었습니다. 이는 사용자 경험 향상과 비즈니스 연속성 유지에 크게 기여합니다.

3. **데이터 기반 의사 결정**: 시각화된 성능 데이터를 통해 최적화 우선순위를 결정하고, 리소스 할당에 대한 더 나은 의사 결정을 내릴 수 있게 되었습니다.

4. **지속적인 성능 개선 문화**: 정기적인 부하 테스트와 모니터링을 통해 성능에 대한 지속적인 관심과 개선 노력이 팀 문화로 자리잡게 되었습니다.

5. **확장성 평가**: 애플리케이션의 현재 처리 능력과 향후 필요한 확장 계획을 수립하는 데 필요한 귀중한 데이터를 제공받을 수 있었습니다.

이러한 부하 테스트 방식은 단순한 기술적 과정을 넘어, 고품질의 사용자 경험을 제공하고 비즈니스 목표를 달성하는 데 핵심적인 역할을 합니다. 앞으로도 이러한 도구와 방법론을 지속적으로 발전시키고 활용함으로써, 더욱 안정적이고 효율적인 애플리케이션 운영이 가능할 것으로 기대됩니다.

