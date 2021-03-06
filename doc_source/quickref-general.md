# General Template Snippets<a name="quickref-general"></a>

The following examples show different AWS CloudFormation template features that aren't specific to an AWS service\.

**Topics**
+ [Base64 Encoded UserData Property](#scenario-userdata-base64)
+ [Base64 Encoded UserData Property with AccessKey and SecretKey](#scenario-userdata-base64-with-keys)
+ [Parameters Section with One Literal String Parameter](#scenario-one-string-parameter)
+ [Parameters Section with String Parameter with Regular Expression Constraint](#scenario-constraint-string-parameter)
+ [Parameters Section with Number Parameter with MinValue and MaxValue Constraints](#scenario-one-number-min-parameter)
+ [Parameters Section with Number Parameter with AllowedValues Constraint](#scenario-one-number-parameter)
+ [Parameters Section with One Literal CommaDelimitedList Parameter](#scenario-one-list-parameter)
+ [Parameters Section with Parameter Value Based on Pseudo Parameter](#scenario-one-pseudo-parameter)
+ [Mapping Section with Three Mappings](#scenario-mapping-with-four-maps)
+ [Description Based on Literal String](#scenario-description-from-literal-string)
+ [Outputs Section with One Literal String Output](#scenario-output-with-literal-string)
+ [Outputs Section with One Resource Reference and One Pseudo Reference Output](#scenario-output-with-ref-and-pseudo-ref)
+ [Outputs Section with an Output Based on a Function, a Literal String, a Reference, and a Pseudo Parameter](#scenario-output-with-complex-spec)
+ [Template Format Version](#scenario-format-version)
+ [AWS Tag Property](#scenario-format-aws-tag)

## Base64 Encoded UserData Property<a name="scenario-userdata-base64"></a>

This example shows the assembly of a UserData property using the Fn::Base64 and Fn::Join functions\. The references `MyValue` and `MyName` are parameters that must be defined in the Parameters section of the template\. The literal string `Hello World` is just another value this example passes in as part of the `UserData`\.

### JSON<a name="quickref-general-example-1.json"></a>

```
1. "UserData" : {
2.     "Fn::Base64" : {
3.         "Fn::Join" : [ ",", [
4.             { "Ref" : "MyValue" },
5.             { "Ref" : "MyName" },
6.             "Hello World" ] ]
7.     }
8. }
```

### YAML<a name="quickref-general-example-1.yaml"></a>

```
1. UserData:
2.   Fn::Base64: !Sub |
3.      Ref: MyValue
4.      Ref: MyName
5.      Hello World
```

## Base64 Encoded UserData Property with AccessKey and SecretKey<a name="scenario-userdata-base64-with-keys"></a>

This example shows the assembly of a UserData property using the Fn::Base64 and Fn::Join functions\. It includes the `AccessKey` and `SecretKey` information\. The references `AccessKey` and `SecretKey` are parameters that must be defined in the Parameters section of the template\.

### JSON<a name="quickref-general-example-2.json"></a>

```
1. "UserData" : {
2.     "Fn::Base64" : {
3.         "Fn::Join" : [ "", [
4.             "ACCESS_KEY=", { "Ref" : "AccessKey" },
5.             "SECRET_KEY=", { "Ref" : "SecretKey" } ]
6.         ]
7.     }
8. }
```

### YAML<a name="quickref-general-example-2.yaml"></a>

```
1. UserData:
2.   Fn::Base64: !Sub |
3.      ACCESS_KEY=${AccessKey}
4.      SECRET_KEY=${SecretKey}
```

## Parameters Section with One Literal String Parameter<a name="scenario-one-string-parameter"></a>

The following example depicts a valid Parameters section declaration in which a single `String` type parameter is declared\.

### JSON<a name="quickref-general-example-3.json"></a>

```
1. "Parameters" : {
2.     "UserName" : {
3.         "Type" : "String",
4.         "Default" : "nonadmin",
5.         "Description" : "Assume a vanilla user if no command-line spec provided"
6.     }
7. }
```

### YAML<a name="quickref-general-example-3.yaml"></a>

```
1. Parameters:
2.   UserName:
3.     Type: String
4.     Default: nonadmin
5.     Description: Assume a vanilla user if no command-line spec provided
```

## Parameters Section with String Parameter with Regular Expression Constraint<a name="scenario-constraint-string-parameter"></a>

The following example depicts a valid Parameters section declaration in which a single `String` type parameter is declared\. The AdminUserAccount parameter has a default of admin\. The parameter value must have a minimum length of 1, a maximum length of 16, and contains alphabetic characters and numbers but must begin with an alphabetic character\.

### JSON<a name="quickref-general-example-4.json"></a>

```
 1. "Parameters" : {
 2.     "AdminUserAccount": {
 3.       "Default": "admin",
 4.       "NoEcho": "true",
 5.       "Description" : "The admin account user name",
 6.       "Type": "String",
 7.       "MinLength": "1",
 8.       "MaxLength": "16",
 9.       "AllowedPattern" : "[a-zA-Z][a-zA-Z0-9]*"
10.     }
11. }
```

### YAML<a name="quickref-general-example-4.yaml"></a>

```
1. Parameters:
2.   AdminUserAccount:
3.     Default: admin
4.     NoEcho: true
5.     Description: The admin account user name
6.     Type: String
7.     MinLength: 1
8.     MaxLength: 16
9.     AllowedPattern: '[a-zA-Z][a-zA-Z0-9]*'
```

## Parameters Section with Number Parameter with MinValue and MaxValue Constraints<a name="scenario-one-number-min-parameter"></a>

The following example depicts a valid Parameters section declaration in which a single `Number` type parameter is declared\. The WebServerPort parameter has a default of 80 and a minimum value 1 and maximum value 65535\.

### JSON<a name="quickref-general-example-5.json"></a>

```
1. "Parameters" : {
2.     "WebServerPort": {
3.       "Default": "80",
4.       "Description" : "TCP/IP port for the web server",
5.       "Type": "Number",
6.       "MinValue": "1",
7.       "MaxValue": "65535"
8.     }
9. }
```

### YAML<a name="quickref-general-example-5.yaml"></a>

```
1. Parameters:
2.   WebServerPort:
3.     Default: 80
4.     Description: TCP/IP port for the web server
5.     Type: Number
6.     MinValue: 1
7.     MaxValue: 65535
```

## Parameters Section with Number Parameter with AllowedValues Constraint<a name="scenario-one-number-parameter"></a>

The following example depicts a valid Parameters section declaration in which a single `Number` type parameter is declared\. The WebServerPort parameter has a default of 80 and allows only values of 80 and 8888\.

### JSON<a name="quickref-general-example-6.json"></a>

```
1. "Parameters" : {
2.     "WebServerPortLimited": {
3.       "Default": "80",
4.       "Description" : "TCP/IP port for the web server",
5.       "Type": "Number",
6.       "AllowedValues" : ["80", "8888"]
7.     }
8. }
```

### YAML<a name="quickref-general-example-6.yaml"></a>

```
1. Parameters:
2.   WebServerPortLimited:
3.     Default: 80
4.     Description: TCP/IP port for the web server
5.     Type: Number
6.     AllowedValues:
7.     - 80
8.     - 8888
```

## Parameters Section with One Literal CommaDelimitedList Parameter<a name="scenario-one-list-parameter"></a>

The following example depicts a valid Parameters section declaration in which a single `CommaDelimitedList` type parameter is declared\. The NoEcho property is set to `TRUE`, which will mask its value with asterisks \(\*\*\*\*\*\) in the `aws cloudformation describe-stacks` output\.

**Important**  
Rather than embedding sensitive information directly in your AWS CloudFormation templates, we recommend you use dynamic parameters in the stack template to reference sensitive information that is stored and managed outside of CloudFormation, such as in the AWS Systems Manager Parameter Store or AWS Secrets Manager\.  
For more information, see the [Do Not Embed Credentials in Your Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html#creds) best practice\.

### JSON<a name="quickref-general-example-7.json"></a>

```
1. "Parameters" : {
2.     "UserRoles" : {
3.         "Type" : "CommaDelimitedList",
4.         "Default" : "guest,newhire",
5.         "NoEcho" : "TRUE"
6.     }
7. }
```

### YAML<a name="quickref-general-example-7.yaml"></a>

```
1. Parameters:
2.   UserRoles:
3.     Type: CommaDelimitedList
4.     Default: "guest,newhire"
5.     NoEcho: true
```

## Parameters Section with Parameter Value Based on Pseudo Parameter<a name="scenario-one-pseudo-parameter"></a>

The following example shows commands in the EC2 user data that use the pseudo parameters `AWS::StackName` and `AWS::Region`\. For more information about pseudo parameters, see [Pseudo Parameters Reference](pseudo-parameter-reference.md)\.

### JSON<a name="quickref-general-example-10.json"></a>

```
 1.           "UserData"       : { "Fn::Base64" : { "Fn::Join" : ["", [
 2.              "#!/bin/bash -xe\n",
 3.              "yum install -y aws-cfn-bootstrap\n",
 4. 
 5.              "/opt/aws/bin/cfn-init -v ",
 6.              "         --stack ", { "Ref" : "AWS::StackName" },
 7.              "         --resource LaunchConfig ",
 8.              "         --region ", { "Ref" : "AWS::Region" }, "\n",
 9. 
10.              "/opt/aws/bin/cfn-signal -e $? ",
11.              "         --stack ", { "Ref" : "AWS::StackName" },
12.              "         --resource WebServerGroup ",
13.              "         --region ", { "Ref" : "AWS::Region" }, "\n"
14.         ]]}}
15.       }
```

### YAML<a name="quickref-general-example-10.yaml"></a>

```
1. UserData:
2.   Fn::Base64: !Sub |
3.      #!/bin/bash -xe
4.      yum update -y aws-cfn-bootstrap
5.      /opt/aws/bin/cfn-init -v --stack ${AWS::StackName} --resource LaunchConfig --region ${AWS::Region}
6.      /opt/aws/bin/cfn-signal -e $? --stack ${AWS::StackName} --resource WebServerGroup --region ${AWS::Region}
```

## Mapping Section with Three Mappings<a name="scenario-mapping-with-four-maps"></a>

The following example depicts a valid Mapping section declaration that contains three mappings\. The map, when matched with a mapping key of `Stop`, `SlowDown`, or `Go`, provides the RGB values assigned to the corresponding `RGBColor` attribute\.

### JSON<a name="quickref-general-example-11.json"></a>

```
 1. "Mappings" : {
 2.     "LightColor" : {
 3.         "Stop" : {
 4.             "Description" : "red",
 5.             "RGBColor" : "RED 255 GREEN 0 BLUE 0"
 6.         },
 7.         "SlowDown" : {
 8.             "Description" : "yellow",
 9.             "RGBColor" : "RED 255 GREEN 255 BLUE 0"
10.         },
11.         "Go" : {
12.             "Description" : "green",
13.             "RGBColor" : "RED 0 GREEN 128 BLUE 0"
14.         }
15.     }
16. }
```

### YAML<a name="quickref-general-example-11.yaml"></a>

```
 1. Mappings:
 2.   LightColor:
 3.     Stop:
 4.       Description: red
 5.       RGBColor: "RED 255 GREEN 0 BLUE 0"
 6.     SlowDown:
 7.       Description: yellow
 8.       RGBColor: "RED 255 GREEN 255 BLUE 0"
 9.     Go:
10.       Description: green
11.       RGBColor: "RED 0 GREEN 128 BLUE 0"
```

## Description Based on Literal String<a name="scenario-description-from-literal-string"></a>

The following example depicts a valid Description section declaration where the value is based on a literal string\. This snippet can be for templates, parameters, resources, properties, or outputs\.

### JSON<a name="quickref-general-example-8.json"></a>

```
1. "Description" : "Replace this value"
```

### YAML<a name="quickref-general-example-8.yaml"></a>

```
1. Description: "Replace this value"
```

## Outputs Section with One Literal String Output<a name="scenario-output-with-literal-string"></a>

This example shows a output assignment based on a literal string\.

### JSON<a name="quickref-general-example-12.json"></a>

```
1. "Outputs" : {
2.     "MyPhone" : {
3.         "Value" : "Please call 555-5555",
4.         "Description" : "A random message for aws cloudformation describe-stacks"
5.     }
6. }
```

### YAML<a name="quickref-general-example-12.yaml"></a>

```
1. Outputs:
2.   MyPhone:
3.     Value: Please call 555-5555
4.     Description: A random message for aws cloudformation describe-stacks
```

## Outputs Section with One Resource Reference and One Pseudo Reference Output<a name="scenario-output-with-ref-and-pseudo-ref"></a>

This example shows an Outputs section with two output assignments\. One is based on a resource, and the other is based on a pseudo reference\.

### JSON<a name="quickref-general-example-13.json"></a>

```
1. "Outputs" : {
2.    "SNSTopic" : { "Value" : { "Ref" : "MyNotificationTopic" } },
3.    "StackName" : { "Value" : { "Ref" : "AWS::StackName" } }
4. }
```

### YAML<a name="quickref-general-example-13.yaml"></a>

```
1. Outputs:
2.   SNSTopic:
3.     Value: !Ref MyNotificationTopic
4.   StackName:
5.     Value: !Ref AWS::StackName
```

## Outputs Section with an Output Based on a Function, a Literal String, a Reference, and a Pseudo Parameter<a name="scenario-output-with-complex-spec"></a>

This example shows an Outputs section with one output assignment\. The Join function is used to concatenate the value, using a percent sign as the delimiter\.

### JSON<a name="quickref-general-example-14.json"></a>

```
1. "Outputs" : {
2.     "MyOutput" : {
3.         "Value" : { "Fn::Join" :
4.             [ "%", [ "A-string", {"Ref" : "AWS::StackName" } ] ]
5.         }
6.     }
7. }
```

### YAML<a name="quickref-general-example-14.yaml"></a>

```
1. Outputs:
2.   MyOutput:
3.     Value: !Join [ %, [ 'A-string', !Ref 'AWS::StackName' ]]
```

## Template Format Version<a name="scenario-format-version"></a>

The following snippet depicts a valid Template Format Version section declaration\.

### JSON<a name="quickref-general-example-9.json"></a>

```
1. "AWSTemplateFormatVersion" : "2010-09-09"
```

### YAML<a name="quickref-general-example-9.yaml"></a>

```
1. AWSTemplateFormatVersion: '2010-09-09'
```

## AWS Tag Property<a name="scenario-format-aws-tag"></a>

This example shows an AWS Tag property\. You would specify this property within the Properties section of a resource\. When the resource is created, it will be tagged with the tags you declare\.

### JSON<a name="quickref-general-example-15.json"></a>

```
 1. "Tags" : [
 2.       {
 3.         "Key" : "keyname1",
 4.         "Value" : "value1"
 5.       },
 6.       {
 7.         "Key" : "keyname2",
 8.         "Value" : "value2"
 9.       }
10.     ]
```

### YAML<a name="quickref-general-example-15.yaml"></a>

```
1. Tags: 
2.   - 
3.     Key: "keyname1"
4.     Value: "value1"
5.   - 
6.     Key: "keyname2"
7.     Value: "value2"
```
