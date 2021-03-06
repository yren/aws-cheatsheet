## cloudformation template
* http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html

### YAML template
* Formation Version (optional)
* Description (optional)
* Metadata (optional)
* Parameters (optional), max 60 parameters in one cf template
* Mappings (optional)
* Conditions (optional)
* Transform (optional)
* Resources (required)
* Outputs (optional)


### YAML template example
```
---
AWSTemplateFormatVersion: "2010-09-09"

Description: >
  Here are some details about the template

Metadata:
  Instances:
    Description: "Information about the instances"
  Databases:
    Description: "Information about the databases"

Parameters:
  InstanceTypeParameter:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - m1.small
      - m1.large
    Description: Enter t2.micro, m1.small, m1.large. Default is t2.micro

Mappings:
  Mapping01:
    Key01:
      Name: Value01
    Key02:
      Name: Value02
    Key03:
      Name: Value03


Conditions:
  Logical ID:
    Intrinsic function

Transform:

Resources:
  Logical ID:
    Type: Resource type
    Properties:
      Set of properties

Outputs:
  Logical ID:
    Description: Information about the value
    Value: Value to return
    Export:
      Name: Value to export

...
```

### Transform example
```
Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      'Fn::Transform':
        - Name: 'AWS::Include'
          Parameters:
            Location: s3://bucket/myBucketName.yaml
        - Name: 'AWS::Include'
          Parameters:
            Location: s3://bucket/myBucketAcl.yaml
```

### Logical ID
* 逻辑 ID 必须为字母或数字 (a-z, A-Z, 0-9), 并且在模板中具有唯一性。使用逻辑 ID 在模板中的其他部分引用资源。例如，如果要将 Amazon Elastic Block Store 卷映射到 EC2 实例，则可以引用逻辑 ID 来将块存储与实例相关联。除逻辑 ID 外，某些资源有物理 ID, 这是资源的实际分配名称。如 EC2 实例 ID, S3 存储桶名称。使用物理 ID 来标识模板外部资源。

### Outputs
* Outputs 部分声明输出值，你可以将这些值导入其他 Stack 中，max 60 Outputs in one cf template

### Intrinsic Function Reference

#### Fn::FindInMap
* returns the value corresponding to keys in a two-level map that is declared in the Mappings section.

```
JSON
{{"Fn::FindInMap": ["mapName, topLevelKey, secondLevelKey"]}}

YAML
!FindInMap [mapName, topLevelKey, secondLevelKey]
```

#### Fn::GetAtt
* return value of attribute from resource.

```
JSON
{"Fn::GetAtt": ["resourceLogicalName", "attrName"]}

YAML 
!GetAtt resourceLogicalName.attrName
```

#### Fn::GetAZs
* return an array list of Available Zones for specifed region.

```
JSON
{"Fn::GetAZs": "region"}

YAML 
!GetAZs region
```

#### Fn::ImportValue
* returns the value of an output exported by another stack.

```
JSON
{"Fn::ImportValue": sharedValueToImport}

YAML
!ImportValue: sharedValueToImport
```

#### Fn::Join
* appends a set of values to single value, spearated by specified delimiter.

```
JSON
{"Fn::Join": ["delimiter", [comma-separated list of values]]}

YAML 
!Join [delimiter, [comma-separated list of values]]
```

#### Fn::Select 
* return single object from list by index

```
JSON 
{"Fn::Select": [index, listOfObjects]}

YAML 
!Select [index, listOfObjects]
```

#### Fn::Sub
* substitute string, Fn::Sub 
```
JSON 
{"Fn::Sub" : [String, { var1Name: var1Value, var2Name: var2Value}]}

YAML 
!Sub 
  - String 
  - { var1Name: var1Value, var2Name: var2Value }
``` 

#### Ref 
* return value of specified parameter or Resource
```
JSON

{"Ref": "logicalName"}

YAML 
!Ref logicalName
```

### Transform
* Transform is macro system, it's specify one or more transforms that CloudFormation use to process your template. It reuse cf template component.

* CloudFormation support `AWS::Serverless` and `AWS::Include`
