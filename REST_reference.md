## API Reference
symbol 规则： 基础币种+计价币种。如BTC/USDT，symbol为btcusdt；ETH/BTC， symbol为ethbtc。以此类推  

返回格式统一为（以下接口只描述data里的数据）：
	
参数名称 | 类型 | 描述 
---- | ---- | ----
code | integer | 状态码 [详见状态码列表](REST_code.md)
message | string | 请求描述
data | array | 数据

例如：

	{
	    "code": 200,
	    "message": "OK",
	    "data": []
	}

### 公共API
#### 测试连通性
	GET /site/ping
描述：  
请求参数：无    
响应数据：  
响应例子：  

	{
    	"code": 200,
    	"message": "OK",
    	"data": []
	}

#### 获取服务器时间戳
	GET /site/timestamp
描述：  
请求参数：无    
响应数据：    

参数名 | 类型 | 描述
---- | ---- | ----
serverTime | integer | 服务器时间戳（毫秒）

响应例子：  

	{
	    "code": 200,
	    "message": "OK",
	    "data": {
	        "serverTime": 1521534792354
	    }
	}

#### 获取交易所规则信息
	GET /site/exchange-info
描述：  
请求参数：无    
响应数据：    

参数名 | 类型 | 描述
---- | ---- | ----
zone | string | 时区 （固定UTC）
serverTime | integer | 服务器时间戳（毫秒）
rateLimits | array | 频率限制
symbols | array | 市场信息

响应例子：  

	{
	    "code": 200,
	    "message": "OK",
	    "data": {
	        "timezone": "UTC",
	        "serverTime": 1521534995824,
	        "rateLimits": [],
	        "symbols": {
	            "ethbtc": {
	                "symbol": "ethbtc",
	                "name": "ETH/BTC",
	                "status": "online",
	                "base_coin": "ETH",
	                "base_precision": "9",
	                "quote_coin": "BTC",
	                "quote_precision": "8"
	            },
	            ...
	        }
	    }
	}

#### 获取BitPlus支持的所有币种
	GET /v1/marke/coins
描述：  
请求参数：无    
响应数据：    

	coin list  
响应例子：  

	{
	    "code": 200,
	    "message": "OK",
	    "data": [
	        "BPB",
	        "BTC",
	        "ETH",
	        "USDT"
	        ..
	    ]
	}


#### 获取BitPlus支持所有交易对
	GET /v1/marke/symbols
描述：  
请求参数：无    
响应数据：    
symbol list:    
 
	参数名 | 类型 | 描述
---- | ---- | ----
symbol | string | 市场交易对
name | string | 市场名称
status | string | 市场开放状态: nonsupport=不支持; auditing=审核中; online=已上线; offline=已下线
base_coin | string | 基础币种
base_precision | string | 数量人精度位数
quote_coin | string | 计价币种
quote_precision | string | 价格精度位数

响应例子：  
	
	{
	    "code": 200,
	    "message": "OK",
	    "data": {
	        "ethbtc": {
	            "symbol": "ethbtc",
	            "name": "ETH/BTC",
	            "status": "online",
	            "base_coin": "ETH",
	            "base_precision": "9",
	            "quote_coin": "BTC",
	            "quote_precision": "8"
	        },
			...
	    }
	}

#### 获取市场depth数据
	GET /v1/marke/depth
描述：获取市场depth数据  
请求参数：

	参数名 | 是否必须 | 类型 | 描述
---- | ---- | ---- | ----
symbol | 是 | string | 市场交易对
limit | 否 | integer | default:20; limit: min-1,max-500

响应数据：    
 
	参数名 | 类型 | 描述
---- | ---- | ----
sell | array | 卖盘，格式：[price(价格），volume(数量)]，按价格price升序
buy | array | 买盘，格式：[price(价格），volume(数量)]，按价格price降序

响应例子：  

	{
	    "code": 200,
	    "message": "OK",
	    "data": {
	        "sell": [
	            [
	                0.078, // 价格
	                0.4783 // 数量
	            ],
	         	  ....
	        ],
	        "buy": [
	            [
	                0.07761981,
	                0.003
	            ],
	       	  ....
	        ]
	    }
	}
	

#### 获取成交历史
	GET /v1/marke/trades
描述：  
请求参数：

	参数名 | 是否必须 | 类型 | 描述
---- | ---- | ---- | ----
symbol | 是 | string | 市场交易对
limit | 否 | integer | default:20; limit: min-1,max-500

响应数据：        
 
	参数名 | 类型 | 描述
---- | ---- | ----
id | string | 成交id
price | decimal | 成交价格
volume | decimal | 成交量
type | string | 主动成交方向：sell,buy
matched_at | string | 成交时间

响应例子：  
	
	{
	    "code": 200,
	    "message": "OK",
	    "data": [
	        {
            "id": "207",
            "price": 0.0781,
            "volume": 0.001,
            "type": "buy",
            "matched_at": "2018-03-17 18:15:31"
        	},
			 ...
	    ]
	}
	
	
#### 获取账户余额
	GET /v1/user/balance (HMAC SHA256)
描述：  
请求参数：

	参数名 | 是否必须 | 类型 | 描述
---- | ---- | ---- | ----
timestamp | 是 | integer | 时间戳（毫秒）
appKey | 是 | string | API 访问密钥
signature | 是 | string | 签名
receiveWindow | 否 | integer | default:5000,请求有效时间

响应数据：        
 
	参数名 | 类型 | 描述
---- | ---- | ----
coin | string | 币种
total | decimal | 总余额  
available | decimal | 可用余额
frozen | decimal | 冻结金额
locked | decimal | 锁定金额

响应例子：  
	
	{
	    "code": 200,
	    "message": "OK",
	    "data": [
	       {
            "coin": "BTC",
            "total": 27.65239814,
            "available": 27.64427083,
            "frozen": 0.00812731,
            "locked": 0
        	},
			 ...
	    ]
	}


#### 获取我的成交历史
	GET /v1/user/my-trades (HMAC SHA256)
描述：  
请求参数：

	参数名 | 是否必须 | 类型 | 描述
---- | ---- | ---- | ----
appKey | 是 | string | API 访问密钥
signature | 是 | string | 签名
symbol | 是 | string | 市场交易对
timestamp | 是 | integer | 时间戳（毫秒）
limit | 否 | integer | default:20; limit: min-1,max-500
receiveWindow | 否 | integer | default:5000,请求有效时间

响应数据：        
 
	参数名 | 类型 | 描述
---- | ---- | ----
id | string | 成交id
price | decimal | 成交价格
volume | decimal | 成交量
type | string | 主动成交方向：sell,buy
matched_at | string | 成交时间

响应例子：  
	
	{
	    "code": 200,
	    "message": "OK",
	    "data": [
	        {
	            "id": "207",
	            "price": 0.078,
        		"volume": 0.2222,
	            "type": "buy",
	            "matched_at": "2018-03-17 18:15:31"
        	 },
			 ...
	    ]
	}
				
#### 创建委托单
	POST /v1/order/create (HMAC SHA256)
描述：  
请求参数：

	参数名 | 是否必须 | 类型 | 描述
---- | ---- | ---- | ----
appKey | 是 | string | API 访问密钥
signature | 是 | string | 签名
timestamp | 是 | integer | 时间戳（毫秒）
symbol | 是 | string | 市场交易对
type | 是 | string | 委托类型：buy-买单，sell-卖单
mode | 是 | string | 交易模式：limit-限价交易
price | 是 | decimal | 报价
volume | 是 | decimal | 委托量
receiveWindow | 否 | integer | default:5000,请求有效时间

响应数据：        
 
	参数名 | 类型 | 描述
---- | ---- | ----
order_no | string | 订单号
status | string | 状态:交易状态：awaiting-挂单中,filled-已完成,cancelled-已撤销

响应例子：  
	
	{
	    "code": 200,
	    "message": "OK",
	    "data": {
	      "order_no": "OPEN_API1522041573",
	      "status": "awaiting"
       }
	}
				

#### 撤销委托单
	DELETE /v1/order/cancel (HMAC SHA256)
描述：  撤销委托单也是一个挂单，只是属性是撤销，非实时得等待系统处理  
请求参数：

	参数名 | 是否必须 | 类型 | 描述
---- | ---- | ---- | ----
appKey | 是 | string | API 访问密钥
signature | 是 | string | 签名
timestamp | 是 | integer | 时间戳（毫秒）
order_no | 是 | string | 订单号
receiveWindow | 否 | integer | default:5000,请求有效时间

响应数据：        
 
	参数名 | 类型 | 描述
---- | ---- | ----
order_no | string | 订单号
status | string | 状态:交易状态：awaiting-挂单中,filled-已完成,cancelled-已撤销

响应例子：  
	
	{
	    "code": 200,
	    "message": "OK",
	    "data": {
	    	"order_no": "OPEN_API1522041573",
	        "status": "awaiting"
        }
	}
				

#### 查询委托单
	GET /v1/order/query (HMAC SHA256)
描述：   
请求参数：

	参数名 | 是否必须 | 类型 | 描述
---- | ---- | ---- | ----
appKey | 是 | string | API 访问密钥
signature | 是 | string | 签名
timestamp | 是 | integer | 时间戳（毫秒）
order_no | 是 | string | 订单号
receiveWindow | 否 | integer | default:5000,请求有效时间

响应数据：        
 
	参数名 | 类型 | 描述
---- | ---- | ----
order_no | string | 订单号
symbol | string | 市场交易对
type | 是 | string | 委托类型：buy-买单，sell-卖单
mode | 是 | string | 交易模式：limit-限价交易
price | decimal | 报价
volume | decimal | 委托量
volume_awaiting | decimal | 等待撮合的委托量
volume_matched | decimal | 撮合数量
volume_awaiting | decimal | 等待撮合数量
completed_at | string | 完成时间
status | string | 状态:交易状态：awaiting-挂单中,filled-已完成,cancelled-已撤销

响应例子：  
	
	{
	    "code": 200,
	    "message": "OK",
	    "data": {
	    	 "order_no": "OPEN_API1522041573",
	        "symbol": "ethbtc",
	        "type": "buy",
	        "mode": "limit",
	        "price": 0.01,
	        "volume": 0.01,
	        "volume_awaiting": 0,
	        "volume_matched": 0,
	        "volume_cancelled": 0.01,
	        "completed_at": "2018-03-26 13:24:43",
	        "status": "cancelled"
        }
	}
				


#### 委托单列表
	GET /v1/order/list(HMAC SHA256)
描述：   
请求参数：

	参数名 | 是否必须 | 类型 | 描述
---- | ---- | ---- | ----
appKey | 是 | string | API 访问密钥
signature | 是 | string | 签名
timestamp | 是 | integer | 时间戳（毫秒）
symbol | 是 | string | 市场交易对
page | 否 | integer | 页码，default：1
pageNum | 否 | integer | 页面记录数，default：20
receiveWindow | 否 | integer | default:5000,请求有效时间

响应数据：        
委托单列表
 
	参数名 | 类型 | 描述
---- | ---- | ----
order_no | string | 订单号
symbol | string | 市场交易对
type | 是 | string | 委托类型：buy-买单，sell-卖单
mode | 是 | string | 交易模式：limit-限价交易
price | string | 报价
volume | string | 委托量
volume_awaiting | string | 等待撮合的委托量
volume_matched | string | 撮合数量
volume_awaiting | string | 等待撮合数量
completed_at | string | 完成时间
status | string | 状态:交易状态：awaiting-挂单中,filled-已完成,cancelled-已撤销

响应例子：  
	
	{
	    "code": 200,
	    "message": "OK",
	    "data": [
	    	{
	           "id": "259",
	            "order_no": "OPEN_API1522041573",
	            "symbol": "ethbtc",
	            "type": "buy",
	            "mode": "limit",
	            "price": 0.01,
	            "volume": 0.01,
	            "volume_awaiting": 0,
	            "volume_matched": 0,
	            "volume_cancelled": 0.01,
	            "completed_at": "2018-03-26 13:24:43",
	            "status": "cancelled"
        	},
        	...
      	]
	}
	