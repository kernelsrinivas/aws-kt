# CloudFormation Template Structure

## 1. Format Version (Optional)
- **Purpose**: Identifies the version of the CloudFormation template format.
- **Usage**: Helps determine the template's capabilities and compatibility with CloudFormation services.
- **Example**:
  ```yaml
  AWSTemplateFormatVersion: '2010-09-09'
  ```

## 2. Description (Optional)
- **Purpose**: Provides a textual description of the template.
- **Usage**: Helps users understand the purpose and functionality of the template at a glance.
- **Example**:
  ```yaml
  Description: 'My CloudFormation Template for setting up an EC2 instance'
  ```

## 3. Metadata (Optional)
- **Purpose**: Includes additional information about the template.
- **Usage**: Can be used for documentation or tooling purposes, but does not affect resource creation.
- **Example**:
  ```yaml
  Metadata:
    'AWS::CloudFormation::Interface':
      ParameterGroups:
        - Label:
            default: 'EC2 Instance Settings'
          Parameters:
            - InstanceType
            - KeyName
  ```

## 4. Parameters (Optional)
- **Purpose**: Defines values to pass to your template at runtime.
- **Usage**: Allows for customization of the template’s resources without modifying the template itself.
- **Example**:
  ```yaml
  Parameters:
    InstanceType:
      Description: 'EC2 instance type'
      Type: String
      Default: 't2.micro'
    KeyName:
      Description: 'Name of an existing EC2 KeyPair to enable SSH access'
      Type: AWS::EC2::KeyPair::KeyName
  ```

## 5. Mappings (Optional)
- **Purpose**: Provides a way to create lookup tables for conditional values.
- **Usage**: Useful for setting different values based on regions or other conditions.
- **Example**:
  ```yaml
  Mappings:
    RegionMap:
      us-east-1:
        'AMI': 'ami-0ff8a91507f77f867'
      us-west-2:
        'AMI': 'ami-0be260e6c022d0d7e'
  ```

## 6. Conditions (Optional)
- **Purpose**: Specifies conditions under which certain resources are created or certain properties are applied.
- **Usage**: Allows for conditional resource creation or configuration based on parameters or mappings.
- **Example**:
  ```yaml
  Conditions:
    CreateProdInstance: !Equals [!Ref EnvironmentType, 'prod']
  ```

## 7. Transform (Optional)
- **Purpose**: Specifies macros to process the template.
- **Usage**: Used to include and process custom logic or transform templates, such as including other templates.
- **Example**:
  ```yaml
  Transform: 'AWS::Include'
  ```

## 8. Resources (Required)
- **Purpose**: Defines the stack’s resources and their configurations.
- **Usage**: Specifies the AWS resources to be created, such as EC2 instances, S3 buckets, or RDS databases.
- **Example**:
  ```yaml
  Resources:
    MyInstance:
      Type: 'AWS::EC2::Instance'
      Properties:
        InstanceType: !Ref InstanceType
        KeyName: !Ref KeyName
        ImageId: !FindInMap [RegionMap, !Ref AWS::Region, AMI]
  ```

## 9. Outputs (Optional)
- **Purpose**: Describes values returned when viewing the stack’s properties.
- **Usage**: Useful for returning information about the resources created by the stack, such as URLs or IDs.
- **Example**:
  ```yaml
  Outputs:
    InstanceId:
      Description: 'The Instance ID'
      Value: !Ref MyInstance
  ```

---

**Summary of Sections**:
- **Format Version**: Version info.
- **Description**: Template description.
- **Metadata**: Additional info.
- **Parameters**: Runtime inputs.
- **Mappings**: Lookup tables.
- **Conditions**: Conditional logic.
- **Transform**: Macros and transformations.
- **Resources**: Core resources and configurations.
- **Outputs**: Returned values.

The **Resources** section is mandatory, defining the essential infrastructure. Other sections are optional, providing additional customization and functionality.
