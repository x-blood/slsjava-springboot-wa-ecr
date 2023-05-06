# Serverless Java17 Spring Boot Web Adapter Container Example

| -                       | -                              |
|:------------------------|--------------------------------|
| Java                    | Java17                         |
| Application Framework   | Spring Boot 3.0.6              |
| Deploy Framework        | AWS SAM CLI 1.82.0             |
| Development environment | Ubuntu Desktop 20.04           |
| » JDK for local build   | Corretto-17.0.7.7.1            |
| » Container build       | Docker Desktop on Linux 4.17.0 |

## Result of K6

```

          /\      |‾‾| /‾‾/   /‾‾/   
     /\  /  \     |  |/  /   /  /    
    /  \/    \    |     (   /   ‾‾\  
   /          \   |  |\  \ |  (‾)  | 
  / __________ \  |__| \__\ \_____/ .io

  execution: local
     script: slsjava_springboot_wa_ecr.js
     output: -

  scenarios: (100.00%) 1 scenario, 200 max VUs, 31s max duration (incl. graceful stop):
           * default: 200 looping VUs for 1s (gracefulStop: 30s)


     data_received..................: 1.2 MB 137 kB/s
     data_sent......................: 137 kB 16 kB/s
     http_req_blocked...............: avg=496.89ms min=199.1ms  med=455.8ms  max=1.13s    p(90)=825.72ms p(95)=930.47ms
     http_req_connecting............: avg=81.25ms  min=58.46ms  med=77.43ms  max=129.54ms p(90)=110.59ms p(95)=112.86ms
     http_req_duration..............: avg=6.21s    min=5.77s    med=6.19s    max=6.74s    p(90)=6.47s    p(95)=6.57s   
       { expected_response:true }...: avg=6.21s    min=5.77s    med=6.19s    max=6.74s    p(90)=6.47s    p(95)=6.57s   
     http_req_failed................: 0.00%  ✓ 0         ✗ 200  
     http_req_receiving.............: avg=130.72µs min=22µs     med=53µs     max=3.08ms   p(90)=97.4µs   p(95)=235.99µs
     http_req_sending...............: avg=91.79µs  min=22µs     med=60µs     max=707µs    p(90)=175.5µs  p(95)=238.24µs
     http_req_tls_handshaking.......: avg=391.03ms min=111.69ms med=357.03ms max=1.04s    p(90)=706.62ms p(95)=811.53ms
     http_req_waiting...............: avg=6.21s    min=5.77s    med=6.19s    max=6.74s    p(90)=6.47s    p(95)=6.57s   
     http_reqs......................: 200    23.503934/s
     iteration_duration.............: avg=7.71s    min=7.37s    med=7.68s    max=8.5s     p(90)=7.99s    p(95)=8.11s   
     iterations.....................: 200    23.503934/s
     vus............................: 25     min=25      max=200
     vus_max........................: 200    min=200     max=200


running (08.5s), 000/200 VUs, 200 complete and 0 interrupted iterations
default ✓ [======================================] 200 VUs  1s
```

**CloudWatch Logs Insights**   
query-string:
```
filter @type="REPORT" and ispresent(@initDuration)
| stats count() as coldStarts, avg(@maxMemoryUsed), avg(@duration), avg(@initDuration), min(@initDuration), max(@initDuration) by bin(10m)
```
---
| coldStarts | avg(@maxMemoryUsed) | avg(@duration) | avg(@initDuration) | min(@initDuration) | max(@initDuration) |
|------------|---------------------|----------------|--------------------|--------------------|--------------------|
| 200        | 201500000           | 8.0594         | 5963.5863          | 5584.57            | 6446.62            |
---

## Getting Started

### Build
```make
make sam-build
```

### Deploy (Initial)
```make
make sam-deploy-guided
```

### Deploy
```
make sam-deploy
```
