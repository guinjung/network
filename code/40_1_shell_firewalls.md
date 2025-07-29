### **🧪 문제 1: 특정 IP 차단 상태 확인 후 차단 설정**

#### **✅ 실행 예시**

$ sudo ./[problem1.sh](http://problem1.sh) 192.168.0.100

\[INFO\] 현재 rich rule 목록에 192.168.0.100 차단 룰이 존재하지 않습니다.

\[INFO\] 차단 룰을 추가합니다...

success

또는

$ sudo ./[problem1.sh](http://problem1.sh) 192.168.0.100

\[INFO\] 192.168.0.100은 이미 차단되어 있습니다.

\[SKIP\] 추가 작업을 수행하지 않습니다.

---
```
[root@localhost guinjung]# cat ./problem1.sh
if firewall-cmd --list-rich-rules | grep "$1"  > /dev/null ; then
        echo "[INFO] $1 은 이미 차단되어 있습니다."
        echo "[SKIP] 추가 작업을 수행하지 않습니다."
else
        echo "[INFO] 현재 rich rule 목록에 $1 차단 룰이 존재하지 않습니다."
        firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='$1' port port='22' protocol='tcp' drop"
        firewall-cmd --reload
        echo "[INFO] $1 차단 룰을 추가합니다..."
fi

[root@localhost guinjung]# ./problem1.sh 192.168.0.59
[INFO] 현재 rich rule 목록에 192.168.0.59 차단 룰이 존재하지 않습니다.
success
success
[INFO] 192.168.0.59 차단 룰을 추가합니다...

[root@localhost guinjung]# ./problem1.sh 192.168.0.59
[INFO] 192.168.0.59 은 이미 차단되어 있습니다.
[SKIP] 추가 작업을 수행하지 않습니다.
```

### **🔒 문제 2: 포트 8080이 열려 있다면 닫기**

#### **✅ 실행 예시**

$ sudo ./[problem2.sh](http://problem2.sh) 8080/tcp

\[INFO\] 포트 8080/tcp 이 열려 있습니다. 제거합니다...

success

또는

$ sudo ./[problem2.sh](http://problem2.sh) 8080/tcp

\[INFO\] 포트 8080/tcp 이 열려 있지 않습니다. 아무 작업도 수행하지 않습니다.

---
```
[root@localhost guinjung]# cat problem2.sh
if firewall-cmd --list-all | grep "^  ports: $1/tcp" ; then
        echo "[INFO] 포트 $1/tcp 이 열려 있습니다. 제거합니다..."
        firewall-cmd --permanent --remove-port="$1/tcp"
        firewall-cmd --reload
else
        echo "[INFO] 포트 $1/tcp 이 열려 있지 않습니다. 아무 작업도 수행하지 않습니다."
fi

[root@localhost guinjung]# ./problem2.sh 8000
  ports: 8000/tcp
[INFO] 포트 8000/tcp 이 열려 있습니다. 제거합니다...
success

[root@localhost guinjung]# ./problem2.sh 8080
[INFO] 포트 8080/tcp 이 열려 있지 않습니다. 아무 작업도 수행하지 않습니다.
```

### **🧩 문제 3: SSH 서비스 제거 후 특정 IP만 허용**

#### **✅ 실행 예시**

$ sudo ./[problem3.sh](http://problem3.sh) 22/?-\> 찾기

\[INFO\] SSH 서비스가 열려 있습니다. 제거합니다...

success

\[INFO\] 192.168.0.10 IP에만 포트 22 허용 규칙을 추가합니다...

success

또는

$ sudo ./[problem3.sh](http://problem3.sh) 22/?-\> 찾기

\[INFO\] SSH 서비스가 이미 제거되어 있습니다.

\[INFO\] 포트 22 허용 규칙만 추가합니다...

success

---

