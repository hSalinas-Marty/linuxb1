# Cours 4 Linux

## Étape 1 

### 1_

```powershell
[root@localhost ~]# crontab -l
```

### 2_

```powershell
[root@localhost /]# cat /etc/passwd
```
```powershell
[root@localhost /]# ls -alh /tmp
```
```powershell
[root@localhost /]# rm /tmp/.hidden_script
```
```powershell
[root@localhost /]# rm /tmp/.hidden_file
```
```powershell
[root@localhost /]# rm /tmp/malicious.sh
```
```powershell
[root@localhost /]# ls -alh /var/tmp
```

```powershell
[root@localhost /]# rm /var/tmp/.nop
```
```powershell
[root@localhost /]# ls -alh /home   
```
```powershell
[root@localhost /]# rm -r /home/attacker
```

### 3_

```powershell
[root@localhost /]# dnf install net-tools
```
```powershell
[root@localhost /]# netstat -a
```
## Étape 2

### 1_

```powershell
[root@localhost /]# lvs
```
```powershell
[root@localhost /]# lvcreate -L 500.00m -s -n secure_data_snap1 /dev/vg_secure/secure_data
```

### 2_

```powershell
[root@localhost /]# ls /mnt/secure_data
```
```powershell
[root@localhost /]# rm /mnt/secure_data/sensitive1.txt
```
```powershell
[root@localhost /]# umount /mnt/secure_data
```
```powershell
[root@localhost /]# lvconvert --mergesnapshot /dev/vg_secure/secure_data_snap1
```
```powershell
[root@localhost /]# mount /mnt/secure_data/
```

### 3_

```powershell
[root@localhost /]# lvextend -L +100M /dev/vg_secure/secure_data
``` 

## Étape 3

### 1_

```powershell
[root@localhost /]# mkdir backup
```
```powershell
[root@localhost /]# echo -e '#!/bin/bash\ntar --exclude='.tmp' --exclude='.log' -czvf /backup/secure_data$(date +\%F).tar.gz /mnt/secure_data' > secure_backup.sh
```
```powershell
[root@localhost /]# chmod +x secure_backup.sh
```

### 2_

```powershell
[root@localhost /]# nano secure_backup.sh
```
```powershell
#!/bin/bash

backup_dir="/backup"
secure_data="/mnt/secure_data"
archive_name="secure_data$(date +%F).tar.gz"

tar --exclude='.tmp' --exclude='.log' -czvf "$backup_dir/$archive_name" "$secure_data"

max_date=$(date +"%Y%m%d" -d "7 days ago")

for file in "$backup_dir"/*.tar.gz; do
  file_date=$(basename "$file" | grep -oE '[0-9]{4}-[0-9]{2}-[0-9]{2}' | sed 's/-//g')

  if [[ "$file_date" -lt "$max_date" ]]; then
    echo "Suppression de : $file"
    rm -f "$file"
  fi
done
```
### 3_

```powershell
[root@localhost /]# ./secure_backup.sh
```

### 4_

```powershell
[root@localhost /]# crontab -e
```
```powershell
* 3 * * * ./secure_backup.sh
```
```powershell
[root@localhost /]# crontab -l
```

## Étape 4 

### 1_

```powershell
[root@localhost /]# auditctl -w /etc -p wa -k etc-changes
```

### 2_

```powershell
[root@localhost /]# echo "yo" > /etc/test_audit.txt
```
```powershell
[root@localhost /]# find -name "audit.log"
```
```powershell
[root@localhost /]# cat ./var/log/audit/audit.log | grep test_audit.txt
```

### 3_

```powershell
[root@localhost /]# ausearch -k etc_changes > /var/log/audit_etc.log
```
```powershell
[root@localhost /]# cat var/log/audit_etc.log
```

## Étape 5

### 1_

```powershell
[root@localhost /]# firewall-cmd --list-all
```
```powershell
[root@localhost /]# firewall-cmd --permanent --add-port=80/tcp
```
```powershell
[root@localhost /]# firewall-cmd --permanent --add-port=22/tcp
```
```powershell
[root@localhost /]# firewall-cmd --permanent --add-port=443/tcp
```
```powershell
[root@localhost /]# firewall-cmd --permanent --add-service http
```
```powershell
[root@localhost /]# firewall-cmd --permanent --remove-service cockpit
```
```powershell
[root@localhost /]# firewall-cmd --permanent --remove-service dhcpv6-client
```
