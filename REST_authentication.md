## 签名认证
目前关于apikey申请和修改，请在“账户 - API管理”页面进行相关操作。其中AccessKey为API 访问密钥，SecretKey为用户对请求进行签名的密钥（仅申请时可见）。

**重要提示：这两个密钥与账号安全紧密相关，无论何时都请勿向其它人透露。**

### 签名运算
key | value
---- | ----
appKey | 4724cc3a99804787b86ee156ebf3200f
secretKey | a8f9dde8db734b8c8050f93354edc8ba

以请求订单列表详情为例：  

 	http://dev.websvr.com/v1/order/list？
 	symbol=ethbtc
 	&timestamp=1522032580000
 	&appKey=4724cc3a99804787b86ee156ebf3200f
 	&receiveWindow=5000
 	
1、先去掉参数timestamp、appKey  
 	
 	http://dev.websvr.com/v1/order/list？
 	symbol=ethbtc
 	&receiveWindow=5000
 
2、按照ASCII码的顺序对参数名进行升序排序（注：使用utf-8编码，若是GET请求方式请对参数进行URI编码）
	 	
 	http://dev.websvr.com/v1/order/list？
 	receiveWindow=5000
 	&symbol=ethbtc
 	
3、拼接参数字符串，形如key1=val1key2=val2...

	receiveWindow=5000symbol=ethbtc
	
4、拼接时间戳在首尾

	1522032580000receiveWindow=5000symbol=ethbtc1522032580000
	
5、使用hashmac256算法进行加密算法，并且输入原始二进制数据，签名密钥：secretKey:a8f9dde8db734b8c8050f93354edc8ba

	/��z�S��L����A�(]P�T�Lϧr��^
	
6、进行base64编码，得到签名结果
	
	L72UeoJTp+RMvL/voX+jQa0oXVCwVNFMz6caf3K18F4=
	注：若为GET请求请记得进行URI编码：L72UeoJTp%2BRMvL%2FvoX%2BjQa0oXVCwVNFMz6caf3K18F4%3D
	
7、最终发送服务器的API请求应该为：
	
	http://dev.websvr.com/v1/order/list？
 	receiveWindow=5000
 	&symbol=ethbtc
 	&timestamp=1522032580000
 	&appKey=4724cc3a99804787b86ee156ebf3200f
 	signature=L72UeoJTp%2BRMvL%2FvoX%2BjQa0oXVCwVNFMz6caf3K18F4%3D
	
	
