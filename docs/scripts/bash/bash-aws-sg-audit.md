---
title: AWS Security Group Audit
icon: simple/amazonec2
---

# AWS Security Group Audit

A "quick and dirty" method of pulling AWS Security Group details to check for changes at different intervals. The script below generates five (5) files, each suffixed with a timestamp (`yyyy-mm-dd-hhmmss`):

| File | Description | 
| ---: | :---------- |
| `sg-audit` | List of each Security Group and all of it's ingress / egress rules. |
| `sg-listing` | List of all Security Groups. Includes Security Group ID, SG Name, and Description. |
| `ec2-sec-groups` | List of all Security Groups assigned to all EC2 instances. |
| `rds-sec-groups` | List of all Security Groups assigned to all RDS instances. |
| `alb-sec-groups` | List of all Security Groups assigned to all ALBs.|

Assumptions built-in to the script:

- Security Groups have tags to identify them by `Name` and `Environment`.
- CIDR ranges in Security Groups are finite and one of only Class A, B, or C subnets.
- Modify `profile` and `region` accordingly.

``` bash title="security-group-audit.sh" linenums="1" hl_lines="3-4 92-97 160-165"
#!/bin/bash
# Get a full accounting of Security Groups
profile="XXX"
region="XX-XXXX-XX"

now=$(date +"%Y-%m-%d-%H%M%S")
saveFile="sg-audit-$now.txt"
ec2File="ec2-sec-groups-$now.txt"
rdsFile="rds-sec-groups-$now.txt"
albFile="alb-sec-groups-$now.txt"

old_IFS=$IFS; IFS=$'\n'

# Save a listing of all Security Groups to disk
sgListFile="sg-listing-$now.txt"
aws ec2 describe-security-groups --profile $profile --region $region --query 'SecurityGroups[*].{GroupId:GroupId,GroupName:GroupName,Description:Description}' --output text > $sgListFile

sgCount=$(wc -l sg-listing-$now.txt | xargs | awk '{print $1}')
echo -e "$sgCount Security Group(s) identified.\n"
echo -e "Security Group audit file will be saved to $saveFile, tab-delimited.\n"

echo "Heading for Security Group audit data are sectioned:"
echo -e "SecGroupId\tSecGroupName\tVPC\tDescr\tNameTag\tEnvironmentTag"
echo -e "\tDirection\tProtocol\tFromPort\tToPort\tSource\tDescriptions-or-Comments\n"

cat $sgListFile | while read line
do
    securityGroup=(`echo $line | awk -F\t '{print $2}'`)
    sgroup=$(aws ec2 describe-security-groups --profile $profile --region $region --group-ids $securityGroup)

    # Security Group properties
    sgId=$(echo $sgroup | jq -r '.SecurityGroups[].GroupId')
    sgGroupName=$(echo $sgroup | jq -r '.SecurityGroups[].GroupName')
    sgDesc=$(echo $sgroup | jq -r '.SecurityGroups[].Description')
    sgVpc=$(echo $sgroup | jq -r '.SecurityGroups[].VpcId')
    sgTagName=$(echo $sgroup | jq -r '.SecurityGroups[].Tags[] | select(.Key == "Name") | .Value' 2>/dev/null)
    sgTagEnv=$(echo $sgroup | jq -r '.SecurityGroups[].Tags[] | select(.Key == "Environment") | .Value' 2>/dev/null)

    echo -e "$sgId\t$sgGroupName\t$sgVpc\t$sgDesc\t$sgTagName\t$sgTagEnv"
    printf "$sgId\t$sgGroupName\t$sgVpc\t$sgDesc\t$sgTagName\t$sgTagEnv\n" >> $saveFile

    # process INGRESS rules
    for rule in $(echo $sgroup | jq -r '.SecurityGroups[].IpPermissions[] | @base64'); do
        _jq() {
            echo ${rule} | base64 --decode | jq -r ${1}
        }
        ipProtocol=$(_jq '.IpProtocol')
        fromPort=$(_jq '.FromPort')
        toPort=$(_jq '.ToPort')
        ipRanges=$(_jq '.IpRanges[]')
        groupPairs=$(_jq '.UserIdGroupPairs[]')

        if [[ "$ipProtocol" == "-1" ]]; then
            fromPort="ALL"
            toPort="ALL"
            ipProtocol="ALL"
        fi

        # Process Security Groups
        for group in $(echo $groupPairs | jq -r '. | @base64'); do
            _jq2() {
                echo ${group} | base64 --decode | jq -r ${1}
            }
            groupId=$(_jq2 '.GroupId')
            groupDesc=$(_jq2 '.Description')
            groupAcct=$(_jq2 '.UserId')
            groupName=""
            sgDesc=""

            sgDetail=$(grep $groupId $sgListFile)
            
            if [ $? -eq 0 ]; then
                groupName=(`echo $sgDetail | awk -F\t '{print $3}'`)
                sgDesc=(`echo $sgDetail | awk -F\t '{print $1}'`)
            fi

            echo -e "\tINGRESS\t$ipProtocol\t$fromPort\t$toPort\t$groupId\t$groupName\t$groupDesc"
            printf "\tINGRESS\t$ipProtocol\t$fromPort\t$toPort\t$groupId\t$groupName\t$groupDesc\n" >> $saveFile
        done

        # Process CIDR Ranges
        for cidr in $(echo $ipRanges | jq -r '. | @base64'); do
            _jq3() {
                echo ${cidr} | base64 --decode | jq -r ${1}
            }
            comment=""
            cidrIp=$(_jq3 '.CidrIp')
            cidrDesc=$(_jq3 '.Description')

            if [ -z "$cidrDesc" ] || [[ $cidrDesc == "null" ]]; then
                case $cidrIp in
                    "10.0.0.0/8")
                        cidrDesc="Class A Subnet";;
                    "172.16.0.0/12")
                        cidrDesc="Class B Subnet";;
                    "192.168.0.0/16")
                        cidrDesc="Class C Subnet";;
                esac
            fi

            if [[ "$cidrIp" == "0.0.0.0/0" ]] || [[ "$cidrIp" == "::/0" ]]; then
                comment="** GLOBAL **"
            fi

            echo -e "\tINGRESS\t$ipProtocol\t$fromPort\t$toPort\t$cidrIp\t\t\t$cidrDesc\t$comment"
            printf "\tINGRESS\t$ipProtocol\t$fromPort\t$toPort\t$cidrIp\t\t\t$cidrDesc\t$comment\n" >> $saveFile
        done
    done

    # process EGRESS rules
    for rule in $(echo $sgroup | jq -r '.SecurityGroups[].IpPermissionsEgress[] | @base64'); do
        _jq() {
            echo ${rule} | base64 --decode | jq -r ${1}
        }
        ipProtocol=$(_jq '.IpProtocol')
        fromPort=$(_jq '.FromPort')
        toPort=$(_jq '.ToPort')
        ipRanges=$(_jq '.IpRanges[]')
        groupPairs=$(_jq '.UserIdGroupPairs[]')

        if [[ "$ipProtocol" == "-1" ]]; then
            fromPort="ALL"
            toPort="ALL"
            ipProtocol="ALL"
        fi

        # Process Security Groups
        for group in $(echo $groupPairs | jq -r '. | @base64'); do
            _jq2() {
                echo ${group} | base64 --decode | jq -r ${1}
            }
            groupId=$(_jq2 '.GroupId')
            groupDesc=$(_jq2 '.Description')
            groupAcct=$(_jq2 '.UserId')
            groupName=""
            sgDesc=""

            sgDetail=$(grep $groupId $sgListFile)
            
            if [ $? -eq 0 ]; then
                groupName=(`echo $sgDetail | awk -F\t '{print $3}'`)
                sgDesc=(`echo $sgDetail | awk -F\t '{print $1}'`)
            fi

            echo -e "\tEGRESS\t$ipProtocol\t$fromPort\t$toPort\t$groupId\t$groupName\t$groupDesc"
            printf "\tEGRESS\t$ipProtocol\t$fromPort\t$toPort\t$groupId\t$groupName\t$groupDesc\n" >> $saveFile
        done

        # Process CIDR Ranges
        for cidr in $(echo $ipRanges | jq -r '. | @base64'); do
            _jq3() {
                echo ${cidr} | base64 --decode | jq -r ${1}
            }
            comment=""
            cidrIp=$(_jq3 '.CidrIp')
            cidrDesc=$(_jq3 '.Description')

            if [ -z "$cidrDesc" ] || [[ $cidrDesc == "null" ]]; then
                case $cidrIp in
                    "10.0.0.0/8")
                        cidrDesc="Class A Subnet";;
                    "172.16.0.0/12")
                        cidrDesc="Class B Subnet";;
                    "192.168.0.0/16")
                        cidrDesc="Class C Subnet";;
                esac
            fi

            if [[ "$cidrIp" == "0.0.0.0/0" ]] || [[ "$cidrIp" == "::/0" ]]; then
                comment="** GLOBAL **"
            fi

            echo -e "\tEGRESS\t$ipProtocol\t$fromPort\t$toPort\t$cidrIp\t\t\t$cidrDesc\t$comment"
            printf "\tEGRESS\t$ipProtocol\t$fromPort\t$toPort\t$cidrIp\t\t\t$cidrDesc\t$comment\n" >> $saveFile
        done
    done
done
echo " "
echo " ----- "
echo " "

# Collect info about EC2, RDS, ALB -- these resources use the Security Groups identified above
ec2Instances=$(aws ec2 describe-instances --profile $profile --region $region --query 'Reservations[*].Instances[*].{InstanceId:InstanceId,Name:Tags[?Key==`Name`]|[0].Value,VPC:VpcId,Subnet:SubnetId,SecurityGroups:SecurityGroups[*]}')
rdsInstances=$(aws rds describe-db-instances --profile $profile --region $region --query 'DBInstances[*].{DBInstance:DBInstanceIdentifier,SubnetGroup:DBSubnetGroup.DBSubnetGroupName,SubnetDesc:DBSubnetGroup.DBSubnetGroupDescription,VPC:DBSubnetGroup.VpcId,MultiAZ:MultiAZ,Encrypted:StorageEncrypted,DeleteProtection:DeletionProtection,SecurityGroups:VpcSecurityGroups[*]}')
albInstances=$(aws elbv2 describe-load-balancers --profile $profile --region $region --query 'LoadBalancers[*].{LBName:LoadBalancerName,Direction:Scheme,VPC:VpcId,Type:Type,SecurityGroups:SecurityGroups[*]}')

# Parse EC2 Instance Associations
echo -e "EC2 Security Group associations file will be saved to $ec2File, tab-delimited.\n"
echo -e "InstanceId\tInstanceName\tVPC\tSubnet\tSecGroupId\tSecGroupName"
printf "InstanceId\tInstanceName\tVPC\tSubnet\tSecGroupId\tSecGroupName\n" >> $ec2File
for ec2 in $(echo $ec2Instances | jq -r '.[] | @base64'); do
    ec2=$(echo "$ec2" | base64 --decode | jq -r .[])
    _jq() {
        echo ${ec2} | jq -r ${1}
    }

    instId=$(_jq '.InstanceId')
    instName=$(_jq '.Name')
    instVpc=$(_jq '.VPC')
    instSubnet=$(_jq '.Subnet')
    instSGroups=$(_jq '.SecurityGroups[]')

    for instSG in $(echo $instSGroups | jq -r '. | @base64'); do
        _jq2() {
            echo ${instSG} | base64 --decode | jq -r ${1}
        }
        instSgId=$(_jq2 '.GroupId')
        instGName=$(_jq2 '.GroupName')

        echo -e "$instId\t$instName\t$instVpc\t$instSubnet\t$instSgId\t$instGName"
        printf "$instId\t$instName\t$instVpc\t$instSubnet\t$instSgId\t$instGName\n" >> $ec2File
    done
done
echo " "
echo " ----- "
echo " "

# Parse RDS Instance Associations
echo -e "RDS Security Group associations file will be saved to $rdsFile, tab-delimited.\n"
echo -e "DatabaseId\tSubnetGroup\tSubnetDescr\tVPC\tSecGroupId\tSecGroupName"
printf "DatabaseId\tSubnetGroup\tSubnetDescr\tVPC\tSecGroupId\tSecGroupName\n" >> $rdsFile
for rds in $(echo $rdsInstances | jq -r '.[] | @base64'); do
    _jq() {
        echo ${rds} | base64 --decode | jq -r ${1}
    }

    dbId=$(_jq '.DBInstance')
    dbSubnetGroup=$(_jq '.SubnetGroup')
    dbSubnetDesc=$(_jq '.SubnetDesc')
    dbVpc=$(_jq '.VPC')
    dbSGroups=$(_jq '.SecurityGroups[]')

    for dbSG in $(echo $dbSGroups | jq -r '. | @base64'); do
        _jq2() {
            echo ${dbSG} | base64 --decode | jq -r ${1}
        }
        dbSgId=$(_jq2 '.VpcSecurityGroupId')
        dbSgName=""

        dbSgDetail=$(grep $dbSgId $sgListFile)
        if [ $? -eq 0 ]; then
            dbSgName=(`echo $dbSgDetail | awk -F\t '{print $3}'`)
        fi

        echo -e "$dbId\t$dbSubnetGroup\t$dbSubnetDesc\t$dbVpc\t$dbSgId\t$dbSgName"
        printf "$dbId\t$dbSubnetGroup\t$dbSubnetDesc\t$dbVpc\t$dbSgId\t$dbSgName\n" >> $rdsFile
    done
done
echo " "
echo " ----- "
echo " "

# Parse ALB Instance Associations
echo -e "LoadBalancer Security Group associations file will be saved to $albFile, tab-delimited.\n"
echo -e "ALBName\tSchema\tVPC\tType\tSecGroupId\tSecGroupName"
printf "ALBName\tSchema\tVPC\tType\tSecGroupId\tSecGroupName\n" >> $albFile
for alb in $(echo $albInstances | jq -r '.[] | @base64'); do
    _jq() {
        echo ${alb} | base64 --decode | jq -r ${1}
    }

    albName=$(_jq '.LBName')
    albDir=$(_jq '.Direction')
    albVpc=$(_jq '.VPC')
    albType=$(_jq '.Type')
    albSGroups=$(_jq '.SecurityGroups[]')

    for albSG in $albSGroups; do
        albSgId="$albSG"
        albSgName=""

        albSgDetail=$(grep $albSgId $sgListFile)
        if [ $? -eq 0 ]; then
            albSgName=(`echo $albSgDetail | awk -F\t '{print $3}'`)
        fi

        echo -e "$albName\t$albDir\t$albVpc\t$albType\t$albSgId\t$albSgName"
        printf "$albName\t$albDir\t$albVpc\t$albType\t$albSgId\t$albSgName\n" >> $albFile
    done
done
echo " "
echo " ----- "
echo " "

IFS=$old_IFS

echo " *** SCRIPT COMPLETE ***"
```