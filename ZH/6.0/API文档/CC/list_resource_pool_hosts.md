
### 请求地址

/api/c/compapi/v2/cc/list_resource_pool_hosts/



### 请求方法

POST


### 功能描述

查询资源池中的主机

### 请求参数


#### 通用参数

| 字段 | 类型 | 必选 |  描述 |
|-----------|------------|--------|------------|
| bk_app_code  |  string    | 是 | 应用 ID     |
| bk_app_secret|  string    | 是 | 安全密钥(应用 TOKEN)，可以通过 蓝鲸智云开发者中心 -&gt; 点击应用 ID -&gt; 基本信息 获取 |
| bk_token     |  string    | 否 | 当前用户登录态，bk_token 与 bk_username 必须一个有效，bk_token 可以通过 Cookie 获取 |
| bk_username  |  string    | 否 | 当前用户用户名，应用免登录态验证白名单中的应用，用此字段指定当前用户 |

#### 接口参数

| 字段      |  类型      | 必选   |  描述      |
|-----------|------------|--------|------------|
| page       |  dict    | 否     | 查询条件 |
| host_property_filter| object| 否| 主机属性组合查询条件 |
| fields  |  array   | 是     | 主机属性列表，控制返回结果的主机里有哪些字段，能够加速接口请求和减少网络流量传输   |

#### host_property_filter
该参数为主机属性字段过滤规则的组合，用于根据主机属性字段搜索主机。组合支持 AND 和 OR 两种方式，可以嵌套，最多嵌套 2 层。
过滤规则为四元组 `field`, `operator`, `value`

| 名称  | 类型 |必填| 默认值 | 说明 | Description|
| ---  | ---  | --- |---  | --- | ---| 
| field|string|是|无|字段名 ||
| operator|string|是|无|操作符 |可选值 equal,not_equal,in,not_in,less,less_or_equal,greater,greater_or_equal,between,not_between |
| value| - | 否| 无|操作数|不同的 operator 对应不同的 value 格式|

组装规则可参考: <https://github.com/Tencent/bk-cmdb/blob/master/src/common/querybuilder/README.md>



#### page

| 字段      |  类型      | 必选   |  描述      |
|-----------|------------|--------|------------|
| start    |  int    | 是     | 记录开始位置 |
| limit    |  int    | 是     | 每页限制条数,最大 500 |
| sort     |  string | 否     | 排序字段 |



### 请求参数示例

```json
{
    "bk_app_code": "esb_test",
    "bk_app_secret": "xxx",
    "bk_token": "xxx",
    "bk_supplier_account": "0",
    "page": {
        "start": 0,
        "limit": 10,
        "sort": "bk_host_id"
    },
    "fields": [
        "bk_host_id",
        "bk_cloud_id",
        "bk_host_innerip",
        "bk_os_type",
        "bk_mac"
    ],
    "host_property_filter": {
        "condition": "AND",
        "rules": [
        {
            "field": "bk_host_outerip",
            "operator": "begins_with",
            "value": "127.0"
        }, {
            "condition": "OR",
            "rules": [{
                "field": "bk_os_type",
                "operator": "not_in",
                "value": ["3"]
            }, {
                "field": "bk_sla",
                "operator": "equal",
                "value": "1"
            }]
        }]
    }
}
```

### 返回结果示例

```json
{
  "result": true,
  "code": 0,
  "message": "success",
  "data": {
    "count": 1,
    "info": [
      {
        "bk_cloud_id": "0",
        "bk_host_id": 17,
        "bk_host_innerip": "192.168.1.1",
        "bk_mac": "",
        "bk_os_type": "1"
      }
    ]
  }
}
```

### 返回结果参数说明

#### data

| 字段      | 类型      | 描述      |
|-----------|-----------|-----------|
| count     | int       | 记录条数 |
| info      | array     | 主机实际数据 |

#### data.info
| 名称  | 类型  | 说明 |Description|
|---|---|---|---| 
| bk_isp_name| string | 所属运营商 | 0:其它；1:电信；2:联通；3:移动|
| bk_sn | string | 设备 SN | |
| operator | string | 主要维护人 | |
| bk_outer_mac | string | 外网 MAC | |
| bk_state_name | string | 所在国家 |CN:中国，详细值，请参考 CMDB 页面 |
| bk_province_name | string | 所在省份 |  |
| import_from | string | 录入方式 | 1:excel;2:agent;3:api |
| bk_sla | string | SLA 级别 | 1:L1;2:L2;3:L3 |
| bk_service_term | int | 质保年限 | 1-10 |
| bk_os_type | string | 操作系统类型 | 1:Linux;2:Windows;3:AIX |
| bk_os_version | string | 操作系统版本 | |
| bk_os_bit | int | 操作系统位数 | |
| bk_mem | string | 内存容量| |
| bk_mac | string | 内网 MAC 地址 | |
| bk_host_outerip | string | 外网 IP | |
| bk_host_name | string | 主机名称 |  |
| bk_host_innerip | string | 内网 IP | |
| bk_host_id | int | 主机 ID | |
| bk_disk | int | 磁盘容量 | |
| bk_cpu_module | string | CPU 型号 | |
| bk_cpu_mhz | int | CPU 频率 | |
| bk_cpu | int | CPU 逻辑核心数 | 1-1000000 |
| bk_comment | string | 备注 | |
| bk_cloud_id | int | 云区域 | |
| bk_bak_operator | string | 备份维护人 | |
| bk_asset_id | string | 固资编号 | |