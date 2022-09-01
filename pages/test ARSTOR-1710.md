- ## vm backup
- ### full
- ```
  ~/zy # protector-client-py show-backup --backup-id "e603bf77-7893-4d17-8b5a-ee5a90890434"
  {
          "status":       0,
          "data": {
                  "backup_id":    "e603bf77-7893-4d17-8b5a-ee5a90890434",
                  "name": "1661742326",
                  "timestamp":    "1661742328",
                  "site_id":      "394e7e59-c53f-455a-9f33-a195ec3b1509",
                  "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                  "object_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                  "object_type":  "instance",
                  "type": "full backup",
                  "state":        "available",
                  "size": 1616019456,
                  "task_id":      "2641abf8-21dc-4b9b-ad84-5a109879e396",
                  "compression":  "",
                  "parent_id":    "",
                  "leader_backup_id":     "e603bf77-7893-4d17-8b5a-ee5a90890434",
                  "snapshot_md5": "",
                  "snapshot_path":        "",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "volume_path":  "",
                  "description":  "create backup: b372ee2b-f70d-4433-8c1c-8dfd2c641a42 to 394e7e59-c53f-455a-9f33-a195ec3b1509 on host: archcnstcm4380",
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
                                  "size": 1616019456,
                                  "backup_id":    "115d48b4-b8c7-4fb9-94f5-f9ef27e0ebeb",
                                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                  "snapshot_remote":      "snapshots/snapshot_536/snapshot-4168a31c-a900-4a0a-9352-4ab18ac4749d",
                                  "snapshot_path":        "site-394e7e59-c53f-455a-9f33-a195ec3b1509/09d6416a-f90a-11ec-97cf-60da833d46f7/b372ee2b-f70d-4433-8c1c-8dfd2c641a42/fd470480-ac13-4f79-a501-5de6d8b0730c/1661742328/fd470480-ac13-4f79-a501-5de6d8b0730c_1661742328_0000.qcow2",
                                  "snapshot_md5": "",
                                  "scrub_metadata":       "{\"mtime\":\"1661742382\"}",
                                  "scrub_description":    "",
                                  "snapshot_checksum_data":       ""
                          }, {
                                  "type": "full backup",
                                  "state":        "available",
                                  "size": 0,
                                  "backup_id":    "98ffe7d3-60e4-42b0-82f2-660c826cbfd4",
                                  "object_id":    "703fcf4f-165e-4231-b51b-ec9205fd0161",
                                  "snapshot_remote":      "snapshots/snapshot_679/snapshot-66992fed-b8da-41ad-8707-18c566212ece",
                                  "snapshot_path":        "site-394e7e59-c53f-455a-9f33-a195ec3b1509/09d6416a-f90a-11ec-97cf-60da833d46f7/b372ee2b-f70d-4433-8c1c-8dfd2c641a42/703fcf4f-165e-4231-b51b-ec9205fd0161/1661742328/703fcf4f-165e-4231-b51b-ec9205fd0161_1661742328_0000.qcow2",
  ```
- ### inc1
- ```
  ~/zy # protector-client-py show-backup --backup-id "ff3e89e7-9a40-4d5f-9f4b-aac54bffae9c"
  {
          "status":       0,
          "data": {
                  "backup_id":    "ff3e89e7-9a40-4d5f-9f4b-aac54bffae9c",
                  "name": "1661743316",
                  "timestamp":    "1661743318",
                  "site_id":      "394e7e59-c53f-455a-9f33-a195ec3b1509",
                  "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                  "object_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                  "object_type":  "instance",
                  "type": "incremental backup",
                  "state":        "available",
                  "size": 427073536,
                  "task_id":      "38ab8c51-4bef-45d7-ab0f-c8051a91d386",
                  "compression":  "",
                  "parent_id":    "e603bf77-7893-4d17-8b5a-ee5a90890434",
                  "leader_backup_id":     "ff3e89e7-9a40-4d5f-9f4b-aac54bffae9c",
                  "snapshot_md5": "",
                  "snapshot_path":        "",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "volume_path":  "",
                  "description":  "create backup: b372ee2b-f70d-4433-8c1c-8dfd2c641a42 to 394e7e59-c53f-455a-9f33-a195ec3b1509 on host: archcnstcm4380",
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
                                  "size": 215154688,
                                  "backup_id":    "25c926e7-399d-4bfa-b251-38c3f2f9a87f",
                                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                  "snapshot_remote":      "snapshots/snapshot_542/snapshot-e6ea13fe-12a8-4471-a410-198900ae2d34",
                                  "snapshot_path":        "site-394e7e59-c53f-455a-9f33-a195ec3b1509/09d6416a-f90a-11ec-97cf-60da833d46f7/b372ee2b-f70d-4433-8c1c-8dfd2c641a42/fd470480-ac13-4f79-a501-5de6d8b0730c/1661742328/fd470480-ac13-4f79-a501-5de6d8b0730c_1661743318_0001.qcow2",
                                  "snapshot_md5": "",
                                  "scrub_metadata":       "{\"mtime\":\"1661743372\"}",
                                  "scrub_description":    "",
                                  "snapshot_checksum_data":       ""
                          }, {
                                  "type": "incremental backup",
                                  "state":        "available",
                                  "size": 211918848,
                                  "backup_id":    "def6c2c6-e0d0-4d88-a822-51ca6f94a1b7",
                                  "object_id":    "703fcf4f-165e-4231-b51b-ec9205fd0161",
                                  "snapshot_remote":      "snapshots/snapshot_520/snapshot-e3380c66-d6c1-4907-b3cd-e2d9d67e1a86",
                                  "snapshot_path":        "site-394e7e59-c53f-455a-9f33-a195ec3b1509/09d6416a-f90a-11ec-97cf-60da833d46f7/b372ee2b-f70d-4433-8c1c-8dfd2c641a42/703fcf4f-165e-4231-b51b-ec9205fd0161/1661742328/703fcf4f-165e-4231-b51b-ec9205fd0161_1661743318_0001.qcow2",
  
  ```
- ### inc2
- ```
  ~/zy # protector-client-py show-backup --backup-id "a0b2e25b-5ed8-4aa2-8f4b-a85cb0a23cfb"
  {
          "status":       0,
          "data": {
                  "backup_id":    "a0b2e25b-5ed8-4aa2-8f4b-a85cb0a23cfb",
                  "name": "1661752119",
                  "timestamp":    "1661752128",
                  "site_id":      "394e7e59-c53f-455a-9f33-a195ec3b1509",
                  "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                  "object_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                  "object_type":  "instance",
                  "type": "incremental backup",
                  "state":        "available",
                  "size": 214368256,
                  "task_id":      "393a7eb0-2ae8-4f53-93f7-b9842c3ddf00",
                  "compression":  "",
                  "parent_id":    "ff3e89e7-9a40-4d5f-9f4b-aac54bffae9c",
                  "leader_backup_id":     "a0b2e25b-5ed8-4aa2-8f4b-a85cb0a23cfb",
                  "snapshot_md5": "",
                  "snapshot_path":        "",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "volume_path":  "",
                  "description":  "create backup: b372ee2b-f70d-4433-8c1c-8dfd2c641a42 to 394e7e59-c53f-455a-9f33-a195ec3b1509 on host: archcnstcm4380",
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
                                  "size": 109477888,
                                  "backup_id":    "9602e1ee-4efd-46d9-bf34-e7e1e582d9df",
                                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                  "snapshot_remote":      "snapshots/snapshot_437/snapshot-f21fa63d-1bb0-420c-8a13-f0342d8af6d5",
                                  "snapshot_path":        "site-394e7e59-c53f-455a-9f33-a195ec3b1509/09d6416a-f90a-11ec-97cf-60da833d46f7/b372ee2b-f70d-4433-8c1c-8dfd2c641a42/fd470480-ac13-4f79-a501-5de6d8b0730c/1661742328/fd470480-ac13-4f79-a501-5de6d8b0730c_1661752128_0002.qcow2",
                                  "snapshot_md5": "",
                                  "scrub_metadata":       "{\"mtime\":\"1661752172\"}",
                                  "scrub_description":    "",
                                  "snapshot_checksum_data":       ""
                          }, {
                                  "type": "incremental backup",
                                  "state":        "available",
                                  "size": 104890368,
                                  "backup_id":    "a274ab7e-4c6c-4540-bd05-1f26055a270b",
                                  "object_id":    "703fcf4f-165e-4231-b51b-ec9205fd0161",
                                  "snapshot_remote":      "snapshots/snapshot_244/snapshot-96d67dd5-e4d2-4634-a590-ae3426820045",
                                  "snapshot_path":        "site-394e7e59-c53f-455a-9f33-a195ec3b1509/09d6416a-f90a-11ec-97cf-60da833d46f7/b372ee2b-f70d-4433-8c1c-8dfd2c641a42/703fcf4f-165e-4231-b51b-ec9205fd0161/1661742328/703fcf4f-165e-4231-b51b-ec9205fd0161_1661752128_0002.qcow2",
  
  ```
- ### show backup
- ```
  ~/zy # protector-client-py show-backup --backup-id "e603bf77-7893-4d17-8b5a-ee5a90890434"
  {
          "status":       0,
          "data": {
                  "backup_id":    "e603bf77-7893-4d17-8b5a-ee5a90890434",
                  "name": "1661742326",
                  "timestamp":    "1661742328",
                  "site_id":      "394e7e59-c53f-455a-9f33-a195ec3b1509",
                  "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                  "object_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                  "object_type":  "instance",
                  "type": "full backup",
                  "state":        "available",
                  "size": 1616019456,
                  "task_id":      "2641abf8-21dc-4b9b-ad84-5a109879e396",
                  "compression":  "",
                  "parent_id":    "",
                  "leader_backup_id":     "e603bf77-7893-4d17-8b5a-ee5a90890434",
                  "snapshot_md5": "",
                  "snapshot_path":        "",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "volume_path":  "",
                  "description":  "create backup: b372ee2b-f70d-4433-8c1c-8dfd2c641a42 to 394e7e59-c53f-455a-9f33-a195ec3b1509 on host: archcnstcm4380",
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
                                  "size": 1616019456,
                                  "backup_id":    "115d48b4-b8c7-4fb9-94f5-f9ef27e0ebeb",
                                  "object_id":    "fd470480-ac13-4f79-a501-5de6d8b0730c",
                                  "snapshot_remote":      "snapshots/snapshot_536/snapshot-4168a31c-a900-4a0a-9352-4ab18ac4749d",
                                  "snapshot_path":        "site-394e7e59-c53f-455a-9f33-a195ec3b1509/09d6416a-f90a-11ec-97cf-60da833d46f7/b372ee2b-f70d-4433-8c1c-8dfd2c641a42/fd470480-ac13-4f79-a501-5de6d8b0730c/1661742328/fd470480-ac13-4f79-a501-5de6d8b0730c_1661742328_0000.qcow2",
                                  "snapshot_md5": "",
                                  "scrub_metadata":       "{\"mtime\":\"1661742382\"}",
                                  "scrub_description":    "",
                                  "snapshot_checksum_data":       ""
                          }, {
                                  "type": "full backup",
                                  "state":        "available",
                                  "size": 0,
                                  "backup_id":    "98ffe7d3-60e4-42b0-82f2-660c826cbfd4",
                                  "object_id":    "703fcf4f-165e-4231-b51b-ec9205fd0161",
                                  "snapshot_remote":      "snapshots/snapshot_679/snapshot-66992fed-b8da-41ad-8707-18c566212ece",
                                  "snapshot_path":        "site-394e7e59-c53f-455a-9f33-a195ec3b1509/09d6416a-f90a-11ec-97cf-60da833d46f7/b372ee2b-f70d-4433-8c1c-8dfd2c641a42/703fcf4f-165e-4231-b51b-ec9205fd0161/1661742328/703fcf4f-165e-4231-b51b-ec9205fd0161_1661742328_0000.qcow2",
                                  "snapshot_md5": "",
                                  "scrub_metadata":       "{\"mtime\":\"1661742332\"}",
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
                                  }, {
                                          "volume":       {
                                                  "migration_status":     null,
                                                  "attachments":  [{
                                                                  "server_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                                                                  "attachment_id":        "31f7756f-cdd7-4548-879a-4498c73543b7",
                                                                  "attached_at":  "2022-08-29T02:18:28.000000",
                                                                  "iothread":     false,
                                                                  "host_name":    null,
                                                                  "volume_id":    "703fcf4f-165e-4231-b51b-ec9205fd0161",
                                                                  "device":       "/dev/vdb",
                                                                  "id":   "703fcf4f-165e-4231-b51b-ec9205fd0161"
                                                          }],
                                                  "links":        [{
                                                                  "href": "http://94.232.10.7:9776/v2/ad88dd5d24ce4e2189a6ae7491c33e9d/volumes/703fcf4f-165e-4231-b51b-ec9205fd0161",
                                                                  "rel":  "self"
                                                          }, {
                                                                  "href": "http://94.232.10.7:9776/ad88dd5d24ce4e2189a6ae7491c33e9d/volumes/703fcf4f-165e-4231-b51b-ec9205fd0161",
                                                                  "rel":  "bookmark"
                                                          }],
                                                  "availability_zone":    "nova",
                                                  "os-vol-host-attr:host":        "archcnstcm4381@arstor#Arstor",
                                                  "encrypted":    false,
                                                  "updated_at":   "2022-08-29T02:18:28.000000",
                                                  "replication_status":   "disabled",
                                                  "snapshot_id":  null,
                                                  "id":   "703fcf4f-165e-4231-b51b-ec9205fd0161",
                                                  "size": 20,
                                                  "user_id":      "44faef681cd34e1c80b8520dd6aebad4",
                                                  "volume_count": null,
                                                  "qos_id":       null,
                                                  "os-vol-tenant-attr:tenant_id": "ad88dd5d24ce4e2189a6ae7491c33e9d",
                                                  "project_id":   "ad88dd5d24ce4e2189a6ae7491c33e9d",
                                                  "use_type":     "data",
                                                  "os-vol-mig-status-attr:migstat":       null,
                                                  "metadata":     {
                                                          "compression":  "LZ4",
                                                          "pageSize":     "4096",
                                                          "mirroring":    "2",
                                                          "readonly":     "False",
                                                          "readCache":    "True",
                                                          "qcow2_settings":       "{}",
                                                          "rebuildPriority":      "5",
                                                          "attached_mode":        "rw"
                                                  },
                                                  "status":       "in-use",
                                                  "backup_id":    null,
                                                  "description":  "",
                                                  "multiattach":  false,
                                                  "source_volid": null,
                                                  "consistencygroup_id":  null,
                                                  "provider_location":    "/cinder/volumes/volume_214",
                                                  "snapshot_count":       0,
                                                  "os-vol-mig-status-attr:name_id":       null,
                                                  "name": "volume_7453d",
                                                  "bootable":     "false",
                                                  "created_at":   "2022-08-29T02:18:21.000000",
                                                  "volume_type":  "basic-replica2"
                                          }
                                  }],
                          "server":       {
                                  "OS-EXT-STS:task_state":        null,
                                  "addresses":    {
                                          "protectorTestNet":     [{
                                                          "OS-EXT-IPS-MAC:mac_addr":      "fa:16:ac:00:02:06",
                                                          "version":      4,
                                                          "addr": "172.0.2.6",
                                                          "OS-EXT-IPS:type":      "fixed",
                                                          "net_id":       "6c1357b0-4685-4328-93b2-2ecd5eea132e"
                                                  }]
                                  },
                                  "links":        [{
                                                  "href": "http://94.232.10.7:9774//v2.1/ad88dd5d24ce4e2189a6ae7491c33e9d/servers/b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                                                  "rel":  "self"
                                          }, {
                                                  "href": "http://94.232.10.7:9774/ad88dd5d24ce4e2189a6ae7491c33e9d/servers/b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                                                  "rel":  "bookmark"
                                          }],
                                  "image":        "",
                                  "os-pci:pci_devices":   [],
                                  "OS-EXT-STS:vm_state":  "active",
                                  "OS-EXT-SRV-ATTR:instance_name":        "instance-0000005b",
                                  "OS-SRV-USG:launched_at":       "2022-08-29T02:18:36.000000",
                                  "flavor":       {
                                          "id":   "33eeb756-7b9f-436b-a7c2-d1fbbc3398dc",
                                          "links":        [{
                                                          "href": "http://94.232.10.7:9774/ad88dd5d24ce4e2189a6ae7491c33e9d/flavors/33eeb756-7b9f-436b-a7c2-d1fbbc3398dc",
                                                          "rel":  "bookmark"
                                                  }]
                                  },
                                  "id":   "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                                  "security_groups":      [{
                                                  "name": "test"
                                          }],
                                  "vm_ha_enabled":        0,
                                  "user_id":      "44faef681cd34e1c80b8520dd6aebad4",
                                  "OS-DCF:diskConfig":    "MANUAL",
                                  "priority":     1,
                                  "accessIPv4":   "",
                                  "accessIPv6":   "",
                                  "progress":     0,
                                  "OS-EXT-STS:power_state":       1,
                                  "OS-EXT-AZ:availability_zone":  "nova",
                                  "config_drive": "",
                                  "status":       "ACTIVE",
                                  "updated":      "2022-08-29T02:34:35Z",
                                  "hostId":       "bc09ca2c4ab11371caab93491338f751efcb058805adb6fae013ec39",
                                  "OS-EXT-SRV-ATTR:host": "archcnstcm4380",
                                  "OS-SRV-USG:terminated_at":     null,
                                  "floppy":       "",
                                  "key_name":     null,
                                  "OS-EXT-SRV-ATTR:hypervisor_hostname":  "archcnstcm4380",
                                  "name": "zy_test1",
                                  "created":      "2022-08-29T02:18:18Z",
                                  "tenant_id":    "ad88dd5d24ce4e2189a6ae7491c33e9d",
                                  "os-extended-volumes:volumes_attached": [{
                                                  "id":   "fd470480-ac13-4f79-a501-5de6d8b0730c"
                                          }, {
                                                  "id":   "703fcf4f-165e-4231-b51b-ec9205fd0161"
                                          }],
                                  "iso":  "",
                                  "metadata":     {
                                          "os_version":   "CentOS 7.x 64-bit",
                                          "storage_type": "ARSTOR",
                                          "virtio_net_pci_legacy":        "off",
                                          "os_type":      "linux",
                                          "hw_video_model":       "cirrus"
                                  }
                          }
                  }
          }
  }
  
  ```
-
- ### delete
- ```
  ~/zy # protector-client-py delete-nas-backup --backup-id "e603bf77-7893-4d17-8b5a-ee5a90890434"
  {
          "status":       0,
          "data": {
                  "task_id":      "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27"
          }
  }
  
  ~/zy # protector-client-py show-task --task-id "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27"
  {
          "status":       0,
          "data": {
                  "name": "1661769471",
                  "task_id":      "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27",
                  "type": "delete-nas-backup",
                  "backup_id":    "e603bf77-7893-4d17-8b5a-ee5a90890434",
                  "object_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                  "start_time":   "2022-Aug-29 18:37:51",
                  "end_time":     "",
                  "hostname":     "archcnstcm4380",
                  "server_ip":    "10.10.11.11",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "394e7e59-c53f-455a-9f33-a195ec3b1509",
                  "leader_task_id":       "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27",
                  "status":       "running",
                  "process":      0.390000,
                  "description":  "delete Archer backup: e603bf77-7893-4d17-8b5a-ee5a90890434 vm-id: ",
                  "error_code":   "",
                  "error_message":        ""
          }
  }
  ~/zy #
  ~/zy #
  ~/zy # protector-client-py show-task --task-id "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27"
  {
          "status":       0,
          "data": {
                  "name": "1661769471",
                  "task_id":      "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27",
                  "type": "delete-nas-backup",
                  "backup_id":    "e603bf77-7893-4d17-8b5a-ee5a90890434",
                  "object_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                  "start_time":   "2022-Aug-29 18:37:51",
                  "end_time":     "",
                  "hostname":     "archcnstcm4380",
                  "server_ip":    "10.10.11.11",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "394e7e59-c53f-455a-9f33-a195ec3b1509",
                  "leader_task_id":       "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27",
                  "status":       "running",
                  "process":      0.520000,
                  "description":  "delete Archer backup: e603bf77-7893-4d17-8b5a-ee5a90890434 vm-id: ",
                  "error_code":   "",
                  "error_message":        ""
          }
  }
  ~/zy #
  ~/zy #
  ~/zy # protector-client-py show-task --task-id "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27"
  {
          "status":       0,
          "data": {
                  "name": "1661769471",
                  "task_id":      "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27",
                  "type": "delete-nas-backup",
                  "backup_id":    "e603bf77-7893-4d17-8b5a-ee5a90890434",
                  "object_id":    "b372ee2b-f70d-4433-8c1c-8dfd2c641a42",
                  "start_time":   "2022-Aug-29 18:37:51",
                  "end_time":     "",
                  "hostname":     "archcnstcm4380",
                  "server_ip":    "10.10.11.11",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "394e7e59-c53f-455a-9f33-a195ec3b1509",
                  "leader_task_id":       "f3dcc4d1-3315-4db5-a6a5-63eb2a8ecc27",
                  "status":       "running",
                  "process":      0.710000,
                  "description":  "delete Archer backup: e603bf77-7893-4d17-8b5a-ee5a90890434 vm-id: ",
                  "error_code":   "",
                  "error_message":        ""
          }
  }
  
  ```