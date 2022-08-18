---
title: Node
---

:::note
This resource is used for interacting with algo node configurations in SENSR.
A node configuration has configs related to algo node setup. (e.g. `username`, `bin` ...)
:::

## GET
### `/[SENSR version]/settings/node/list`
- Get the list of available config IDs and associated data types of an algo node.
- <span style={{color:"blue"}}>Parameters</span>

    - **N/A**

- <span style={{color:"red"}}>Return code</span>

    - **200 OK** 

- [List of the types that SENSR API supports natively](./algorithm#sensr-versionsettingsalgorithmlist)

- Example
```shell
$ curl -X GET 'http://localhost:9080/[SENSR version]/settings/node/list'
** Respond **
[
    {
        "uid": "String",
        "ip": "String",
        "master_ip": "String",
        "port": "String",
        "name": "String",
        "bin": "String",
        "rosbag_path": "String",
        "username": "String",
        "preset": "String",
        "enable_gpu": "Bool"
    }
]
```

<br/>

### `/[SENSR version]/settings/node`
- Get an config table of the desired algo node.
- <span style={{color:"blue"}}>Parameters</span>

    - **node-uri** : ID of algo node.

- <span style={{color:"red"}}>Return code</span>

    - **200 OK** 
    - **400 BAD REQ**

- Example
```shell
$ curl -X GET 'http://localhost:9080/[SENSR version]/settings/node?node-uri=algo_0000'
** Respond **
{
    "uid": "algo_0000",
    "ip": "127.0.0.1",
    "master_ip" : "127.0.0.1",
    "port": "5055",
    "name": "algo",
    "bin": "",
    "rosbag_path": "/home/seoulrobotics/Downloads/data_out.bag",
    "username": "",
    "preset": "outdoor",
    "enable_gpu": false
}
```

<br/>

### `/[SENSR version]/settings/node-transform`
- Get the transform (position and rotation) of the algo node
- <span style={{color:"blue"}}>Parameters</span>

  - **node-uri** : ID of algo node.

- <span style={{color:"red"}}>Return code</span>

  - **200 OK**
  - **400 BAD REQ**

- Example
```shell
$ curl -X GET 'http://localhost:9080/[SENSR version]/settings/node-transform?node-uri=algo_0000'
** Respond **
{
    "base_to_origin": {
        "tx": 0.0,
        "ty": 0.0
    },
    "sensor_to_base": {
        "tz": 0.0,
        "qw": 1.0,
        "qx": 0.0,
        "qy": 0.0,
        "qz": 0.0
    }
}
```

<br/>

### `/[SENSR version]/settings/node-detection-range`
- Get the detection range of the algo node
- <span style={{color:"blue"}}>Parameters</span>

  - **node-uri** : ID of algo node.

- <span style={{color:"red"}}>Return code</span>

  - **200 OK**
  - **400 BAD REQ**

- Example
```shell
$ curl -X GET 'http://localhost:9080/[SENSR version]/settings/node-detection-range?node-uri=algo_0000'
** Respond **
{
    "x_range": [
        -50.0,
        50.0
    ],
    "y_range": [
        -10.0,
        50.0
    ],
    "z_range": [
        -1.5,
        3.5
    ]
}
```

<br/>
<br/>

## PUT
### `/[SENSR version]/settings/node`
- This command creates a new algo node.
- The command takes in the algo node configuration table as can be seen in the example below. Please refer to the POST command section for details about the format of the data as it is almost identical. The only difference is that you can skip the `uid` here, SENSR will create one for the newly created algo node.
- If this command was successful, the uid of the new algo node is returned.
- You should also call [/commands/apply-change](../commands/#sensr-versioncommandsapply-change) to save your changes.
- <span style={{color:"blue"}}>Parameters</span>

  - **N/A**

- <span style={{color:"red"}}>Return code</span>

  - **200 OK**
  - **400 BAD REQ**

- Example
```shell
$ curl -X PUT 'http://localhost:9080/[SENSR version]/settings/node'
--data '{
    "ip": "192.168.0.100",
    "master_ip": "127.0.0.1",
    "port": "5055",
    "name": "localhost",
    "bin": "",
    "rosbag_path": "",
    "username": "",
    "preset": "outdoor",
    "enable_gpu": false
}'
** Respond **
"algo_0001"
```

<br/>
<br/>

## POST
### `/[SENSR version]/settings/node`
- Update the config of the algo node specified by the `uid`-field in the request body. Note that the combination of IP address and port should be unique.
- You should also call [/commands/apply-change](../commands/#sensr-versioncommandsapply-change) to save your changes.
- <span style={{color:"blue"}}>Parameters</span>

  - **N/A**

- <span style={{color:"red"}}>Return code</span>

    - **200 OK** 
    - **400 BAD REQ**

- Example
```shell
$ curl -X POST 'http://localhost:9080/[SENSR version]/settings/node'
--data '{
    "uid": "algo_0000",
    "ip": "127.0.0.1",
    "master_ip": "127.0.0.1",
    "port": "5055",
    "name": "localhost",
    "bin": "",
    "rosbag_path": "",
    "username": "",
    "preset": "outdoor",
    "enable_gpu": false
}'
** Respond **
200 OK
```

<br/>

### `/[SENSR version]/settings/node-transform`
- Update the transform (position and rotation) of the algo node specified by the `uid`-parameter. 
- The fields "qw", "qx", "qy", and "qz" are the components of the quaternion rotation. The rotation of the algo node is also limited to the z-axis, so "qx" and "qy" should be kept as zero. Additionally the norm of the quaternion should be 1.0 for it to be considered valid.
- Note that for the algo node the height should always be ground-level, so keep "tz" as zero. 
- You should also call [/commands/apply-change](../commands/#sensr-versioncommandsapply-change) to save your changes.
- <span style={{color:"blue"}}>Parameters</span>
  
  - **node-uri** : ID of algo node.

- <span style={{color:"red"}}>Return code</span>

  - **200 OK**
  - **400 BAD REQ**

- Example
```shell
$ curl -X POST 'http://localhost:9080/[SENSR version]/settings/node-transform?node-uri=algo_0000'
--data '{
    "base_to_origin": {
        "tx": 1.0,
        "ty": 2.0
    },
    "sensor_to_base": {
        "tz": 0.0,
        "qw": 1.0,
        "qx": 0.0,
        "qy": 0.0,
        "qz": 0.0
    }
}'
** Respond **
200 OK
```

<br/>

### `/[SENSR version]/settings/node-detection-range`
- Update the detection range of the algo node specified by the `uid`-parameter.
- You should also call [/commands/apply-change](../commands/#sensr-versioncommandsapply-change) to save your changes.
- <span style={{color:"blue"}}>Parameters</span>

  - **node-uri** : ID of algo node.

- <span style={{color:"red"}}>Return code</span>

  - **200 OK**
  - **400 BAD REQ**

- Example
```shell
$ curl -X POST 'http://localhost:9080/[SENSR version]/settings/node-detection-range?node-uri=algo_0000' 
--data '{
    "x_range": [
        -20.0,
        10.0
    ],
    "y_range": [
        -10.0,
        50.0
    ],
    "z_range": [
        -1.5,
        3.5
    ]
}'
** Respond **
200 OK
```

<br/>
<br/>

## DELETE
### `/[SENSR version]/settings/node`
- Delete an algo node.
- <span style={{color:"blue"}}>Parameters</span>

    - **node-uri** : ID of algo node.

- <span style={{color:"red"}}>Return code</span>

    - **200 OK** 
    - **400 BAD REQ**

- Example
```shell
$ curl -X DELETE 'http://localhost:9080/[SENSR version]/settings/node?node-uri=algo_0001'        
** Respond **
200 OK
```
<br/>