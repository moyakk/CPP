# libcurl 을 이용한 HTTP POST Request
- C++ 에서 HTTP POST 요청을 사용하기 위해 적용되었습니다.
  - 좀 더 구체적으로는 POST 방식의 REST API 를 사용하기 위해 적용되었습니다.
  - HTTP 외에도 HTTPS, FTP, LDAP 등의 프로토콜을 지원한다고 합니다.
- 본 메뉴얼은 CentOS 7.8.2003 환경을 기준으로 작성되었습니다.



## 라이브러리 설치
```
yum install -y libcurl-devel
```
- 의존 패키지인 curl, libcurl 이 함께 설치됩니다.



## 샘플코드
- 기본적인 샘플 코드는 [여기](https://curl.se/libcurl/c/example.html)서 볼 수 있습니다.



### 단순히 요청을 보내기만 하면 되는 경우
```cpp
#include <iostream>
#include <pthread.h>
#include <curl/curl.h>

static void *thread(void *arg) {
  struct curl_slist headers = curl_slist_append(headers, "Content-Type: application/json") ;
  std::string url = "http://127.0.0.1:9999" ;
  std::string jsonStr = "{\"key\":\"value\"}" ;
  CURL *curl = curl_easy_init() ;
  if (curl) {
    curl_easy_setopt(curl, CURLOPT_URL, url.c_str()) ;
    curl_easy_setopt(curl, CURLOPT_HTTPHEADER, headers) ;
    curl_easy_setopt(curl, CURLOPT_POST, 1) ;
    curl_easy_setopt(curl, CURLOPT_POSTFIELDS, , jsonStr.c_str()) ;
    if (curl_easy_perform(curl) != CURLE_OK) {
      cerr << "ERROR" << endl ;
    } else {
      cout << "OK" << endl ;
    }
    curl_easy_cleanup(curl) ;
  }
  curl_slist_free_all(headers) ;
}

void main() {
  curl_global_init(CURL_GLOBAL_ALL) ;
  int n_threads = 3 ;
  pthread_t tid[n_threads] ;
  for (int i=0; i<n_threads; i++) {
    if (pthread_create(&tid[i], NULL, thread, NULL) != 0) {
      cout << "ERROR " << i << endl ;
    } else {
      cout << "START " << i << endl ;
    }
  }
  for (int i=0; i<n_threads; i++) {
    pthread_join(tid[i], NULL) ;
    cout << "STOP " << i << endl ;
  }
  curl_global_cleanup() ;
}
```



### 요청에 대한 응답을 기다려야 하는 경우
```cpp

// 작성중 ...

```


## Python을 이용한 간단한 HTTP 서버
- CURL 라이브러리를 이용한 클라이언트 코드의 동작 테스트를 위해 사용되었습니다.
- CentOS 7 에 기본으로 설치되어 있는 Python 2.7.5 를 기준으로 작성되었습니다.

### server.py
```
import SimpleHTTPServer
import SocketServer

PORT = 9999

class ServerHandler(SimpleHTTPServer.SimpleHTTPRequestHandler):

  def do_GET(self):
    self.send_response(200)
    self.end_headers()
    self.wfile.write(b'GET OK')

  def do_POST(self):
    cont_len = int(self.headers.getheader('content-length', 0))
    cont = self.rfile.read(cont_len)
    print cont
    self.send_response(200)
    self.end_headers()
    self.wfile.write(b'POST OK')

Handler = ServerHandler
httpd = SocketServer.TCPServer(("", PORT), Handler)
print "Start HTTP server on port", PORT
```

### 실행방법
```
$ python server.py
Start HTTP server on port 9999
```