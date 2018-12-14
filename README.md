# gquant_blacklist  http://www.gquant.net
Gquant量化 -股票关注名单  http://www.gquant.net/public/blacklist/ 

公共维护的股票名单.仅用于量化策略或者股票投资使用~ 并非针对个股~ 仅供投资者参考~
使用者可以把它看作为一个股票黑名单或者高风险回报名单，主要看自己如何使用。 
该名单概念始于2016年聚宽量化平台 Morningstar的28小市值的策略中的黑名单。 
本公共程序的创建在2018年7月22日，为了纪念某些医药股那些天的连续跳水~

使用说明：

1. 黑名单有时效性，回测的时候最好不使用，模拟交易建议使用。 
2. 请访问http://www.gquant.net/public/blacklist/stock.json进行网络数据抓取或者本地保存后使用。 
3. 数据源默认为json格式，分为两个节点：名单内股票和剔除名单股票，请视实际情况而定。 
同时提供其他常用调用方法，请点击以下链接查看：① 网页表格 ② 聚宽python方式 ③ BigQuant python方式 ④ 原生PHP方式 
4. 数据源会不定期更新，欢迎公共维护（新增或修改名单） 
欢迎交流，联系方式：http://www.gquant.net/public/
最后一句，君子爱财，取之有道。不是什么钱都可以赚的！岂能因声音微小而不呐喊？

聚宽python方式
聚宽用户请访问：https://www.joinquant.com/post/13981 可以直接获取源码！
	import json,time,datetime,requests  #记得加载！！！！
	outlist=[]
    blacklist=[]
    r=requests.get('http://www.gquant.net/public/blacklist/stock.json') #读取gquant的量化名单json
    content=r.text
    if len(content)>5:
      jsonstr = json.loads(content)
      outjson=jsonstr['data']['outlist'] #加载outlist节点
      blackjson=jsonstr['data']['blacklist'] #加载blacklist节点
    
    for stock in outjson:
        code = stock['code']
        outlist.append(normalize_code(code)) #读取code节点转化为聚宽可使用格式

    for stock in blackjson:
        code = stock['code']
        blacklist.append(normalize_code(code)) #读取code节点转化为聚宽可使用格式
      
    
    print blacklist 
    print outlist 

    #########两个名单可以合并使用，也可以单独使用。

		 
BigQuant python方式
请直接在BIGQUANT里的公共函数库中调取使用。

import json,time,datetime,requests  #记得加载！！！！
def viplist():
    outlist=[]
    blacklist=[]
    r=requests.get('http://www.gquant.net/public/blacklist/stock.json') #读取gquant的量化名单json
    content=r.text
    if len(content)>5:
        jsonstr = json.loads(content)
        outjson=jsonstr['data']['outlist'] #加载outlist节点
        blackjson=jsonstr['data']['blacklist'] #加载blacklist节点
    
        for stock in outjson:
            code = stock['code']
            ex=stock['ex'].upper()           
            code=code+'.'+ex+'A'
            outlist.append(code) #读取code节点

        for stock in blackjson:
            code = stock['code']
            ex=stock['ex'].upper()            
            code=code+'.'+ex+'A'
            blacklist.append(code) #读取code节点
      
    
        #print(blacklist) 
        #print(outlist)
        #量化名单公共维护地址：http://www.gquant.net/public/blacklist/ 欢迎参与数据维护
        #########两个名单可以合并使用，也可以单独使用。
    return blacklist,outlist
#用法如下：
instruments=viplist()[0]
D.history_data(instruments, start_date='2005-01-01', end_date=None, fields=['open', 'close'], frequency='daily', groupped_by_instrument=False, price_type='backward_adjusted')		 
		 
原生PHP方式
$url='http://www.gquant.net/public/blacklist/stock.json';
$stocklist=file_get_contents($url);
$stockjson=json_decode($stocklist,true);
if($stockjson['status']=='ok')
{
	$outlist=$stockjson['data']['outlist']; //被剔除黑名单数据
	$blacklist=$stockjson['data']['blacklist'];//黑名单数组
}

$list=array_merge($outlist,$blacklist);//合并数组同时使用。
print_r($outlist);
print_r($blacklist);
print_r($list);
