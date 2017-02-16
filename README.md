# OpenSIRF Documentation

Installing a development environment
===============================

Use the `install-dev.sh` script to install a development environment in your Linux system.

Requirements:
* Vagrant
* VirtualBox

# Running the installer

`$ wget https://raw.githubusercontent.com/OpenSIRF/opensirf-server/develop/install-dev.sh`

`$ bash install-dev.sh`

The installer will prompt you for the network interface to be used for communication between VMs. Be sure to select the interface that has a connection to the internet, as some packages need to be installed as part of the installation process.

# What gets installed

The script will install three virtual machines:

* OpenSIRF Server (devsirfserver)
* A VM with OpenStack Swift and OpenStack Identity (devsirfswift)
* A VM with a volume group and two logical volumes set up (devsirffs)

All the virtual machines are configured with the hostnames of the other machines. You need to configure the hostname of the `devsirfserver` machine only with the in `/etc/hosts`.

# Basic tests

## Retrieve configuration

`curl -i -X GET http://devsirfserver:8088/sirf/config`

## Create a new container

`curl -i -X PUT http://devsirfserver:8088/sirf/container/myContainer`

## Retrieve the new container's catalog

`curl -i -X GET http://devsirfserver:8088/sirf/container/myContainer/catalog`

## Post a new preservation object

`curl -i -X POST -H "Content-type:multipart/form-data" -F inputstream=@/home/phil/aaa -F poi="@po1.json;type=application/json" http://devsirfserver:8088/sirf/container/myContainer/po`

(this will post the file under `/home/phil/aaa`. The file `po1.json` must contain a JSON PreservationObjectInformation object with a `versionIdentifierUUID`. An example below:

```{
        "objectIdentifiers": [
          {
            "objectName": [

            ],
            "objectLogicalIdentifier": {
              "objectIdentifierLocale": "",
              "objectIdentifierType": "",
              "objectIdentifierValue": ""
            },
            "objectParentIdentifier": {
              "objectIdentifierLocale": "",
              "objectIdentifierType": "",
              "objectIdentifierValue": ""
            },
            "objectVersionIdentifier": {
              "objectIdentifierLocale": "en",
              "objectIdentifierType": "versionIdentifier",
              "objectIdentifierValue": "philPo1"
            }
          }
        ],
        "objectCreationDate": "20160925120201",
        "objectLastModifiedDate": "2017-01-30T15:37Z",
        "objectLastAccessedDate": "20160925120200",
        "objectRelatedObjects": [

        ],
        "packagingFormat": {
          "packagingFormatName": "testPackagingFormat"
        },
        "objectFixity": {
          "digestInformation": [
            {
              "digestAlgorithm": "SHA-1",
              "digestOriginator": "ObjectApi",
              "digestValue": "907353DBDC8E59DD341DD569728564DDBD05774B"
            }
          ],
          "lastCheckDate": "20170130153736"
        },
        "objectAuditLog": [

        ],
        "objectExtension": [

        ],
        "versionIdentifierUUID": "philPo1"
      }
```

## Retrieve the Preservation Object

`curl -i -X GET http://devsirfserver:8088/sirf/container/myContainer/philPo1/data`
