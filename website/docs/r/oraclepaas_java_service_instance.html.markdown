---
layout: "opc"
page_title: "Oracle: oraclepaas_java_service_instance"
sidebar_current: "docs-oraclepaas-resource-service-instance"
description: |-
  Creates and manages a java service instance in an Oracle PaaS identity domain.

---

# oraclepaas_java_service_instance

The ``oraclepaas_java_service_instance`` resource creates and manages a java service instance in an Oracle PaaS identity domain.

## Example Usage

```hcl
resource "oraclepaas_database_service_instance" "default" {
  ...
}

resource "oraclepaas_java_service_instance" "default" {
    name = "test-java-service-instance-%d"
    edition = "SUITE"
    service_version = "12cRelease212"
    ssh_public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC3QxPp0BFK+ligB9m1FBcFELyvN5EdNUoSwTCe4Zv2b51OIO6wGM/dvTr/yj2ltNA/Vzl9tqf9AUBL8tKjAOk8uukip6G7rfigby+MvoJ9A8N0AC2te3TI+XCfB5Ty2M2OmKJjPOPCd6+OdzhT4cWnPOM+OAiX0DP7WCkO4Kx2kntf8YeTEurTCspOrRjGdo+zZkJxEydMt31asu9zYOTLmZPwLCkhel8vY6SnZhDTNSNkRzxZFv+Mh2VGmqu4SSxfVXr4tcFM6/MbAXlkA8jo+vHpy5sC79T4uNaPu2D8Ed7uC3yDdO3KRVdzZCfWHj4NjixdMs2CtK6EmyeVOPuiYb8/mcTybrb4F/CqA4jydAU6Ok0j0bIqftLyxNgfS31hR1Y3/GNPzly4+uUIgZqmsuVFh5h0L7qc1jMv7wRHphogo5snIp45t9jWNj8uDGzQgWvgbFP5wR7Nt6eS0kaCeGQbxWBDYfjQE801IrwhgMfmdmGw7FFveCH0tFcPm6td/8kMSyg/OewczZN3T62ETQYVsExOxEQl2t4SZ/yqklg+D9oGM+ILTmBRzIQ2m/xMmsbowiTXymjgVmvrWuc638X6dU2fKJ7As4hxs3rA1BA5sOt0XyqfHQhtYrL/Ovb1iV+C7MRhKicTyoNTc7oVcDDG0VW785d8CPqttDi50w=="
    force_delete = true	
    weblogic_server {
        shape = "oc3"
        database {
            name = "${oraclepaas_database_service_instance.test.name}"
            username = "sys"
            password = "Test_String7"
        }
        admin {
            username = "terraform-user"
            password = "Test_String7"
        }
    }
    cloud_storage {
        container = "Storage-%s/acctest-%d"
        auto_generate = true
    }
}
```

The following is an example of how to provision a service instance with the Oracle Traffic Director:

```hcl
resource "opc_database_service_instance" "default" {
  ...
}

resource "opc_java_service_instance" "default" {
    name = "java-service-instance-otd"
  	edition = "SUITE"
  	service_version = "12cRelease212"
    ssh_public_key = "ssh-key"
  	weblogic_server {
  		shape = "oc3"
  		database {
  			name = "${oraclepaas_database_service_instance.test.name}"
  			username = "sys"
  			password = "Test_String7"
  		}
  		admin {
  			username = "terraform-user"
  			password = "Test_String7"
  		}
  	}
  	cloud_storage {
  		container = "Storage-%s/acctest-%d"
  		auto_generate = true
  	}
  	otd {
  		admin {
  			username = "terraform-user"
  			password = "Test_String7"
  		}
  		shape = "oc1m"
  	}
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) The name of the Service Instance.

* `ssh_public_key` - (Required) The ssh key to connect to the java service instance.

* `edition` - (Required) The edition for the service instance. Possible values are `SE`, `EE`, or `SUITE`.

* `backups` - (Required) Provides Cloud Storage information for service instance backups. Backups
is documented below

* `metering_frequency` - (Optional) Billing unit. Possible values are `HOURLY` or `MONTHLY`. Default value is `HOURLY`.

* `availability_domain` - (Optional) Name of a data center location in the Oracle Cloud Infrastructure region that is specified in region. This is 
only available for OCI.

* `snapshot_name` - (Optional) Name of the snapshot to clone from.

* `source_service_name` - (Optional) Name of the existing Oracle Java Cloud Service instance that has the snapshot from which you are creating a clone.

* `subnet` - (Optional) A subdivision of a cloud network that is set up in the data center as specified in availabilityDomain. 
This is only available for OCI.

* `use_identity_service` - (Optional) Flag that specifies whether to use Oracle Identity Cloud Service (true) or the local WebLogic identity store
(false) for user authentication and to maintain administrators, application users, groups and roles. The default
value is false.

* `weblogic_server` - (Required) The attributes required to create a WebLogic server alongside the java service instance.
WebLogic Server is documented below.

* `otd` - (Optional) The attributes required to create an Oracle Traffic Director (Load balancer). OTD is
documented below.

* `level` - (Optional) Service level for the service instance. Possible values are `BASIC` or `PAAS`. Default
value is `PAAS`.

* `backup_destination` - (Optional) Specifies whether to enable backups for this Oracle Java Cloud Service instance.
Valid values are `BOTH` or `NONE`. Defaults to `BOTH`.

* `description` - (Optional) Provides additional on the java service instance.

* `enable_admin_console` - (Optional) Flag that specifies whether to enable (true) or disable (false) the access
rules that control external communication to the WebLogic Server Administration Console, Fusion Middleware Control,
and Load Balancer Console.

* `ip_network` - (Optional) The three-part name of a custom IP network to attach this service instance to. For example: `/Compute-identity_domain/user/object`.

* `region` - (Optional) Name of the region where the Oracle Java Cloud Service instance is to be provisioned.
This attribute is only applicable to accounts where regions are supported. A region name must be specified if you
want to use ipReservations or ipNetwork.

* `bring_your_own_license` - (Optional) Flag that specifies whether to apply an existing on-premises license for Oracle WebLogic Server (true) to the new
Oracle Java Cloud Service instance you are provisioning. The default value is `false`.

* `force_delete` - (Optional) Flag that specifies whether you want to force the removal of the service instance even if the database
instance cannot be reached to delete the database schemas. Default value is `false`

Backups supports the following:

* `cloud_storage_container` - (Required) Name of the Oracle Storage Cloud Service container used to provide storage for your service instance backups. Use the following format to specify the container name: `<storageservicename>-<storageidentitydomain>/<containername>`

* `cloud_storage_username` - (Optional) Username for the Oracle Storage Cloud Service administrator. If left unspecified,
the username for Oracle Public Cloud is used.

* `cloud_storage_password` - (Optional) Password for the Oracle Storage Cloud Service administrator. If left unspecified,
the password for Oracle Public Cloud is used.

* `create_if_missing` - (Optional) Specify if the given cloud_storage_container is to be created if it does not already exist. Default value is `false`.

WebLogic Server supports the following:

* `database` - (Required) Information about the database deployment on Oracle Database Cloud Service. Database
is documented below.

* `shape` - (Required) Desired compute shape.

* `admin` - (Required) Admin information for the WebLogic Server. Admin is documented below.

* `app_db` - (Optional) Details of Database Cloud Service database deployments that host application schemas.
Multiple can be specified. App DB is specified below.

* `backup_volume_size` - (Optional) Size of the backup volume for the service. The value must be a multiple of GBs.
You can specify this value in bytes or GBs. If specified in GBs, use the following format:
nG, where n specifies the number of GBs. For example, you can express 10 GBs as bytes or GBs.
For example: 100000000000 or 10G. This value defaults to the system configured volume size.

* `cluster_name` - (Optional) - Specifies the name of the cluster that contains the Managed Servers
for the service instance.

* `connect_string` - (Optional) - Connection string for the database. The connection string must be entered using one
of the following formats: host:port:SID, host:port/serviceName.

* `content_port` - (Optional) - Port for accessing the deployed applications using HTTP. Default value is 8001.

* `deployment_channel_port` - Port for accessing the Administration Server using WLST. Default value is 9001.

* `domain` - (Optional) Information about the WebLogic domain. Domain is documented below.

* `ip_reservations` - (Optional) A list of ip reservation names.

* `managed_servers` - (Optional) Details information about the managed servers the java service instance will
look after. Managed Servers is documented below.

* `mw_volume_size` - (Optional) Size of the MW_HOME disk volume for the service (/u01/app/oracle/middleware).
The value must be a multiple of GBs. You can specify this value in bytes or GBs.
If specified in GBs, use the following format: nG, where n specifies the number of GBs.
For example, you can express 10 GBs as bytes or GBs. For example: 100000000000 or 10G.
This value defaults to the system configured volume size.

* `node_manager` - (Optional) Node Manager is a WebLogic Server utility that enables you to start, shut down,
and restart Administration Server and Managed Server instances from a remote location. Node Manager is documented
below.

* `pdb_service_name` - (Optional) Name of the pluggable database for Oracle Database 12c. The default value is the
pluggable database name when the database was created.

* `ports` - (Optional) A block of port specifications. Weblogic Server Ports are specified below.

* `upper_stack_product_name` - (Optional) The Oracle Fusion Middleware product installer to add to this Oracle Java
Cloud Service instance. Valid values are `ODI` (Oracle Data Integrator) or `WCP` (Oracle WebCenter Portal)

* `root_url` - (Computed) The URL of the WebLogic Server Administration console.

OTD supports the following:

* `admin` - (Required) Admin information for the Oracle Traffic Director. Admin is documented below.

* `shape` - (Required) Desired compute shape.

* `high_availability` - (Optional) Flag that specifies whether load balancer HA is enabled.
This value defaults to false (that is, HA is not enabled).

* `ip_reservations` - (Optional) A list of ip reservation names.

* `listener` - (Optional) Specifies the type and number of the listener port. Listener is documented below.

* `load_balancing_policy` - (Optional) Policy to use for routing requests to the load balancer. Valid policies
include: `least_connection_count`, `least_response_time`, `round_robin`. The default value is
`least_connection_count`.

* `root_url` - (Computed) The URL of the OTD console.

Database supports the following:

* `username` - (Required) Username for the database administrator.

* `password` - (Required) Password for the database administrator.

* `name` - (Required) Name of the database on the Database Cloud Service.

* `hostname` - (Computed) The hostname for the database.

Admin supports the following:

* `username` - (Required) Username for the WebLogic Server or Oracle Traffic Director administrator.

* `password` - (Required) Password for the WebLogic Server or Oracle Traffic Director administrator.

* `port` - (Optional) Port for accessing the WebLogic Server or Oracle Traffic Director using HTTP. The default values are 7001 for WebLogic Server or 8989 for Oracle Traffic Director.

* `secured_port` - (Optional) Secured Port for accessing the WebLogic Server. The default value is 7002.

* `hostname` - (Computed) The hostname for the admin server on the WebLogic Server or OTD.

App DB supports the following:

* `username` - (Required) Username for the database administrator.

* `password` - (Required) Password for the database administrator.

* `name` - (Required) Name of the database deployment on the Database Cloud Service.

* `pdb_name` - (Optional) Name of the pluggable database for Oracle Database 12c. If not specified, the pluggable database name configured when the database was created will be used.

Domain supports the following:

* `mode` - (Optional) Mode of the domain. Valid values are `DEVELOPMENT`  or `PRODUCTION`. Default value is
`PRODUCTION`.

* `name` - (Optional) Name of the WebLogic domain. By default, the domain name will be generated from the first
eight characters of the Oracle Java Cloud Service instance name (serviceName), using the
following format: first8charsOfServiceInstanceName_domain.

* `partition_count` - (Optional) Number of partitions to enable in the domain for WebLogic Server 12.2.1.
Valid values include: 0 (no partitions), 1, 2, and 4.

* `volume_size` - (Optional) Size of the domain volume for the service. The value must be a multiple of GBs.
You can specify this value in bytes or GBs. If specified in GBs, use the following format:
nG, where n specifies the number of GBs. For example, you can express 10 GBs as bytes or GBs.
For example: 100000000000 or 10G.

Listener supports the following:

* `port` - (Optional) Listener port for the load balancer for accessing deployed applications using HTTP.
The default value is 8080.

* `enabled` - (Optional) Boolean on whether to enable the listener port. Default is true.

* `privileged_listener_port` - (Optional) Privileged listener port for accessing the deployed applications using HTTP. The default value is 80.

* `privileged_secured_listener_port` - (Optional) Privileged listener port for accessing the deployed applications using HTTPS. The default value is 443.

Managed Server supports the following:

* `server_count` - (Optional) Number of Managed Servers in the domain. Valid values include: 1, 2, 4, and 8.
The default value is 1.

* `initial_heap_size` - (Optional) Initial Java heap size for a Managed Server JVM, specified in megabytes.

* `max_heap_size` - (Optional) Maximum Java heap size for a Managed Server JVM, specified in megabytes.

* `jvm_args` - (Optional) One or more Managed Server JVM arguments separated by a space.

* `initial_permanent_generation` - (Optional) Initial Permanent Generation space in Java heap memory.

* `max_permanent_generation` - (Optional) Maximum Permanent Generation space in Java heap memory.

* `overwrite_jvm_args` - (Optional) Flag that determines whether the user defined Managed Server JVM arguments
specified in msJvmArgs should replace the server start arguments (true), or append the server start arguments
(false). Default is false.

Node Manager supports the following:

* `username` - (Optional) User name for the Node Manager. This value defaults to the WebLogic administrator user name.

* `password` - (Optional) Password for the Node Manager. This value defaults to the WebLogic administrator password.

* `port` - (Optional) Port for the Node Manager. This value defaults to 5556.

WebLogic Server Ports support the following:

* `privileged_content_port` - (Optional) Privileged content port for accessing the deployed applications using HTTP.
To disable the privileged content port, set the value to 0. The default value is 80.

* `priviliged_secured_content_port` - (Optional) Privileged content port for accessing the deployed applications using HTTPS.
To disable the privileged secured content port, set the value to 0. The default value is 443.

* `deployment_channel_port` - (Optional) Port for accessing the WebLogic Administration Server using WLST.
The default value is 9001.

* `content_port` - (Optional) Port for accessing the deployed applications using HTTP. The default value is 8001.

In addition to the above, the following values are exported:

* `uri` - The Uniform Resource Identifier for the Service Instance