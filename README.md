# A Data

[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/adata?color=d)](https://pypi.org/project/adata/)[![Licence](https://img.shields.io/hexpm/l/apa?color=d)](https://gitee.com/inchaos/adata/blob/master/LICENSE)


## 0、介绍
> 专注A股，专注量化，向阳而生；开放、纯净、持续、为Ai(爱)发电。
>
> 专注股票行情数据，为了保证数据的高可用性，采用多数据源融合切换。
>
> 目标：支持个人量化行情的需要；众人拾柴火焰高。

## 一、快速开始

### （1）安装sdk

~~~python
# 首次安装
pip install adata
# 指定镜像源
pip install adata -i http://mirrors.aliyun.com/pypi/simple/

# 升级版本
pip install -U adata
# 指定镜像源
pip install -U adata -i http://mirrors.aliyun.com/pypi/simple/
~~~

**注：国内镜像可能存在同步延迟，可使用官方镜像源，以下是镜像源**

阿里云【推荐】：http://mirrors.aliyun.com/pypi/simple/ 

清华大学：https://pypi.tuna.tsinghua.edu.cn/simple 

官方镜像源：https://pypi.org/simple

### （2）使用示例

#### 1. 获取股票代码

获取所有的股票代码

~~~python
import adata

res_df = adata.stock.info.all_code()
print(res_df)
~~~

示例结果：

~~~python
  stock_code short_name exchange
0        001324       N长青科       SZ
1        301361       众智科技       SZ
2        300514        友讯达       SZ
3        300880       迦南智能       SZ
4        301368       丰立智能       SZ
...         ...        ...      ...
5488     300325        德威退       SZ
5489     300362        天翔退       SZ
5490     300367        网力退       SZ
5491     300372        欣泰退       SZ
5492     300431        暴风退       SZ

[5493 rows x 3 columns]
~~~

#### 2. 获取股票的行情

获取到股票代码后，传入对应的stock_code参数，查询对应股票的行情信息。

```python
import adata

# k_type: k线类型：1.日；2.周；3.月 默认：1 日k
res_df = adata.stock.market.get_market(stock_code='000001', k_type=1, start_date='2021-01-01')
print(res_df)
```

示例结果：

~~~
            trade_time   open  close  ... pre_close stock_code  trade_date
0    2021-01-04 00:00:00  18.69  18.19  ...     18.93     000001  2021-01-04
1    2021-01-05 00:00:00  17.99  17.76  ...     18.19     000001  2021-01-05
2    2021-01-06 00:00:00  17.67  19.15  ...     17.76     000001  2021-01-06
3    2021-01-07 00:00:00  19.11  19.49  ...     19.15     000001  2021-01-07
4    2021-01-08 00:00:00  19.49  19.44  ...     19.49     000001  2021-01-08
..                   ...    ...    ...  ...       ...        ...         ...
571  2023-05-16 00:00:00  12.80  12.62  ...     12.83     000001  2023-05-16
572  2023-05-17 00:00:00  12.58  12.49  ...     12.62     000001  2023-05-17
573  2023-05-18 00:00:00  12.57  12.49  ...     12.49     000001  2023-05-18
574  2023-05-19 00:00:00  12.43  12.34  ...     12.49     000001  2023-05-19
575  2023-05-22 00:00:00  12.31  12.38  ...     12.34     000001  2023-05-22

[576 rows :x 13 columns]
~~~

#### 3. 其它数据使用

请参考下面数据列表和相关字典文档，找到对应的函数并查看对应的函数注释，进行正确使用。

- [字典DIC](./docs/DIC.md) 

## 二、数据列表

整理了最新版本的数据列表和相关使用Api，详细内容和相关使用参数，请参考数据字典文档。

### （1）股票-Stock

#### 	1. 基本信息

| 数据             | API                                  | 说明                          | 备注                                                     |
| ---------------- | ------------------------------------ | ----------------------------- | -------------------------------------------------------- |
| A股代码          | stock.info.all_code()                | 所有A股代码信息               |                                                          |
| **概念**         |                                      |                               |                                                          |
| 概念代码         | stock.info.all_concept_code_ths()    | 所有A股概念代码信息（同花顺） | 来源：同花顺公开数据                                     |
| 概念成分列表     | stock.info.concept_constituent_ths() | 获取同花顺概念指数的成分股    | 注意：返回结果只有股票代码和股票简称，可根据概念名称查询 |
| **指数**         |                                      |                               |                                                          |
| 指数代码         | stock.info.all_index_code()          | 获取所有A股市场的指数代码     | 来源同花顺，可能存在同花顺对代码重新编码的情况           |
| 指数对应的成分股 | stock.info.index_constituent()       | 获取对应指数的成分股列表      |                                                          |
| **其它**         |                                      |                               |                                                          |
| 股票交易日历     | stock.info.trade_calendar()          | 获取股票交易日信息            | 来源：深交所                                             |

#### 	2. 行情信息

| 数据     | API                                           | 说明                                  | 备注                                                         |
| -------- | --------------------------------------------- | ------------------------------------- | ------------------------------------------------------------ |
| 分红信息 | stock.market.get_dividend()                   | 获取单只股票的分红信息                |                                                              |
| 股票行情 | stock.market.get_market()                     | 获取单只股票的行情信息-日、周、月 k线 |                                                              |
|          | stock.market.get_market_min()                 | 获取单个股票的今日分时行情            | 只能获取当天                                                 |
|          | stock.market.list_market_current()            | 获取多个股票最新行情信息              | 实时行情                                                     |
| 概念行情 | stock.market.get_market_concept_ths()         | 获取单个概念的行情信息-日、周、月 k线 | 目前只有同花顺相关概念行情，<br />获取概念行情时，<br />请注意传入参数是指数代码还是概念代码，<br />指数代码8开头，index_code |
|          | stock.market.get_market_concept_min_ths()     | 获取同花顺概念行情-当日分时           | 只能获取当天                                                 |
|          | stock.market.get_market_concept_current_ths() | 获取同花顺当前的概念行情              | 实时行情                                                     |
| 指数行情 | stock.market.get_market_index()               | 获取指数的行情信息-日、周、月 k线     |                                                              |
|          | stock.market.get_market_index_min()           | 获取指数的行情-当日分时               |                                                              |
|          | stock.market.get_market_index_current()       | 获取当前的指数行情                    | 实时行情                                                     |

**注：概念和指数从本质来看是一样的，所以相关的接口和返回结果是一致的，概念是各个厂商自定义的指数，指数是官方或者权威机构定义的，都是一揽子股票的组合。**

### （2）基金-ETF

| 数据           | API                                      | 说明                           | 备注            |
| -------------- | ---------------------------------------- | ------------------------------ | --------------- |
| ETF（场内）    | fund.info.all_etf_exchange_traded_info() | 获取所有A股市场的ETF信息       | 来源：1. 同花顺 |
| 其它数据排期中 | TODO                                     | 若您有相关资源可以一起参与贡献 |                 |

### （3）债券-Bond

| 数据           | API                          | 说明                                | 备注            |
| -------------- | ---------------------------- | ----------------------------------- | --------------- |
| 可转债代码     | bond.info.all_convert_code() | 获取所有A股市场的可转换债券代码信息 | 来源：1. 同花顺 |
| 其它数据排期中 | TODO                         | 若您有相关资源可以一起参与贡献      |                 |

### （4）舆情

| 项目                     | 进度         | 说明                           |
| ------------------------ | ------------ | ------------------------------ |
| 主要指相关新闻和政策消息 | 排期中...... | 若您有相关资源可以一起参与贡献 |

## 三、数据源

| 数据源     | 板块                                                         | 描述             |
| ---------- | ------------------------------------------------------------ | ---------------- |
| 同花顺     | [数据中心](http://data.10jqka.com.cn/)，[行情中心](http://q.10jqka.com.cn/)，[问财](http://www.iwencai.com/unifiedwap/home/index) | 让投资变的更简单 |
| 百度股市通 | [股市通](https://gushitong.baidu.com/)                       | 科技让投资更简单 |
| 东方财富   | [数据中心](https://data.eastmoney.com/center/)，[行情中心](http://quote.eastmoney.com/center/) | 财经门户         |
| 腾讯理财   | [行情中心](https://stockapp.finance.qq.com/mstats/#)         |                  |

***--------------------------------------------------------------感谢各位大厂提供的数据-------------------------------------------------***

## 四、 其它参考

主要记录查阅过的项目和相关平台，并对此项目产生了深远印象，特此鸣谢。

| [akshare](https://gitee.com/mirrors/akshare) | [聚宽量化](https://www.joinquant.com/) | [baostock](http://baostock.com/baostock/index.php/Python%E5%BC%80%E5%8F%91%E8%B5%84%E6%BA%90) | [MyData](http://api.mairui.club/hsdata.html) |      |
| -------------------------------------------- | -------------------------------------- | ------------------------------------------------------------ | -------------------------------------------- | ---- |

## 五、发布计划

| 版本号 | 内容 | 发布日期     | 备注                           |
| ------ | ---- | ------------ | ------------------------------ |
| 0.x.x  | 股票 | 2023-04-05 ~ | 预览版本                       |
| 1.x.x  | 股票 | 2023-10-01   | 中国Ai股                       |
| 2.x.x  | 基金 | 排期中       | 场内可交易基金：ETF            |
| 3.x.x  | 债券 | 排期中       | 场内可交易债券：可转债         |
| 4.x.x  | 舆情 | 排期中       | 挖掘新闻、网络文章和政策事件等 |

## 六、理念

1. 关于AData，我们只关注交易产生的数据。在A股只有交易数据是真实的，对于量化和AI训练，也只需要关心交易相关的行情数据，做到真正的专注。当然，你可能会说财务数据等也非常有用，但财务数据相对滞后，而且可能ZJ，甚至有XL可能，最终对于普通交易者可能就成了接盘侠。财务数据在我们这里，只做股票池筛选作用，不做实时交易指标推荐。

2. 根据多年的数据治理经验，函数和字典在设计上面，符合标准的数据存储，可根据数据字典建表落地到数据库。

**注：**

- 永久免费开源A股数据库，只有交易相关的数据，专注量化交易。
- 送给A股的各位朋友一首歌：[谢天笑-向阳花](./docs/向阳花.md)，愿你我向阳而生。

## 参与贡献

1.  Fork 本仓库
2.  新建 Feat_xxx 分支
3.  提交代码（注意代码风格和本项目一致即可）
4.  新建 Pull Request


## 特别鸣谢

> 对于项目有支持，包括但不仅限：内容贡献，bug提交，思想交流等等，对项目有影响的个人和机构

| Simon |
| ----- |

