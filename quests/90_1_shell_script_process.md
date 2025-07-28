## **✅ 문제 : 간단한 서버 관리 스크립트 작성**

### **🔧 요구사항**

* `start`: 포트 8000에서 `http.server`를 백그라운드로 실행 (`nohup`, 로그는 `server.log`)

* `stop`: 실행 중인 프로세스를 찾아 종료

* `status`: 프로세스가 실행 중인지 확인하여 출력

* `restart`: 중지 후 다시 실행

  ### **🎯 실행 예시**

  $ ./webserver.sh start  
  서버가 백그라운드에서 시작되었습니다.  
    
  $ ./webserver.sh status  
  서버 실행 중입니다. PID: 13579  
    
  $ ./webserver.sh stop  
  서버가 종료되었습니다.  
    
  $ ./[webserver.sh](http://webserver.sh) tail\_log  
  … log message 확인


문제 모두 조건에 따라:

* `if [ "$1" == "start" ]` 식으로 흐름 제어

* 변수 `PORT`, `PID`, `LOGFILE` 등을 정의해 구성 가능


```shell
# 셸 스크립트

1 #!/bin/bash
  2 
  3 arg=$1
  4 #logfile="./server.log"
  5 
  6 # start: 포트 8000에서 http.server를 백그라운드로 실행 (nohup, 로그는 server.log)
  7 if [ "${arg}" = "start" ]; then
  8 
  9         if [ -f "${logfile}" ]; then
 10                 nohup python3 -m http.server 8000 --bind 0.0.0.0 & #>> "${logfile}"
 11         else
 12                 touch ./server.log
 13                 nohup python3 -m http.server 8000 --bind 0.0.0.0 & #>> "${logfile}"
 14         fi
 15 
 16 # stop: 실행 중인 프로세스를 찾아 종료
 17 elif [ "${arg}" = "stop" ]; then
 18 
 19         pid=$(ps aux | grep "http" | grep -v "grep" | awk '{print $2}')
 20         kill -9 "${pid}"
 21 
 22 # status: 프로세스가 실행 중인지 확인하여 출력
 23 elif [ "${arg}" = "status" ]; then
 24 
 25         pid=$(ps aux | grep "http" | grep -v "grep" | awk '{print $2}')
 26         echo "서버 실행 중입니다. PID: "${pid}""
 27 
 28 # restart: 중지 후 다시 실행
 29 elif [ "${arg}" = "restart" ]; then
 30 
 31         pid=$(ps aux | grep "http" | grep -v "grep" | awk '{print $2}')
 32 
 33         if [ "${pid}" = "" ]; then
 34                 echo "No server running. Starting a new server..."
 35                 nohup python3 -m http.server 8000 --bind 0.0.0.0 & #>> "${logfile}"
 36         else
 37                 kill -9 "${pid}"
 38                 nohup python3 -m http.server 8000 --bind 0.0.0.0 & #>> "${logfile}"
 39         fi
 40 
41 elif [ "${arg}" = "tail_log" ]; then
 42 
 43         #if [ -f "${logfile}" ]; then
 44         if [ -f "./nohup.out" ]; then
 45                 cat "./nohup.out"
 46         else
 47                 echo "No log available"
 48         fi
 49 
 50 else
```