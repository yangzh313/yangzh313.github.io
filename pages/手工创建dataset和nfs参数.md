- dataset
	- ```
	  {
	      "id": "zpool/site-061858a0-3b41-4e39-87cb-88bf6301bbed/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1234567890",
	      "type": "FILESYSTEM",
	      "name": "zpool/site-061858a0-3b41-4e39-87cb-88bf6301bbed/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1234567890",
	      "pool": "zpool",
	      "encrypted": false,
	      "encryption_root": null,
	      "key_loaded": false,
	      "children": [],
	      "managedby": {
	          "value": "10.25.2.24",
	          "rawvalue": "10.25.2.24",
	          "source": "LOCAL",
	          "parsed": "10.25.2.24"
	      },
	      "deduplication": {
	          "parsed": "off",
	          "rawvalue": "off",
	          "value": "OFF",
	          "source": "DEFAULT"
	      },
	      "mountpoint": "/mnt/zpool/site-061858a0-3b41-4e39-87cb-88bf6301bbed/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1234567890",
	      "aclmode": {
	          "parsed": "discard",
	          "rawvalue": "discard",
	          "value": "DISCARD",
	          "source": "INHERITED"
	      },
	      "acltype": {
	          "parsed": "posix",
	          "rawvalue": "posix",
	          "value": "POSIX",
	          "source": "INHERITED"
	      },
	      "xattr": {
	          "parsed": null,
	          "rawvalue": "sa",
	          "value": "SA",
	          "source": "LOCAL"
	      },
	      "atime": {
	          "parsed": false,
	          "rawvalue": "off",
	          "value": "OFF",
	          "source": "INHERITED"
	      },
	      "casesensitivity": {
	          "parsed": "sensitive",
	          "rawvalue": "sensitive",
	          "value": "SENSITIVE",
	          "source": "NONE"
	      },
	      "checksum": {
	          "parsed": true,
	          "rawvalue": "on",
	          "value": "ON",
	          "source": "DEFAULT"
	      },
	      "exec": {
	          "parsed": true,
	          "rawvalue": "on",
	          "value": "ON",
	          "source": "DEFAULT"
	      },
	      "sync": {
	          "parsed": "standard",
	          "rawvalue": "standard",
	          "value": "STANDARD",
	          "source": "DEFAULT"
	      },
	      "compression": {
	          "parsed": "lz4",
	          "rawvalue": "lz4",
	          "value": "LZ4",
	          "source": "INHERITED"
	      },
	      "compressratio": {
	          "parsed": "1.00",
	          "rawvalue": "1.00",
	          "value": "1.00x",
	          "source": "NONE"
	      },
	      "origin": {
	          "parsed": "",
	          "rawvalue": "",
	          "value": "",
	          "source": "NONE"
	      },
	      "quota": {
	          "parsed": null,
	          "rawvalue": "0",
	          "value": null,
	          "source": "LOCAL"
	      },
	      "refquota": {
	          "parsed": null,
	          "rawvalue": "0",
	          "value": null,
	          "source": "LOCAL"
	      },
	      "reservation": {
	          "parsed": null,
	          "rawvalue": "0",
	          "value": null,
	          "source": "LOCAL"
	      },
	      "refreservation": {
	          "parsed": null,
	          "rawvalue": "0",
	          "value": null,
	          "source": "LOCAL"
	      },
	      "copies": {
	          "parsed": 1,
	          "rawvalue": "1",
	          "value": "1",
	          "source": "LOCAL"
	      },
	      "snapdir": {
	          "parsed": null,
	          "rawvalue": "hidden",
	          "value": "HIDDEN",
	          "source": "DEFAULT"
	      },
	      "readonly": {
	          "parsed": false,
	          "rawvalue": "off",
	          "value": "OFF",
	          "source": "DEFAULT"
	      },
	      "recordsize": {
	          "parsed": 131072,
	          "rawvalue": "131072",
	          "value": "128K",
	          "source": "DEFAULT"
	      },
	      "key_format": {
	          "parsed": "none",
	          "rawvalue": "none",
	          "value": null,
	          "source": "DEFAULT"
	      },
	      "encryption_algorithm": {
	          "parsed": "off",
	          "rawvalue": "off",
	          "value": null,
	          "source": "DEFAULT"
	      },
	      "used": {
	          "parsed": 142848,
	          "rawvalue": "142848",
	          "value": "140K",
	          "source": "NONE"
	      },
	      "available": {
	          "parsed": 194011167808,
	          "rawvalue": "194011167808",
	          "value": "181G",
	          "source": "NONE"
	      },
	      "special_small_block_size": {
	          "parsed": "0",
	          "rawvalue": "0",
	          "value": "0",
	          "source": "DEFAULT"
	      },
	      "pbkdf2iters": {
	          "parsed": "0",
	          "rawvalue": "0",
	          "value": "0",
	          "source": "DEFAULT"
	      },
	      "creation": {
	          "parsed": {
	              "$date": 1657781659000
	          },
	          "rawvalue": "1657781659",
	          "value": "Thu Jul 14 14:54 2022",
	          "source": "NONE"
	      },
	      "user_properties": {},
	      "locked": false
	  }
	  ```
- nfs
	- ```
	  {
	          "id": 43,
	          "paths": [
	              "/mnt/zpool/site-061858a0-3b41-4e39-87cb-88bf6301bbed/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1234567890"
	          ],
	          "aliases": [],
	          "comment": "",
	          "hosts": [],
	          "alldirs": false,
	          "ro": false,
	          "quiet": false,
	          "maproot_user": "",
	          "maproot_group": "",
	          "mapall_user": "",
	          "mapall_group": "",
	          "security": [],
	          "enabled": true,
	          "networks": [],
	          "locked": false
	      }
	  ```