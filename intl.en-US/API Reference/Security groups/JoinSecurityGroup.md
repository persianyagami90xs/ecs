# JoinSecurityGroup

You can call this operation to add an ECS instance or an elastic network interface \(ENI\) to a security group.

## Description

**Note:** This operation is not recommended. We recommend that you call the [ModifyInstanceAttribute](~~25503~~) operation to add or remove ECS instances to or from a security group, and call the [ModifyNetworkInterfaceAttribute](~~58513~~) operation to add or remove ENIs to or from a security group.

When you call this operation, take note of the following items:

-   Before you add an instance to a security group, the instance must be in the **Stopped** \(Stopped\) or **Running** \(Running\) state.
-   An instance can be added to a maximum of five security groups.
-   You can [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to add an instance to more security groups. An instance can be added to a maximum of 16 security groups.
-   A basic security group can contain a maximum of 2,000 instances. An advanced security group can contain a maximum of 65,536 instances.
-   The security group and the instance must belong to the same region.
-   The security group and the instance must be of the same network type. If the network type is VPC, the security group and the instance must be in the same VPC.
-   An instance and an ENI cannot be added together to a security group. You cannot specify the `InstanceId` and `NetworkInterfaceId` parameters at the same time.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Ecs&api=JoinSecurityGroup&type=RPC&version=2014-05-26)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|JoinSecurityGroup|The operation that you want to perform. Set the value to JoinSecurityGroup. |
|SecurityGroupId|String|Yes|sg-bp67acfmxazb4p\*\*\*\*|The ID of the security group. You can call the [DescribeSecurityGroups](~~25556~~) operation to view available security groups. |
|InstanceId|String|No|i-bp67acfmxazb4p\*\*\*\*|The ID of the instance.

**Note:** If this parameter is specified, the `NetworkInterfaceId` parameter cannot be specified. |
|NetworkInterfaceId|String|No|eni-bp13kd656hxambfe\*\*\*\*|The ID of the ENI.

**Note:** If this parameter is specified, the `InstanceId` parameter cannot be specified. |
|RegionId|String|No|cn-hangzhou|The region ID. You can call the [DescribeRegions](~~25609~~) operation to query the most recent region list.

-   You do not need to specify a region ID when you add an instance to a security group.
-   You must specify a region ID when you add an ENI to a security group. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|473469C7-AA6F-4DC5-B3DB-A3DC0DE3C83E|The ID of the request. |

## Examples

Sample requests

```
https://ecs.aliyuncs.com/?Action=JoinSecurityGroup
&InstanceId=i-bp67acfmxazb4p****
&SecurityGroupId=sg-bp67acfmxazb4p****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<JoinSecurityGroupResponse>
      <RequestId>473469C7-AA6F-4DC5-B3DB-A3DC0DE3C83E</RequestId>
</JoinSecurityGroupResponse>
```

`JSON` format

```
{
    "RequestId": "473469C7-AA6F-4DC5-B3DB-A3DC0DE3C83E"
}
```

## Error codes

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|404|InvalidSecurityGroupId.NotFound|The specified SecurityGroupId does not exist.|The error message returned because the specified security group does not exist in this account. Check whether the security group ID is correct.|
|400|InstanceSecurityGroupLimitExceeded|Exceeding the allowed amount of security groups that an instance can be in.|The error message returned because the maximum number of security groups to which the instance can be added has been reached.|
|403|IncorrectInstanceStatus|The current status of the resource does not support this operation.|The error message returned because the operation is not supported while the resource is in the current state.|
|403|InstanceLockedForSecurity|The specified operation is denied as your instance is locked for security reasons.|The error message returned because the operation is not supported while the instance is locked for security reasons.|
|403|SecurityGroupInstanceLimitExceeded|The maximum number of instances in a security group is exceeded.|The error message returned because the maximum number of instances in the specified security group has been reached.|
|404|InvalidInstanceId.NotFound|The specified InstanceId does not exist.|The error message returned because the specified InstanceId parameter does not exist.|
|400|InvalidInstanceId.Mismatch|Specified instance and security group are not in the same VPC.|The error message returned because the specified instance and security group do not belong to the same VPC or one of the following special cases occurs: 1. The security group is of the VPC network type but the instance is not. 2. The instance is of the VPC network type but the security group is not.|
|403|InvalidInstanceId.AlreadyExists|The specified instance already exists in the specified security group.|The error message returned because the specified instance already exists in the security group.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because an internal error has occurred. Try again later. If the problem persists, submit a ticket.|
|403|SecurityGroupInstanceLimitExceeded|%s|The error message returned because the maximum number of instances in the specified security group has been reached.|
|403|AclLimitExceed|%s|The error message returned because the maximum value of AccessPoint has been reached.|
|403|InstanceSecurityGroupLimitExceeded|%s|The error message returned because the maximum number of security groups to which the instance can be added has been reached.|
|400|InvalidInstanceId.Malformed|The specified parameter "InstanceId" is not valid.|The error message returned because the specified InstanceId parameter is invalid.|
|404|InvalidEniId.NotFound|%s|The error message returned because the specified NetworkInterfaceId parameter does not exist.|
|403|InvalidOperation.InvalidEniType|%s|The error message returned because the ENI type does not support this operation.|
|400|InvalidOperation.InvalidEniState|%s|The error message returned because the operation is not supported while the ENI is in the current state.|
|403|InvalidOperation.VpcMismatch|%s|The error message returned because the operation is invalid. Check whether the VPC in the operation corresponds to other parameters.|
|400|NotBelongUser|%s|The error message returned because you are not authorized to manage the specified resource.|
|400|MissingParameter.RegionId|The specified RegionId should not be null.|The error message returned because the RegionId parameter is not specified.|
|403|InvalidOperation.EniServiceManaged|%s|The error message returned because the operation is invalid.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Ecs).

