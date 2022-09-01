title:: 【Arprotector1.4】异集群注册站点同步未显示resource

- [[ARSTOR-1716] 【Arprotector1.4】异集群注册站点同步未显示resource - ArcherOS-Jira](http://jira.archeros.com/browse/ARSTOR-1716)
-
-
- ## backup vd
- ```
  protector-client-py register-site --management-platform CS --mount-point /mnt/zy_share --location-type HCI_local_NAS --addr 127.0.0.1 --site-id "38e929c6-634c-409e-852d-6b092426f53c" --site-mode RWD --domain-type Archer --nas-addr 127.0.0.1 --nas-directory /root/zy/share
  ```
- ### full
- ~/zy # protector-client-py show-backup --backup-id "e3667fd9-1f84-4cfd-84fc-07f89e62fe9d"
  {
          "status":       0,
          "data": {
                  "backup_id":    "e3667fd9-1f84-4cfd-84fc-07f89e62fe9d",
                  "name": "1661754372",
                  "timestamp":    "1661754378",
                  "site_id":      "38e929c6-634c-409e-852d-6b092426f53c",
                  "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                  "object_type":  "instance",
                  "type": "full backup",
                  "state":        "available",
                  "size": 1900609536,
                  "task_id":      "9b391d43-b42c-4fdd-a822-03356bd2a713",
                  "compression":  "",
                  "parent_id":    "",
                  "leader_backup_id":     "e3667fd9-1f84-4cfd-84fc-07f89e62fe9d",
                  "snapshot_md5": "",
                  "snapshot_path":        "",
                  "source_type":  "ArcherVD_NAS",
                  "management_platform":  "CS",
                  "volume_path":  "",
                  "description":  "create backup: fd470480-ac13-4f79-a501-5de6d8b0730c to 38e929c6-634c-409e-852d-6b092426f53c on host: archcnstcm4380",
                  "mount_point":  "/mnt/zy_share",
                  "scrub_metadata":       "",
                  "scrub_consistent":     "mtime_consistent",
                  "scrub_description":    "",
                  "checksum_method":      "none",
                  "snapshot_checksum_data":       "",
                  "keep_source_snapshot": "no",
                  "arch": "x86_64",
                  "sub_backups":  [{
                                  "type": "full backup",
                                  "state":        "available",
                                  "size": 1900609536,
                                  "backup_id":    "b6275518-93ea-4b8b-943e-7d3fe09c4101",
                                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                  "snapshot_remote":      "snapshots/backup/snapshot-28a29257-05b8-48ca-a68a-f8941bea730a",
                                  "snapshot_path":        "site-38e929c6-634c-409e-852d-6b092426f53c/09d6416a-f90a-11ec-97cf-60da833d46f7/fd470480-ac13-4f79-a501-5de6d8b0730c/fd470480-ac13-4f79-a501-5de6d8b0730c/1661754378/fd470480-ac13-4f79-a501-5de6d8b0730c_1661754378_0000.qcow2",
- ### inc1
- ```
  ~/zy # protector-client-py show-backup --backup-id "1510a85b-5b6b-4ab5-a0c9-d8ef3b334e3b"
  {
          "status":       0,
          "data": {
                  "backup_id":    "1510a85b-5b6b-4ab5-a0c9-d8ef3b334e3b",
                  "name": "1661754525",
                  "timestamp":    "1661754528",
                  "site_id":      "38e929c6-634c-409e-852d-6b092426f53c",
                  "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                  "object_type":  "instance",
                  "type": "incremental backup",
                  "state":        "available",
                  "size": 104873984,
                  "task_id":      "b1bbfa9a-a136-4271-ae73-5edf4b39ff39",
                  "compression":  "",
                  "parent_id":    "e3667fd9-1f84-4cfd-84fc-07f89e62fe9d",
                  "leader_backup_id":     "1510a85b-5b6b-4ab5-a0c9-d8ef3b334e3b",
                  "snapshot_md5": "",
                  "snapshot_path":        "",
                  "source_type":  "ArcherVD_NAS",
                  "management_platform":  "CS",
                  "volume_path":  "",
                  "description":  "create backup: fd470480-ac13-4f79-a501-5de6d8b0730c to 38e929c6-634c-409e-852d-6b092426f53c on host: archcnstcm4380",
                  "mount_point":  "/mnt/zy_share",
                  "scrub_metadata":       "",
                  "scrub_consistent":     "mtime_consistent",
                  "scrub_description":    "",
                  "checksum_method":      "none",
                  "snapshot_checksum_data":       "",
                  "keep_source_snapshot": "no",
                  "arch": "x86_64",
                  "sub_backups":  [{
                                  "type": "incremental backup",
                                  "state":        "available",
                                  "size": 104873984,
                                  "backup_id":    "e1fe6114-ceaa-48ef-976f-a0d6caef23ba",
                                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                  "snapshot_remote":      "snapshots/backup/snapshot-34e41cd6-36bf-446a-badd-348d04fd2306",
                                  "snapshot_path":        "site-38e929c6-634c-409e-852d-6b092426f53c/09d6416a-f90a-11ec-97cf-60da833d46f7/fd470480-ac13-4f79-a501-5de6d8b0730c/fd470480-ac13-4f79-a501-5de6d8b0730c/1661754378/fd470480-ac13-4f79-a501-5de6d8b0730c_1661754528_0001.qcow2",
  ```
- ### inc2
- ```
  ~/zy # protector-client-py show-backup --backup-id "370f255c-93fd-40a1-b35f-d5768f99fb45"
  {
          "status":       0,
          "data": {
                  "backup_id":    "370f255c-93fd-40a1-b35f-d5768f99fb45",
                  "name": "1661754629",
                  "timestamp":    "1661754638",
                  "site_id":      "38e929c6-634c-409e-852d-6b092426f53c",
                  "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                  "object_type":  "instance",
                  "type": "incremental backup",
                  "state":        "available",
                  "size": 10534912,
                  "task_id":      "95b55518-cf70-4505-84ba-ba21fe903594",
                  "compression":  "",
                  "parent_id":    "1510a85b-5b6b-4ab5-a0c9-d8ef3b334e3b",
                  "leader_backup_id":     "370f255c-93fd-40a1-b35f-d5768f99fb45",
                  "snapshot_md5": "",
                  "snapshot_path":        "",
                  "source_type":  "ArcherVD_NAS",
                  "management_platform":  "CS",
                  "volume_path":  "",
                  "description":  "create backup: fd470480-ac13-4f79-a501-5de6d8b0730c to 38e929c6-634c-409e-852d-6b092426f53c on host: archcnstcm4380",
                  "mount_point":  "/mnt/zy_share",
                  "scrub_metadata":       "",
                  "scrub_consistent":     "mtime_consistent",
                  "scrub_description":    "",
                  "checksum_method":      "none",
                  "snapshot_checksum_data":       "",
                  "keep_source_snapshot": "no",
                  "arch": "x86_64",
                  "sub_backups":  [{
                                  "type": "incremental backup",
                                  "state":        "available",
                                  "size": 10534912,
                                  "backup_id":    "984b645e-577a-429a-bf90-96eb2f7867d4",
                                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                  "snapshot_remote":      "snapshots/backup/snapshot-d2e46c9f-5960-44b1-9d09-a89175ae71d9",
                                  "snapshot_path":        "site-38e929c6-634c-409e-852d-6b092426f53c/09d6416a-f90a-11ec-97cf-60da833d46f7/fd470480-ac13-4f79-a501-5de6d8b0730c/fd470480-ac13-4f79-a501-5de6d8b0730c/1661754378/fd470480-ac13-4f79-a501-5de6d8b0730c_1661754638_0002.qcow2",
  
  ```
-
- ### show backup
- ```
  ~/zy # protector-client-py show-backup --backup-id "e3667fd9-1f84-4cfd-84fc-07f89e62fe9d"
  {
          "status":       0,
          "data": {
                  "backup_id":    "e3667fd9-1f84-4cfd-84fc-07f89e62fe9d",
                  "name": "1661754372",
                  "timestamp":    "1661754378",
                  "site_id":      "38e929c6-634c-409e-852d-6b092426f53c",
                  "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                  "object_type":  "instance",
                  "type": "full backup",
                  "state":        "available",
                  "size": 1900609536,
                  "task_id":      "9b391d43-b42c-4fdd-a822-03356bd2a713",
                  "compression":  "",
                  "parent_id":    "",
                  "leader_backup_id":     "e3667fd9-1f84-4cfd-84fc-07f89e62fe9d",
                  "snapshot_md5": "",
                  "snapshot_path":        "",
                  "source_type":  "ArcherVD_NAS",
                  "management_platform":  "CS",
                  "volume_path":  "",
                  "description":  "create backup: fd470480-ac13-4f79-a501-5de6d8b0730c to 38e929c6-634c-409e-852d-6b092426f53c on host: archcnstcm4380",
                  "mount_point":  "/mnt/zy_share",
                  "scrub_metadata":       "",
                  "scrub_consistent":     "mtime_consistent",
                  "scrub_description":    "",
                  "checksum_method":      "none",
                  "snapshot_checksum_data":       "",
                  "keep_source_snapshot": "no",
                  "arch": "x86_64",
                  "sub_backups":  [{
                                  "type": "full backup",
                                  "state":        "available",
                                  "size": 1900609536,
                                  "backup_id":    "b6275518-93ea-4b8b-943e-7d3fe09c4101",
                                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                  "snapshot_remote":      "snapshots/backup/snapshot-28a29257-05b8-48ca-a68a-f8941bea730a",
                                  "snapshot_path":        "site-38e929c6-634c-409e-852d-6b092426f53c/09d6416a-f90a-11ec-97cf-60da833d46f7/fd470480-ac13-4f79-a501-5de6d8b0730c/fd470480-ac13-4f79-a501-5de6d8b0730c/1661754378/fd470480-ac13-4f79-a501-5de6d8b0730c_1661754378_0000.qcow2",
                                  "snapshot_md5": "",
                                  "scrub_metadata":       "{\"mtime\":\"1661754404\"}",
                                  "scrub_description":    "",
                                  "snapshot_checksum_data":       ""
                          }],
                  "sites":        [{
                                  "ip":   "127.0.0.1",
                                  "type": 0
                          }],
                  "resource":     {
                          "ClusterId":    "09d6416a-f90a-11ec-97cf-60da833d46f7",
                          "Volumes":      [{
                                          "volume":       {
                                                  "migration_status":     null,
                                                  "attachments":  [{
                                                                  "server_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                                                                  "attachment_id":        "fba6f4ab-dfae-4af6-8339-382593ecd2ec",
                                                                  "attached_at":  "2022-08-29T02:18:21.000000",
                                                                  "iothread":     false,
                                                                  "host_name":    null,
                                                                  "volume_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                                                  "device":       "/dev/vda",
                                                                  "id":   "fd470480-ac13-4f79-a501-5de6d8b0730c"
                                                          }],
                                                  "links":        [{
                                                                  "href": "http://94.232.10.7:9776/v2/ad88dd5d24ce4e2189a6ae7491c33e9d/volumes/fd470480-ac13-4f79-a501-5de6d8b0730c",
                                                                  "rel":  "self"
                                                          }, {
                                                                  "href": "http://94.232.10.7:9776/ad88dd5d24ce4e2189a6ae7491c33e9d/volumes/fd470480-ac13-4f79-a501-5de6d8b0730c",
                                                                  "rel":  "bookmark"
                                                          }],
                                                  "availability_zone":    "nova",
                                                  "os-vol-host-attr:host":        "archcnstcm4381@arstor#Arstor",
                                                  "encrypted":    false,
                                                  "updated_at":   "2022-08-29T02:18:21.000000",
                                                  "replication_status":   "disabled",
                                                  "snapshot_id":  null,
                                                  "id":   "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                                  "size": 20,
                                                  "user_id":      "44faef681cd34e1c80b8520dd6aebad4",
                                                  "volume_count": null,
                                                  "qos_id":       null,
                                                  "os-vol-tenant-attr:tenant_id": "ad88dd5d24ce4e2189a6ae7491c33e9d",
                                                  "project_id":   "ad88dd5d24ce4e2189a6ae7491c33e9d",
                                                  "use_type":     "system",
                                                  "os-vol-mig-status-attr:migstat":       null,
                                                  "metadata":     {
                                                          "compression":  "LZ4",
                                                          "pageSize":     "8192",
                                                          "mirroring":    "2",
                                                          "readonly":     "False",
                                                          "readCache":    "true",
                                                          "qcow2_settings":       "{}",
                                                          "rebuildPriority":      "5",
                                                          "attached_mode":        "rw"
                                                  },
                                                  "status":       "in-use",
                                                  "backup_id":    null,
                                                  "description":  null,
                                                  "multiattach":  false,
                                                  "source_volid": null,
                                                  "consistencygroup_id":  null,
                                                  "provider_location":    "/cinder/volumes/volume_567",
                                                  "snapshot_count":       0,
                                                  "os-vol-mig-status-attr:name_id":       null,
                                                  "name": "zy_test1_root_disk",
                                                  "bootable":     "true",
                                                  "created_at":   "2022-08-29T02:18:12.000000",
                                                  "volume_type":  "basic-replica2"
                                          }
                                  }]
                  }
          }
  }
  
  ```