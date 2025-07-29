### **🧪 문제 1: 특정 IP 차단 상태 확인 후 차단 설정**

#### **✅ 실행 예시**

$ sudo ./[problem1.sh](http://problem1.sh) )

\[INFO\] 현재 rich rule 목록에 192.168.0.100 차단 룰이 존재하지 않습니다.

\[INFO\] 차단 룰을 추가합니다...

success

또는

$ sudo ./[problem1.sh](http://problem1.sh) 192.168.0.100

\[INFO\] 192.168.0.100은 이미 차단되어 있습니다.

\[SKIP\] 추가 작업을 수행하지 않습니다.



```shell
# 셸 스크립트
  1 #!/bin/bash
  2 
  3 ip=$1
  4 #rich_rule=$(sudo firewall-cmd --list-rich-rules)
  5 
  6 # If IP exists in firewall rich rule
  7 if sudo firewall-cmd --list-rich-rules | grep -q "${ip}"; then
  8         echo "[INFO] ${ip}은 이미 차단되어 있습니다.
  9 [SKIP] 추가 작업을 수행하지 않습니다."
 10 
 11 # If IP does not exist in firewall rich rule
 12 else
 13         sudo firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='${ip}' r    eject"
 14         sudo firewall-cmd --reload
 15         echo "[INFO] 현재 rich rule 목록에 ${ip} 차단 룰이 존재하지 않습니다.
 16 [INFO] 차단 룰을 추가합니다...
 17 success"
 18 fi

```

---

### **🔒 문제 2: 포트 8080이 열려 있다면 닫기**

#### **✅ 실행 예시**

$ sudo ./[problem2.sh](http://problem2.sh) 8080/tcp

\[INFO\] 포트 8080/tcp 이 열려 있습니다. 제거합니다...

success

또는

$ sudo ./[problem2.sh](http://problem2.sh) 8080/tcp

\[INFO\] 포트 8080/tcp 이 열려 있지 않습니다. 아무 작업도 수행하지 않습니다.

---

