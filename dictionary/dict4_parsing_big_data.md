========= Example can be taken from parse.py ==========

## 🔑 20 Complex Q\&A Examples

### 1. ❓ How to get the first parameter name from the first ParameterGroup?

```python
print(mydata["Metadata"]["AWS::CloudFormation::Interface"]["ParameterGroups"][0].get("Parameters")[0])
```

👉 **Output:** `VPC`

---

### 2. ❓ How to get the label of the second ParameterGroup?

```python
print(mydata["Metadata"]["AWS::CloudFormation::Interface"]["ParameterGroups"][1]["Label"].get("default"))
```

👉 **Output:** `Amazon EC2 Configuration`

---

### 3. ❓ How to get the third parameter name from the Amazon RDS Database Configuration group?

```python
print(mydata["Metadata"]["AWS::CloudFormation::Interface"]["ParameterGroups"][2]["Parameters"][2])
```

👉 **Output:** `MasterUserPassword`

---

### 4. ❓ How to get the description of `PrivateSubnet2ID` parameter?

```python
print(mydata["Parameters"]["PrivateSubnet2ID"].get("Description"))
```

👉 **Output:** `Private subnet 2 in Availability Zone 2.`

---

### 5. ❓ How to get the type of `PublicSubnet1ID`?

```python
print(mydata["Parameters"]["PublicSubnet1ID"].get("Type"))
```

👉 **Output:** `AWS::EC2::Subnet::Id`

---

### 6. ❓ How to get the default AWX Version (deep lookup)?

```python
print(mydata["Parameters"]["AWXVersion"].get("Default"))
```

👉 **Output:** `17.1.0`

---

### 7. ❓ How to get the default DB instance class?

```python
print(mydata["Parameters"]["DBInstanceClass"].get("Default"))
```

👉 **Output:** `db.t3.medium`

---

### 8. ❓ How to get the first allowed DB instance class?

```python
print(mydata["Parameters"]["DBInstanceClass"].get("AllowedValues")[0])
```

👉 **Output:** `db.m1.small`

---

### 9. ❓ How to get the default Preferred Backup Window?

```python
print(mydata["Parameters"]["PreferredBackupWindow"].get("Default"))
```

👉 **Output:** `00:00-02:00`

---

### 10. ❓ How to get the default Preferred Maintenance Window Day?

```python
print(mydata["Parameters"]["PreferredMaintenanceWindowDay"].get("Default"))
```

👉 **Output:** `Mon`

---

### 11. ❓ How to get the default Preferred Maintenance Window Start Time?

```python
print(mydata["Parameters"]["PreferredMaintenanceWindowStartTime"].get("Default"))
```

👉 **Output:** `04:00`

---

### 12. ❓ How to get the default Preferred Maintenance Window End Time?

```python
print(mydata["Parameters"]["PreferredMaintenanceWindowEndTime"].get("Default"))
```

👉 **Output:** `06:00`

---

### 13. ❓ How to get the Condition value for `UsingDefaultBucket` (2nd element in Fn::Equals)?

```python
print(mydata["Conditions"]["UsingDefaultBucket"]["Fn::Equals"][1])
```

👉 **Output:** `aws-quickstart`

---

### 14. ❓ How to get the TemplateURL of `InfrastructureStack` (the string with `${S3Bucket}`)?

```python
print(mydata["Resources"]["InfrastructureStack"]["Properties"]["TemplateURL"]["Fn::Sub"][0])
```

👉 **Output:** `https://${S3Bucket}.s3.${S3Region}.${AWS::URLSuffix}/${QSS3KeyPrefix}templates/awx-infrastructure.template`

---

### 15. ❓ How to get the reference for `InstanceType` parameter inside `InfrastructureStack`?

```python
print(mydata["Resources"]["InfrastructureStack"]["Properties"]["Parameters"]["InstanceType"].get("Ref"))
```

👉 **Output:** `InstanceType`

---

### 16. ❓ How to get the `Fn::Sub` value for ECSSubnets?

```python
print(mydata["Resources"]["InfrastructureStack"]["Properties"]["Parameters"]["ECSSubnets"].get("Fn::Sub"))
```

👉 **Output:** `${PrivateSubnet1ID},${PrivateSubnet2ID}`

---

### 17. ❓ How to get the `Fn::Sub` value for ALBSubnets?

```python
print(mydata["Resources"]["InfrastructureStack"]["Properties"]["Parameters"]["ALBSubnets"].get("Fn::Sub"))
```

👉 **Output:** `${PublicSubnet1ID},${PublicSubnet2ID}`

---

### 18. ❓ How to get the reference used for MasterUsername inside `InfrastructureStack` Parameters?

```python
print(mydata["Resources"]["InfrastructureStack"]["Properties"]["Parameters"]["MasterUsername"].get("Ref"))
```

👉 **Output:** `MasterUsername`

---

### 19. ❓ How to get the `Fn::GetAtt` resource list for ALBDNSName output?

```python
print(mydata["Outputs"]["ALBDNSName"]["Value"].get("Fn::GetAtt"))
```

👉 **Output:** `['InfrastructureStack', 'Outputs', 'ALBDNSName']`

---

### 20. ❓ How to get the last element of the `Fn::GetAtt` list in ALBDNSName output?

```python
print(mydata["Outputs"]["ALBDNSName"]["Value"]["Fn::GetAtt"][-1])
```

👉 **Output:** `ALBDNSName`

---

