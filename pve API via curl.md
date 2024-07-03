## AUTHENTICATION
### get cookie and csrf token
```bash
curl --silent --insecure --data "username=root@pam&password=$PVE_PASSWD" \
 https://$APINODE:8006/api2/json/access/ticket \
| jq --raw-output '.data.ticket' | sed 's/^/PVEAuthCookie=/' > cookie
```

```bash
curl --silent --insecure --data "username=root@pam&password=$PVE_PASSWD" \
 https://$APINODE:8006/api2/json/access/ticket \
| jq --raw-output '.data.CSRFPreventionToken' | sed 's/^/CSRFPreventionToken:/' > csrftok
```

### get API token
```bash
```
---
## curl request parts
### GET req. via cookie
```bash
curl --silent --insecure --cookie "$(<cookie)" -X GET \
```

### GET req. via API token
```bash
curl --silent --insecure -H Authorization:\ PVEAPIToken=root@pam\!pdns-update-test=${PVEAPITOKEN} -X GET \
```

### tail of request command
```bash
https://$APINODE:8006/api2/json/nodes/${TARGETNODE}/ \
| jq '.'
```

---
### start vm
```bash
https://$APINODE:8006/api2/json/nodes/${TARGETNODE}/qemu/${VMID}/status/start \
```

### shutdown vm via qemu-ga
```bash
https://$APINODE:8006/api2/json/nodes/${TARGETNODE}/qemu/${VMID}/agent/shutdown \
```

### reboot vm via qemu-ga
```bash
https://$APINODE:8006/api2/json/nodes/${TARGETNODE}/qemu/${VMID}/status/reboot \
```

### get current status vm
```bash
https://$APINODE:8006/api2/json/nodes/${TARGETNODE}/qemu/${VMID}/status/current \
| jq '.data | .status,.qmpstatus'
```

---
### get backups list
```bash
https://$APINODE:8006/api2/json/nodes/${TARGETNODE}/storage/BKP/content\?content\=backup\&vmid\=${VMID} \
| jq '.data[] | [.notes,.volid]'
```

### make backup archive
```bash
https://$APINODE:8006/api2/json/nodes/${TARGETNODE}/vzdump\?vmid\=${VMID}\&storage\=BKP \
```

### restore vm from backup archive
```bash
https://${APINODE}:8006/api2/json/nodes/${TARGETNODE}/qemu\?vmid\=${VMID}\&force\=1\&storage\=VMS1\&archive\="BKP:backup/vm/${VMID}/${BACKUP_NAME}" \
```

