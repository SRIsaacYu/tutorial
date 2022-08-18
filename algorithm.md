---
title: Algorithm
---

:::note
This resource demonstrates how to interact with algorithm configurations in SENSR.
An algorithm configuration contains parameters for controlling the algorithms of the perception pipeline.
These parameters enables the user to optimize the perception results of SENSR for their specific use-case.
:::

## GET
### `/[SENSR version]/settings/algorithm/list`
- Get the list of supported config-id and config type in /settings/algorithm resource
- <span style={{color:"blue"}}>Parameters</span>

    - **node-type** : (`algo` or `master`) Type of node of the desired config. Most algorithm configs are located under `algo` node. But if you runs multiple algo nodes, the integration of algorithm results are occurrs in the `master` node.
    - **node-uri** : ID of the algo node where the desired config is located (If you use only one algo node it is usually `algo_0000`. and there is one more special algo node called `common` which stores shared parameters between algo nodes).
    - **config-id** : ID of the desired config (optional)

- <span style={{color:"red"}}>Return code</span>

    - **200 OK** 
    - **400 BAD REQ**
    - **404 NOT FOUND**

- Here is a list of the types that SENSR API supports natively:
    - Int
    - Bool
    - Float
    - String
    - Offset Datetime
        - [Offset Datetime](https://github.com/toml-lang/toml/blob/main/toml.md#user-content-offset-date-time)
    - Local Datetime
        - [Local Datetime](https://github.com/toml-lang/toml/blob/main/toml.md#local-date-time)
    - Local Time
        - [Local Time](https://github.com/toml-lang/toml/blob/main/toml.md#local-time)
    - Vector2 (2 Float array)
    - Vector3 (3 Float array)
    - Vector4 (4 Float array)
    - Array[type]
        - An homogeneous array of one single type.
    - Array[Table]
        - Array can have table type too. In this case, returned json format will be array which has one table inside.
    - Table
        - A table that supports either nested tables, arrays or simple types. In this case, returned json format will be table.

- Example
```shell
$ curl -X GET 'http://localhost:9080/[SENSR version]/settings/algorithm/list?node-type=master&node-uri=common'
** Respond **
{
    "output_merger": {
        "result_timeouts": {
            "algo_node_object_result_timeout": "Float",
            "algo_node_point_result_timeout": "Float",
            "algo_node_sensor_status_timeout": "Float"
        },
        "multi_algo_node_merger": {
            "assoc_dist_threshold": "Float"
        }
    }
}
```

<br/>

### `/[SENSR version]/settings/algorithm/meta`
- Get meta information of the desired config.
- <span style={{color:"blue"}}>Parameters</span>

    - **node-type** : same as node-type in [/settings/algorithm/list](#sensr-versionsettingsalgorithmlist)
    - **node-uri** : same as node-uri in [/settings/algorithm/list](#sensr-versionsettingsalgorithmlist)
    - **config-id** : ID of the desired config (optional)

- <span style={{color:"red"}}>Return code</span>

    - **200 OK** 
    - **400 BAD REQ**
    - **404 NOT FOUND**

- Explanation of each field in meta
    - show : Visibility of the config in Preference menu.
    - display_name : Displayed name in preference menu.
    - use_tooltip : Visibility of tooltip in the preference menu.
    - tooltip_msg : Displayed tooltip message
    - tooltip_args : Arguments which will be shown in tooltip.
    - step_size : Step size of the config
    - min : Min. range of the config
    - max : Max. range of the config

- Example
```shell
$ curl -X GET 'http://localhost:9080/[SENSR version]/settings/algorithm/meta?node-type=master&node-uri=common&config-id=output_merger.result_timeouts.algo_node_point_result_timeout'
** Respond **
{
    "datatype": "Float",
    "show": true,
    "display_name": "Algo-Node Point Result Timeout",
    "min": 0.1
}
```

<br/>

### `/[SENSR version]/settings/algorithm`
- Get an actual value of the desired config
- <span style={{color:"blue"}}>Parameters</span>

    - **node-type** : same as node-type in [/settings/algorithm/list](#sensr-versionsettingsalgorithmlist)
    - **node-uri** : same as node-uri in [/settings/algorithm/list](#sensr-versionsettingsalgorithmlist)
    - **config-id** : ID of the desired config (optional)

- <span style={{color:"red"}}>Return code</span>

    - **200 OK** 
    - **400 BAD REQ**
    - **404 NOT FOUND**

- Example
```shell
$ curl -X GET 'http://localhost:9080/[SENSR version]/settings/algorithm?node-type=master&node-uri=common&config-id=output_merger.result_timeouts.algo_node_point_result_timeout'
** Respond **
0.2
```

<br/>
<br/>

## POST
### `/[SENSR version]/settings/algorithm`
- Update the value of the desired config
- <span style={{color:"blue"}}>Parameters</span>

    - **node-type** : same as node-type in [/settings/algorithm/list](#sensr-versionsettingsalgorithmlist)
    - **node-uri** : same as node-uri in [/settings/algorithm/list](#sensr-versionsettingsalgorithmlist)
    - **config-id** : ID of the desired config (optional)

- <span style={{color:"red"}}>Return code</span>

    - **200 OK** 
    - **400 BAD REQ**
    - **404 NOT FOUND**

- Example
```shell
$ curl -X POST 'http://localhost:9080/[SENSR version]/settings/algorithm?node-type=master&node-uri=common&config-id=output_merger.result_timeouts.algo_node_point_result_timeout' --data '0.5'
** Respond **
200 OK
```
<br/>