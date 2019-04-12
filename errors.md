---

copyright:

  years: 2018, 2019
  
lastupdated: "2019-04-09"

keywords: error, message, API, limitations, rias, support

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}

# IBM Cloud Virtual Private Cloud API error messages
{: #rias-error-messages}

When you receive an error message from the {{site.data.keyword.cloud}} Virtual Private Cloud APIs, you can use the message ID to find more information about how to resolve the problem.
{:shortdesc}

## acl_in_use
**Message**: The network ACL cannot be deleted because it is attached to resources.

The network ACL cannot be deleted because it is attached to a subnet or VPC. 

To see if a subnet is using the network ACL, use the `GET /v1/subnets?version=2019-01-01&generation=1` API.  Equivalent CLI command: `ibmcloud is subnets`.  To update the network ACL used by a subnet use the `PATCH /v1/subnets/{subnet_id}?version=2019-01-01&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API or `ibmcloud is subnet-update` CLI. 

To see if a VPC is using the network ACL, use the `GET /v1/vpcs?version=2019-01-01&generation=1` API.  Equivalent CLI command: `ibmcloud is vpcs`. It is not possible to update or delete the default network ACL used by a VPC. The default network ACL is deleted automatically when the VPC gets deleted.

## address_prefix_conflict
**Message**: The address prefix with this CIDR is in use.

The CIDR for the requested address prefix conflicts with an existing address prefix.

To view the list of address prefixes for a VPC, use the `GET /vpcs/{vpc-id}/address_prefixes` API and check that the requested CIDR address is not in use by the `cidr` field of another address prefix.
Equivalent CLI command: `ibmcloud is vpc-address-prefix`

## address_prefix_in_use
**Message**: Cannot delete an address prefix in use.

One or more subnets are using the address prefix.  To determine which subnets are using the address prefix, use the `GET /v1/subnets?version=2019-01-01&generation=1` API command and  check the `ipv4_cidr_block` field.  Equivalent CLI command:  `ibmcloud is subnets` and check the `Subnet CIDR` column. You must delete all the subnets using the address prefix before the address prefix can be deleted.

## backend_service_unavailable
**Message**: The backend service is unavailable.

A backend cloud service that is used by VPC failed to respond. VPC uses multiple IBM Cloud services, such as:

- [Identity and Asset Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [Global Catalog](https://{DomainName}/catalog)
- Resource Controller
- Resource Manager

You can check [status](https://{DomainName}/status) of IBM Cloud services and try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## bad_field
**Message**: Correct instance UUID should be provided

One of the values provided in the request is incorrect. Check the `target` value in the error returned for clues as to which parameter was incorrect. In some cases, the UUID or volume in the request cannot be found. Provide a valid value and try again.

Refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window} for additional help. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## bad_request
**Message**: The information given was invalid, malformed, or missing a required field.

The request was not what we expected. Use the [API documentation](https://{DomainName}/apidocs/rias){: new_window} to help you format the request.

If you are following the specification but still get the error, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## classic_access_vpc_conflict_duplicate_res
**Message**: Only one Classic Access VPC can be created.

Only one VPC with Classic Access can be created per region. To list the VPC with Classic Access, run `ibmcloud is vpcs --classic-access`. The existing VPC with Classic Access must be deleted before another one with Classic Access can be created.

## classic_access_vpc_account_not_VRF_enabled
**Message**: Account must be VRF-enabled to create a classic access VPC.

The linked classic account is not VRF-enabled. A classic access VPC requires the linked classic account to be VRF-enabled. To enable your account for VRF, please refer to [VRF on IBM Cloud](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

## default_address_prefix_not_found
**Message**: Default address prefix not found.

You may see this error message when the default address prefix is not found.

## duplicate_error
**Message**: The input provided already exists.

The resource specified already exists. Try using a different name for the resource you want to create. For example, when creating a new VPC, you can list existing VPCs by running `ibmcloud is vpcs`.  Choose a name that does not conflict.

## floating_ip_in_use
**Message**: The floating IP is in use.

This floating IP is already associated with a network interface or public gateway.

## floating_ip_not_empty
**Message**: Cannot delete a server with a floating IP associated.

Be sure to disassociate the floating IP before you delete the server.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## gateway_too_many
**Message**: Can only have one public gateway per zone in a VPC.

Only one public gateway per zone is allowed in a VPC but the one public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the GET public_gateways API and look at the "vpc" and "zone" values. If using the CLI, you can run the `ibmcloud is public-gateways` command and see the "VPC" and "Zone" value.

Use the public gateway's ID to attach it to a subnet, see an example in our [API examples](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-). If using the CLI, you can use the subnet update command to attach it to a public gateway, for example, `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`.

## health_monitor_invalid_delay
**Message**: The specified health monitor delay <health_monitor_delay> is invalid

Provide a value between 2 and 60 for the parameter `health monitor delay`.

## health_monitor_invalid_max_retries
**Message**:  The specified health monitor max retries <health_monitor_max_retries> is invalid

Provide a value between 1 and 10 for `health max retries`.

## health_monitor_invalid_timeout
**Message**:  The specified health monitor timeout <health_monitor_timeout> is invalid.

Provide a value between 1 and 59 for `health timeout`. The selected value must be less than the value of `health monitor delay`.

## health_monitor_invalid_url_path
**Message**: The specified health URL path <health_monitor_url> is invalid.

Provide a valid URL for the health monitor. The URL may contain alphanumeric characters, hyphens, and periods. No spaces or backslashes are allowed. The maximum length of the URL path is 255 characters.

## health_monitor_invalid_protocol
**Message**: The specified health monitor protocol <health_monitor_protocol> is invalid.

Provide a valid value for the health monitor protocol. Acceptable values are `http` and `tcp`.

## health_monitor_missing_protocol
**Message**:  Health monitor protocol is missing

Health monitor protocol is a required field. In your request, specify the health monitor protocol. The acceptable values are `http` and `tcp`.

## health_monitor_missing_url_path
**Message**:  Health monitor URL path is missing. It is required for a HTTP type health monitor.

For a health monitor using HTTP protocol, health monitor URL path is required field. Provide a valid URL path.

## health_monitor_timeout_delay_conflict
**Message**:  Health Monitor timeout <health_monitor_timeout> should not be greater than or equal to delay <health_monitor_delay>.

The value of the parameter `health monitor delay` must be greater than the value of `health monitor timeout`.

## http_request_size_exceeded
**Message**: The HTTP request is too large.

This problem occurs when the payload you have sent in your request has too many characters. Please try again with a smaller payload. For example, instead of trying to do everything in a single request, try creating a minimal resource in one request, and then appending state to it incrementally in several subsequent requests.

Refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window} for additional help. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## iam_failure
**Message**: None

A failure has occurred in the IAM service, verifying authentication or authorization. The outage of the IAM service may be temporary. Retry the request after few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## ike_policies_quota_exceeded
**Message**: The quota for IKE policies is exceeded for the account.

The quotas per resource are specified on the [Quotas](/docs/infrastructure/vpc?topic=vpc-quotas#vpn-quotas){: new_window} page.

To view current IKE policies, use the `GET /ike_policies` API.
Equivalent CLI command: `ibmcloud is ike-policies`

## ike_policy_duplicate_name
**Message**: The name `<ike_policy_name>` is in use already by IKE policy `<ike_policy_id>`.

Provide a different IKE policy name. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## ike_policy_in_use
**Message**: The IKE policy `<ike_policy_id>` is in use by one or more connections.

An IKE policy cannot be deleted if it is in use by one or more connections.

To see which connections are using the IKE policy, use the `GET /ike_policies/<ike_policy_id>/connections` API.
Equivalent CLI command: `ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
**Message**: The name `<ike_policy_name>` is not a valid IKE policy name.

A valid IKE policy name starts with a letter, followed by letters, digits, underscores or hyphens.

## ike_policy_not_found
**Message**: The IKE policy `<ike_policy_id>` could not be found.

You referenced an IKE policy that does not exist. Please review your request to ensure that you specified the proper IKE policy ID. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## instance_delete_conflict
**Message**: The instance cannot be deleted in the current status.

You cannot delete an instance when it is in `deleting`, `pending`, `starting`, `stopping`, or `restarting` status. The instance must be in `running` or `stopped` status. If the status does not change, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## instance_invalid_hostname
**Message**: The instance cannot be created because the hostname is not valid.

The instance hostname must meet the following requirements:
* The hostname must contain only alphanumeric characters and dashes
* Uppercase characters are not allowed
* Leading and trailing dashes are not allowed
* Leading numbers are not allowed
* Consecutive dashes are not allowed
* Maximum length is 15 characters for Windows
* Maximum length is 40 characters for other operating systems

## instance_invalid_port_speed
**Message**: The instance cannot be created because the specified network interface port speed is not valid.

The network interface port speed must be `100` or `1000` Mbps.

## instance_name_exists
**Message**: The same instance name already exists.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## instance_sec_volume_over_quota
**Message**: The instance cannot be created because the number of volume attachments would exceed the quota.

The VPC quotas per resource are specified on the [Quotas](/docs/infrastructure/vpc?topic=vpc-quotas){: new_window} page.

## instance_too_many_keys
**Message**: The Windows instance cannot be created because the request contains multiple keys.

Windows instances only support one key. 

## insufficient_space_for_subnet
**Message**: Insufficient space for subnet in address prefix.

The subnet cannot be created because the number of addresses requested cannot be allocated.

## internal_error
**Message**: An internal error occurred.

An unexpected error occurred. This problem may be temporary. Try the request again in a few minutes. If this error persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## internal_server_error
**Message**: None

An unexpected error occurred. This problem may be temporary. Try the request again in a few minutes. If this error persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## internal_solution
**Message**: Please contact your administrator.

An unexpected error occurred. This problem may be temporary. Try the request again in a few minutes. If this error persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## invalid_id_format
**Message**: Bad ID format. Ensure format is correct.

Make sure that the ID you provided does not contain any malformed data.

You may get this error message if a malformed start query is used when making a pagination request. For example,
`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-01-01&generation=1` contains two `?`s. Fix the query and try again.

## invalid_route
**Message**: The requested route does not exist.

The requested route on the API URL you provided does not exist. Verify that the URL you specified to call the API endpoint is correct.

## invalid_state
**Message**: An action was requested on a resource which is not supported at the current status of the resource.

1. A reboot operation may already be in progress for the instance. Refer to [actions allowed](/docs/infrastructure/vpc?topic=vpc-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance) depending on the status of the instance.
2. While the status of the instance is `pending`, you cannot perform the following actions:
* delete the instance
* attach a volume to the instance
* detach a volume from the instance
* update the instance

Try your action again later. If the status of the resource does not change, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## invalid_version
**Message**: The `version` parameter is invalid, it must be of the form `YYYY-MM-DD`.

The version must comply with the format _YYYY-MM-DD_. For single-digit months or dates, such as January 1st, the version should look like `2019-01-01`. The version parameter must be present in the URL. The date given in the version parameter must be later than 2019-01-01 but before the current date.

## invalid_version_range
**Message**: The `version` value cannot be set at a future date nor before `2019-01-01`.

The date given in the version parameter must be later than 2019-01-01 but before the current date.

## invalid_zone
**Message**: Please check whether the resources you are requesting are in the same zone.

You may see this message attempting to attach a public gateway in zone-1 to a subnet in zone-2. 

## ipsec_policies_quota_exceeded
**Message**: The quota for IPsec policies is exceeded for the account.

The quotas per resource are specified on the [Quotas](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#vpn-quotas){: new_window} page.

To view current IPsec policies, use the `GET /ipsec_policies` API.
Equivalent CLI command: `ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
**Message**: The name `<ipsec_policy_name>` is in use already by IPsec policy `<ipsec_policy_id>`.

Provide a different IPsec policy name. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## ipsec_policy_in_use
**Message**: The IPsec policy `<ipsec_policy_id>` is in use by one or more connections.

An IPsec policy cannot be deleted if it is in use by one or more connections.

To see which connections are using the IPsec policy, use the `GET /ipsec_policies/<ipsec_policy_id>/connections` API.
Equivalent CLI command: `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
**Message**: The name `<ipsec_policy_name>` is not a valid IPsec policy name.

A valid IPsec policy name starts with a letter, followed by letters, digits, underscores or hyphens.

## ipsec_policy_not_found
**Message**: The IPsec policy `<ipsec_policy_id>` could not be found.

You referenced an IPsec policy that does not exist. Please review your request and specify a valid IPsec policy ID. If you are sure the policy exists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## key_content_exists
**Message**: The same key content already exists.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## key_name_exists
**Message**: The same key name already exists.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## key_invalid_name
**Message**: The key cannot be created because the name is not valid.

The key name must meet the following requirements:
* must begin with alphabetical character, [A-Za-z]
* must contain only alphanumeric characters, dashes or underscores, [-A-Za-z0-9_]
* the length must not exceed 65 characters

## key_invalid_type
**Message**: The key cannot be created because the type is not valid.

The only supported key type is `rsa`.

## listener_certificate_not_found
**Message**: Certificate instance with CRN `<listener_certificate_crn>` cannot be found or no permission to access the certificate instance.

Please provide an existing certificate instance CRN or ask your account administrator to grant you access permissions to the provided certificate instance.

## listener_duplicate_port
**Message**: Port `<listener_port>` is used by another listener instance. Please choose a different port.

Please choose a different port.

## listener_invalid_certificate
**Message**: CRN of certificate instance for the HTTPS listener is invalid.

Please provide a valid certificate instance CRN.

## listener_invalid_connection_limit
**Message**: Listener connection limit `<listener_connection_limit>` in invalid.

Please provide a valid connection limit. Connection limit has to be an integer between 1 and 15000.

## listener_invalid_port
**Message**: Listener port `<listener_port>` is invalid.

Please choose a different port.

## listener_invalid_protocol
**Message**: Listener protocol `<listener_protocol>` is invalid.

Please provide a valid listener protocol. Acceptable values for listener protocol are `http`, `https`, and `tcp`.

## listener_missing_certificate_crn
**Message**: CRN of the certificate instance for the `https` listener is missing.

CRN of the certificate instance is a required field.

## listener_missing_port
**Message**: Listener port is missing.

Listener port is a required field.

## listener_missing_protocol
**Message**: Listener protocol is missing.

Listener protocol is a required field. Please provide the listener protocol in your request. The acceptable values are `http`, `https` and `tcp`.

## listener_not_found
**Message**: The listener with ID `<listener_id>` cannot be found.

Please provide an existing listener ID.

## listener_over_quota
**Message**: Listener cannot be created. Quota of listeners for the load balancer resource has reached the maximum limit.

The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## listener_pool_protocols_conflict
**Message**: Listener protocol(`<listener_protocol>`) and pool protocol(`<pool_protocol>`) are in conflict.

A listener with `https` or `http` protocol can only be associated with a pool with `http` protocol. Similarly, a `tcp` listener can only be associated with a `tcp` pool.

## listener_reserved_port_conflict
**Message**: Listener port `<listener_port>` is one of the reserved ports. The port range of 56500 to 56520 is reserved for management purposes. Please choose a different port.

Please choose a different port.

## load_balancer_delete_conflict
**Message**: The load balancer with ID <load_balancer_id> cannot be deleted because its status is one of these:  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Please delete the load balancer when it is in ACTIVE status.

## load_balancer_duplicate_name
**Message**: Name `<load_balancer_name>` is used by another load balancer instance. Please choose a different name.

Provide a different name for the load balancer instance. The load balancer name should be unique within the VPC and the account.

Your Load Balancer for VPC will not conflict with your Cloud Load Balancer, because DNS domain names are different for these services, and the name of the Load Balancer for VPC is not included in the DNS name.
{: note}

## load_balancer_insufficient_ips
**Message**: The subnet with ID(s) <subnet_id> has insufficient available IPv4 addresses.

In your request, provide valid subnets with available IPv4 addresses, where you wish to create the load balancer.

## load_balancer_invalid_description
**Message**: Load balancer description `<load_balancer_description>` is invalid.

A load balancer description cannot exceed 255 characters.

## load_balancer_invalid_is_public
**Message**: Value of is_public `<load_balancer_is_public>` is invalid.

The value of the load balancer parameter `is_public` must be either _true_ or _false_.

## load_balancer_invalid_name
**Message**: Name `<load_balancer_name>` is invalid.

Names may not be empty. A valid load balancer name starts with a letter, followed by letters, digits, or underscores. The length of the name may not exceed 40 characters. 

## load_balancer_invalid_subnet
**Message**: The subnet with ID <subnet_id>  is invalid. Please make sure to use an existing subnet with 'available' status.

Please provide valid subnets that are in `available` status when entering your request to create a load balancer.

## load_balancer_missing_is_public
**Message**: 'is_public' field is missing.

The field is `is_public` is required. Please provide a value for the `is_public` field.

## load_balancer_missing_name
**Message**: Load balancer name is missing.

The load balancer `name` is a required field. Please provide a name for the load balancer.

## load_balancer_missing_subnets
**Message**: Load balancer subnets is missing.

The field `subnets` is required. In your request to create a load balancer, provide information about the subnets in which you wish to create the load balancer.

## load_balancer_missing_subnet_id
**Message**: Subnet ID is missing.

The **Subnet ID** is a required field. Provide a subnet ID with your command.

## load_balancer_not_found
**Message**: The load balancer with ID `<load_balancer_id>` cannot be found.

Provide the ID of an existing load balancer.

## load_balancer_over_quota
**Message**: Load balancer cannot be created. Quota of load balancer instances under your account has reached maximum limit.

Delete an existing load balancer, or contact support to increase the load balancer quota on your account.

The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## load_balancer_subnet_not_found
**Message**: The subnet with ID <subnet_id>  cannot be found.

In your request, you must provide IDs of existing subnets in which you wish to create the load balancer.

## load_balancer_unchanged_update
**Message**: There is nothing to update the load balancer with ID `<load_balancer_id>`.

No information was given for updating a load balancer instance with the ID you specified.

## load_balancer_update_conflict
**Message**: The load balancer with ID <load_balancer_id> cannot be updated because its status is one of these:
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Please update the load balancer when it is in ACTIVE status.

## member_duplicate_address_and_port
**Message**:  Member with address <member_address> and port <member_port> already exists in the pool.

Provide a unique member address and port combination in your request.

## member_invalid_address
**Message**: The specified member IP address <member_ip> is invalid.

In your request, provide a valid IPv4 CIDR address for the member.

## member_invalid_port
**Message**: The specified member port `<member_port>` is invalid.

Provide a valid port number. A valid port number is in the range from 1 to 65535.

## member_invalid_weight
**Message**: The specified member weight `<member_weight>` is invalid.

Provide a valid member weight. The value must be an integer between 0 and 100.

## member_ip_not_found
**Message**: Member with IP address <member_IP> does not belong to any subnets in the Region and VPC of the load balancer.

In your request, provide the address of a member that belongs to a subnet in same the Region and VPC as the load balancer.

## member_missing_address
**Message**: Member address is missing.

Member address is a required field. Provide a value for member address.

## member_missing_protocol_port
**Message**: Member port is missing.

Member port is a required field. Provide a value for member port.

## member_not_found
**Message**:  Member with ID <member_id> cannot be found.

Please provide an existing member ID.

## member_over_quota
**Message**: Member cannot be created. Quota of member instances under the pool has reached maximum limit.

The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## missing_ims_account_id
**Message**: None

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## missing_version
**Message**: The `version` parameter is required, and it must be of the form `YYYY-MM-DD`.

A version parameter is required for all API requests. The version must comply with the format _YYYY-MM-DD_. For single-digit months or dates, such as January 1st, the version should look like `2019-01-01`. The date given in the version parameter must be later than 2019-01-01 but before the current date. Check out these [API examples](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis) for how to provide the version parameter.

## network_conflict
**Message**: CIDR conflicts with existing Address Prefix in VPC.

You might see this message if you provided a network CIDR that conflicts with an existing network CIDR in the same IP space.

## not_authorized
**Message**: The request is not authorized.

A common reason you may see this error is if your IAM token is missing or expired. For instructions on how to generate a token, refer to [Creating a VPC using the REST APIs](/docs/infrastructure/vpc/example-code.html#creating-a-vpc-using-the-rest-apis). If the token is not expired, make sure the account you are using is authorized to use this function and you have the [correct permissions](/docs/infrastructure/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).

If you are supposed to be authorized but you are still getting this error, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## not_found
**Message**: Please check whether the resource you are requesting exists.

You referenced a resource that does not exist or one to which you do not have access. Please review your request to ensure that you specified the proper IDs and references.

Refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window} for additional help. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## not_implemented
**Message**: None

The method is not implemented. Check your request to make sure you are calling a [valid API](https://{DomainName}/apidocs/rias){: new_window}. [Contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support) if you expect this API to be implemented.

## not_in_address_prefix
**Message**: The provided CIDR does not fit in any of the address prefixes in the provided zone.

This error occurs if a user is trying to create a subnet with a CIDR that does not fall into any address prefixes for the given zone.

Run `GET /vpcs/{vpc_id}/address_prefixes` to get the list of address prefixes for the VPC. Look at the `cidr` and `zone` values of the response and make sure the subnet's `cidr` is a subset of the  `cidr` of the address prefix for the zone you are trying to create it.

## over_quota
**Message**: The request would exceed the quota for a resource type.

The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## password_not_ready
**Message**: None

Your instance must be running before you can retrieve the password. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## pool_delete_conflict
**Message**: Pool cannot be deleted because it is still associated with one or more listeners.

Please ensure the pool is disassociate with any listeners and try again.

## pool_duplicate_name
**Message**: Pool name `<pool_name>` is already used by another pool. Please choose a different name.

Please choose a different pool name.

## pool_invalid_name
**Message**:  Name <pool_name> is invalid.

Please provide a valid pool name.  A valid load balancer name starts with a letter followed by letters, digits, hyphens and  the length of the name should not exceed 40 characters.

## pool_invalid_session_persistence
**Message**: Pool session persistence value is not valid.

Please provide a valid session persistence value. Acceptable value is SOURCE_IP

## pool_missing_algorithm
**Message**: Load balancing algorithm is missing.

Load balancing algorithm is a required field. Please provide the load balancing algorithm in your request. The acceptable values are `round_robin`, `weighted_round_robin` and `least_connections`.

## pool_missing_health_monitor
**Message**: Pool health monitor is missing.

Pool health monitor is a required field.

## pool_missing_members
**Message**: Pool members are missing.

Please provide pool members.

## pool_missing_name
**Message**: Pool name is missing.

Pool name is a required field.

## pool_missing_protocol
**Message**: Pool protocol is missing.

Pool protocol is a required field. Please provide the pool protocol in your request. The acceptable values are `http` and `tcp`.

## pool_not_found
**Message**: The pool with ID `<pool_id>` cannot be found.

Please provide an existing pool ID.

## pool_over_quota
**Message**: Pool cannot be created. Quota of pools for the load balancer resource has reached maximum limit.

The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## public_gateway_in_use
**Message**: Cannot delete a public gateway when it is in use.

The public gateway currently is attached to one or more subnets. You must detach the public gateway from all subnets before you can delete it.

To see which subnet is using the public gateway, use the `GET /v1/subnets?version=2019-01-01&generation=1` API.  Equivalent CLI command: `ibmcloud is subnets`.  To detach the public gateway from the subnet, use the API command `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-01-01&generation=1` or the CLI command `ibmcloud is subnet-public-gateway-detach`. 

## rate_limit_exceeded
**Message**: Too many requests within a short time.

This error message is returned if too many requests are received within a specified time interval. Wait a while and try again.

## security_group_active_transactions
**Message**: The interface cannot be attached or detached until the instance appears in Active state.

The instance must be active before its network interfaces can be attached to a security group. Use `GET /v1/instances/{id}?version=2019-01-01&generation=1` or `ibmcloud is instance` to check on the status of the instance. Once the status is `running`, please try again. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## security_group_already_attached
**Message**: The interface is already attached to the security group.

The interface is already attached to the security group. Use `GET /v1/security_groups/{id}/network_interfaces?version=2019-01-01&generation=1` or `ibmcloud is security-group-network-interfaces` to view attached interfaces.


If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).


## security_group_exists
**Message**: The security group already exists.


Security group names must be unique. A security group with that name already exists. Use `GET /v1/security_groups?version=2019-01-01&generation=1` or `ibmcloud is security-groups` to view current security groups.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias) or the [Using Security Groups document](https://{DomainName}/docs/infrastructure/vpc-network?topic=vpc-network-using-security-groups){: new_window}.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).


## security_group_interfaces_attached
**Message**: Cannot delete the security group while interfaces are attached.


Detach all interfaces before deleting the security group. Use `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-01-01&generation=1` or `ibmcloud is security-group-network-interface-remove` to detach an interface.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias) or the
[Using Security Groups document](https://{DomainName}/docs/infrastructure/vpc-network?topic=vpc-network-using-security-groups){: new_window}.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).


## security_group_interfaces_per_sg_exceeded
**Message**: Exceeded limit of interfaces per security group.

Attaching another interface to the security group would exceed the limit of interfaces per security group. The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#security-groups-quotas){: new_window}. Evaluate your strategy and consider creating another security group with similar rules.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias)
or the [Using Security Groups document](https://{DomainName}/docs/infrastructure/vpc-network?topic=vpc-network-using-security-groups){: new_window}.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).


## security_group_last_security_group_is_default
**Message**: The default security group cannot be removed when it is the only security group attached.

A network interface must be attached to at least one security group.
It will be attached to the VPC's default security group if one is not specified.
Attach the interface to a different security group, then try again to detach it from the default security group.
For information on the default security group, refer to the [Using Security Groups document](https://{DomainName}/docs/infrastructure/vpc-network?topic=vpc-network-using-security-groups){: new_window}.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## security_group_limit_exceeded
**Message**: Exceeded security group limit.

You have attempted to create a new security group, but you are currently at your account quota. The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#security-groups-quotas){: new_window}. Evaluate your strategy for assigning instances to security groups. It is often possible to reduce the overall number of security groups by assigning multiple instances to the same security group. This strategy will reduce the number of security groups, and drop you below your account quota. In rare cases, generally for large organizations, there is a need for expanding the quota. In this case, please [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support) to inquire if this is possible.

## security_group_network_interface_not_active
**Message**: The interface cannot be attached because it is not active.

Network interfaces must be active before attaching to security groups. Use `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-01-01&generation=1` or `ibmcloud is instance-network-interface` to view the status of an interface. Once the status is 'available', please try again. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## security_group_not_attached
**Message**: The interface is not attached.

The interface is not attached to the security group. Use `GET /v1/security_groups/{id}/network_interfaces?version=2019-01-01&generation=1` or `ibmcloud is security-group-network-interfaces` to view attached interfaces.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias) or the [Using Security Groups document](https://{DomainName}/docs/infrastructure/vpc-network?topic=vpc-network-updating-the-default-security-group-using-the-cli){: new_window}.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).


## security_group_not_in_vpc
**Message**: The interface and security group are in different VPCs.

To attach a network interface to a security group, the instance must be in the same VPC as the security group. Use `GET /v1/security_groups/{id}?version=2019-01-01&generation=1` or `ibmcloud is security-group` to view the security groups details and VPC. Use `GET /v1/instances/{id}?version=2019-01-01&generation=1` or `ibmcloud is instance` to view the instance details and VPC.

## security_group_order_bindings
**Message**: Cannot delete the security group while it has pending instance orders.

If an instance was created with security group(s) specified on the network interfaces, the instance and network interfaces must be active before the security group can be deleted. Use `GET /v1/security_groups/{id}/network_interfaces?version=2019-01-01&generation=1` or `ibmcloud is security-group-network-interfaces` to view attached network interfaces and their status. Once the status of the interfaces is 'available', please try again. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## security_group_port_max_less_than_port_min
**Message**: TCP/UDP max port cannot be less than min port.

The maximum port value cannot be less than the minimum port value. Specify a maximum port value that is larger than the minimum port value.

## security_group_port_range_both_or_neither
**Message**: Port range must be unset, or both a minimum and maximum port must be set for 'tcp' and 'udp'.

Rules with a 'tcp' or 'udp' protocol may have an unset port range (to apply the rule to all traffic), or they may specify a port range. To restrict traffic to a single port, set both the minimum and maximum port to the port value. For example, the following command would create a rule to allow SSH on port 22: `ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`.

For further information on security group rule configurations, refer to the [API documentation](https://{DomainName}/apidocs/rias).


## security_group_port_range_invalid_protocol
**Message**: A port range was specified with a protocol of 'icmp'. A port range is only valid for a protocol of 'tcp' or 'udp'.

Rules with an 'icmp' protocol have a type and code. Rules with 'tcp' or 'udp' protocols have a port range. For further information on security group rule configurations, refer to the [API documentation](https://{DomainName}/apidocs/rias).

## security_group_remote_group_not_in_vpc
**Message**: The remote group is not in the same VPC as this security group.

Security group rules may refer to a remote security group, allowing traffic between attached interfaces of the two groups. Remote security groups must be in the same VPC. Use `GET /v1/security_groups?2019-01-01` or `ibmcloud is security-groups` to view current security groups and their VPCs.

For further instructions to fix this problem, refer to the [Using Security Groups document](https://{DomainName}/docs/infrastructure/vpc-network?topic=vpc-network-using-security-groups){: new_window}.


## security_group_remoting_rules
**Message**: Cannot delete the security group while remoting rules are attached.

The security group contains one or more rules referencing a remote security group. Use `GET /v1/security_groups/{id}/rules?version=2019-01-01&generation=1` or `ibmcloud is security-group-rules` to view the rules. The 'remote' field will specify any remote security groups. Please try again after deleting or editing the remoting rule.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias) or the [Using Security Groups document](https://{DomainName}/docs/infrastructure/vpc-network?topic=vpc-network-using-security-groups){: new_window}.


## security_group_remoting_rules_per_sg_exceeded
**Message**: Exceeded limit of remoting rules per security group.


Adding another rule refering to a remote security group would exceed the limit of remoting rules per security group. The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#security-groups-quotas){: new_window}.


## security_group_rules_per_sg_exceeded
**Message**: Exceeded limit of rules per security group.

Adding a rule would exceed the limit of rules per security group. The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#security-groups-quotas){: new_window}. Evaulate your strategy and consider creating another security group.


## security_group_sgs_per_interface_exceeded
**Message**: Exceeded limit of security groups per interface.

Attaching this interface to another security group would exceed the limit of security groups per interface. The quotas per resource are specified in [Quotas and limits for VPC](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#security-groups-quotas){: new_window}. Evaulate your strategy and consider adding rules to an existing security group.


## security_group_type_code_invalid_protocol
**Message**: An 'icmp' type/code was given, but the requested protocol was not 'icmp'. Set the protocol to 'icmp' or specify a port range.

Only rules with an 'icmp' protocol have a type and code. Rules with 'tcp' or 'udp' protocols have a port range. For further information on security group rule configurations, refer to the [API documentation](https://{DomainName}/apidocs/rias).

## security_group_vpc_default
**Message**: Cannot delete the default security group for a VPC.

The default security group cannot be deleted. For information on the default security group, refer to the guide on [Updating the default security group](https://{DomainName}/docs/infrastructure/vpc-network?topic=vpc-network-updating-the-default-security-group).

## service_manager_service_failure
**Message**: None

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## subnet_acl_conflict
**Message**: Cannot delete the network ACL, it is attached to a subnet.

Detach the network ACL from all subnets before attempting to delete it. If the problem persists even after detaching the network ACL, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## subnet_conflict
**Message**: CIDR conflicts with existing Subnet in VPC.

Run the `GET /subnets` API to retrieve all subnets in VPC. Make sure that the CIDR that you have provided is not being used by other subnets by checking the value of `ipv4_cidr_block`.

If using the CLI, you can run `ibmcloud is subnets` and look at "Subnet CIDR" value for conflicts.

Refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window} for additional help. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## subnet_not_empty
**Message**: The subnet is not empty, please contact your administrator.

There was a request to delete a subnet, but the subnet still has resources in it. Subnets may have instances, VPNs, load balancers, or public gateways in them. You must delete your resources in the subnet before you may delete the subnet.

In some situations, this error can occur even when the console shows 0 VSIs and 0 load balancers, because deletion is asynchronous and it may take a few minutes for the internal status to change. Try your subnet deletion again in a few minutes.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## subnet_not_empty_pgway_exists
**Message**: Cannot delete the subnet while it is attached to a public gateway. Please detach the public gateway and retry.

There was a request to delete a subnet, but the subnet still has a public gateway attached to it. You must delete or detach the public gateway before you may delete the subnet.

If using the CLI, you can run `ibmcloud is public-gateways` to list the public gateways and `ibmcloud is subnet-public-gateway-detach` to detach a public gateway from a subnet.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## subnet_not_empty_ipaddr_exists
**Message**: Cannot delete the subnet while it contains IP addresses. Please delete any server instance associated with the IP address and retry.

There was a request to delete a subnet, but the subnet still contains IP addresses. You must delete the server instance associated with the IP address before you may delete the subnet.

If using the CLI, you can run `ibmcloud is instances` to list the server instances and look at the "Address" value to determine its IP Address and "Status" to determine its status. Run `ibmcloud is instance-delete` to delete a server instance that doesn't already have a Status of "deleting".

In some situations, this error can occur even when the console shows 0 VSIs, because deletion is asynchronous and it may take a few minutes for the internal status to change. Try your subnet deletion again in a few minutes.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## subnet_not_empty_loadbalancer_exists
**Message**: Cannot delete the subnet while it contains a load balancer. Please delete the load balancer and retry.

There was a request to delete a subnet, but the subnet still contains a load balancer. You must delete the load balancer before you may delete the subnet.

If using the CLI, you can run `ibmcloud is load-balancers` to list the load balancers and look at the "Subnets" value to determine its Subnet and "Status" to determine its status. Run `ibmcloud is load-balancer-delete` to delete a load balancer that doesn't already have a Status of "deleting".

In some situations, this error can occur even when the console shows 0 load balancers, because deletion is asynchronous and it may take a few minutes for the internal status to change. Try your subnet deletion again in a few minutes.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## subnet_not_empty_vpn_gway_exists
**Message**: Cannot delete the subnet while it contains a VPN gateway. Please delete the VPN gateway and retry.

There was a request to delete a subnet, but the subnet still has a VPN gateway attached to it. You must delete the VPN gateway before you may delete the subnet.

If using the CLI, you can run `ibmcloud is vpn-gateways` to list the load balancers and look at the "Subnets" value to determine its Subnet and "Status" to determine its status. Run `ibmcloud is vpn-gateway-delete` to delete a VPN gateway that doesn't already have a Status of "deleting".

In some situations, this error can occur even when the console shows 0 VPN gateways, because deletion is asynchronous and it may take a few minutes for the internal status to change. Try your subnet deletion again in a few minutes.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## subnet_unknown_state
**Message**: None

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## system_limit_exceeded
**Message**: This operation would exceed a system limit.

You might receive this error message if you try to create an address prefix, but the maximum number of address prefixes already exists.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## target_in_use
**Message**: The target already has floating IP attached.

There was a request to attach a floating IP address to a server’s network interface, but the network interface already has a floating IP attached to it. 

To find the floating IP address currently attached to a network interface, run the API command `GET /v1/floating_ips?version=2019-01-01&generation=1` and look for the network interface ID in the `target.id` field.  

If you are using the CLI, run the command `ibmcloud is floating-ips` and look at the `Target` value. Be aware that the CLI truncates the network interface ID at the first dash (“-“) character. For example, a server’s primary network interface with an ID of `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` appears as `primary(abdfcb29-.)` in the CLI output. 

## token_invalid
**Message**: The service token was expired or invalid.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## token_missing
**Message**: The service token was empty or did not exist in the request.

Recreate a token and try again.

## validation_enum
**Message**: The value supplied was not a valid option.

One of the fields supplied has a value that must come from an enumeration of possible values.

For Network ACL rules, you may specify a direction:

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

For example, the following value would be invalid because `northbound` is not a valid option in the enumeration `[ inbound, outbound ]`.

```json
{
    "direction":"northbound"
}
```

Refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window} or the [CLI reference](/docs/infrastructure-service-cli-plugin?topic=infrastructure-service-cli-vpc-reference){: new_window} for acceptable values.

## validation_failure
**Message**: The JSON provided did not match the expected structure.

To fix this problem, be sure the content of your request is valid JSON and that your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}.

## validation_invalid_cidr
**Message**: The value is not a valid CIDR.

The value must be a valid internal CIDR block with a 0 host part.

Certain IP address ranges are reserved. More information about reserved IP ranges is available in our overview of [Using your VPC with Regions and Subnets](/docs/infrastructure/vpc-network?topic=vpc-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv4_cidr
**Message**: The value is not a valid IPv4 CIDR.

Must be a IPv4 internal CIDR block with a 0 host part.

Certain IP address ranges are reserved. More information about reserved IP ranges is available in our overview of [Using your VPC with Regions and Subnets](/docs/infrastructure/vpc-network?topic=vpc-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv6_cidr
**Message**: The value is not a valid IPv6 CIDR.

Currently, IPv6 is not supported. Please use an IPv4 address.

## validation_invalid_address
**Message**: The value is not a valid address.

Must be a valid IP address.

A list of individually reserved IP addresses is given in the [Regions and Subnets](/docs/infrastructure/vpc-network?topic=vpc-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) document.

## validation_invalid_ipv4_address
**Message**: The value is not a valid IPv4 address.

Provide a valid IPv4 address.

## validation_invalid_ipv6_address
**Message**: The value is not a valid IPv6 address.

Give a valid IPv6 address. Currently, IPv6 is not supported; use an IPv4 address.

## validation_invalid_field_type
**Message**: The value type does not match the field type.

To fix this problem, be sure the content of your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window} for the endpoint you are calling.

## validation_max_value
**Message**: A value provided for a parameter is larger than allowed.


Provide a smaller value that meets the maximum as given by the [specification](https://{DomainName}/apidocs/rias){: new_window}.


## validation_min_value
**Message**: A value provided for a parameter is smaller than allowed.

You may get this error if you try to create a subnet with `total_ipv4_address_count` less than 8.

Provide a larger value that meets the minimum as given by the [specification](https://{DomainName}/apidocs/rias){: new_window}.


## validation_not_null
**Message**: The field supplied must be null.

Be sure the value is null. Refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}.

Here is an invalid example:

```json
{
    "name": null
}
```

## validation_only_one
**Message**: The value must match one of the subschemas.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}.

## validation_discriminator_forbidden
**Message**: The discriminator field forbids this substructure.

For example, if you specify
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

The protocol is `icmp`, and _port_min_ is a `tcp` field, so you'll get an error.
1. validation_discriminator_required for missing `icmp` rules
2. validation_discriminator_forbidden for `tcp` fields with `icmp` specified

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## validation_internal_error
**Message**: None

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## validation_invalid_name
**Message**: The value is not a valid name.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## validation_invalid_ref
**Message**: The value is not a valid HREF.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## validation_non_empty_uuid
**Message**: None

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## validation_required_field
**Message**: Missing a required field.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## validation_unique_failed
**Message**: The field supplied must be unique.

You might have specified a name that is already in use. Please try a different value.

Make sure your request conforms to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## volume_attachment_delete_invalid_request
**Message**: The volume attachment cannot be deleted.

This operation is not allowed. The boot volume's volume attachment cannot be deleted because the boot volume is required by the instance.

## volume_attachment_update_invalid_request
**Message**: The volume attachment could not be updated because the request is invalid.

The volume attachment could not be updated for either of these reasons:

* The boot volume's volume attachment cannot be renamed
* The boot volume's volume attachment `delete_volume_on_instance_delete` field cannot be set to `true`

## volume_capacity_max
**Message**: The maximum volume capacity allowed is 2000 GB.

The volume capacity you specified exceeds the maximum volume capacity. Specify a value between 10 and 2000 GB. See the [IBM Cloud Block Storage for VPC](/docs/infrastructure/block-storage-is?topic=block-storage-is-block-storage-about) documentation for accepted capacity ranges for a Custom profile.

## volume_capacity_missing
**Message**: The required volume capacity value is missing in the request.

When you create a volume, you must specify a volume capacity based on this profile definition. For more information, see the [IBM Cloud Block Storage for VPC](/docs/infrastructure/block-storage-is?topic=block-storage-is-block-storage-about) documentation.

## volume_capacity_zero_or_negative
**Message**: The volume capacity should be greater than zero.

When creating a volume, the capacity value specified in the request must be a positive number between 10 GB and 2000 GB. See the [IBM Cloud Block Storage for VPC](/docs/infrastructure/block-storage-is?topic=block-storage-is-block-storage-about) documentation for supported volume capacity values.

## volume_encryption_key_account_id_mismatch
**Message**: The volume encryption key specified in the request does not belong to current user's account.

The Key Protect root key CRN does not match your IAM authorization account ID. Specify a different Key Protect root key CRN for your IAM account. See the [Key Protect](https://{DomainName}/docs/services/key-protect/index.html) documentation for more information.

## volume_encryption_key_cname_mismatch
**Message**: The volume encryption key specified in the request cannot be used for the current environment.

The volume encryption key that you specified in the request cannot be used in the current environment. Provide a root key CRN applicable to your environment.

## volume_encryption_key_crn_invalid
**Message**: The volume encryption key CRN specified in the request is not valid.

The Key Protect CRN you provided is not valid. Obtain the CRN of the root key in the
Cloud Identity and Access Management service. For more information, see the  [IBM Cloud Block Storage for VPC](/docs/infrastructure/block-storage-is?topic=block-storage-is-block-storage-encryption) documentation.

## volume_encryption_key_not_active
**Message**: The volume encryption key specified in the request is not active.

Activate the exising key or provide a new key. If you cannot activate your own key, contact your system administrator or [customer support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## volume_encryption_key_not_authorized
**Message**: You are not authorized to access the provided volume encryption key.

Provide another encryption key for which you have access or contact your system administator to obtain access to the current key.

## volume_encryption_key_not_implemented
The volume encryption key feature is not supported.

A volume could not be created using your encryption key. This version supports volumes with provider-managed encryption only. To use your own encryption key, use a version of Block Storage for VPC that supports customer-managed encryption.
Contact [customer support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support) for more information.

## volume_id_invalid
**Message**: The volume ID specified in the request parameter is not valid.

The volume ID you specified is not in the correct format. Verify that you correctly entered the volume ID and try again. If you're not sure of the volume ID, specify `ibmcloud is volumes` in the CLI or `GET volumes` in the API and search the list of volumes.

## volume_id_missing
**Message**: The volume ID is missing in the request parameter.

You must provide the volume ID in the request parameter when getting a specific volume.

## volume_iops_zero_or_negative
**Message**: The volume IOPS should be greater than zero.

When creating a volume, the IOPS value specified in the request must be a positive number. See the [IBM Cloud Block Storage for VPC](/docs/infrastructure/block-storage-is?topic=block-storage-is-block-storage-about) documentation for supported IOPS values.

## volume_name_duplicate
**Message**: The volume name specified in the request already exists.

Specify a unique volume name.

## volume_not_found
**Message**: A volume with the specified ID is not found.

Verify that the volume ID you entered is correct and try again. For a list of volumes, specify `ibmcloud is volumes` in the CLI or `GET volumes` in the API.

## volume_not_implemented
**Message**: The requested operation is not supported in this release.

Contact [customer support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support) for more information.

## volume_profile_capacity_iops_invalid
**Message**: The volume profile specified in the request is not valid for the provided capacity and/or IOPS.

The volume capacity and/or IOPS value that you specified in the request is not allowed by the volume profile. See the [IBM Cloud Block Storage for VPC](/docs/infrastructure/block-storage-is?topic=block-storage-is-block-storage-about) documentation for valid minimum and maximum capacity and IOPS values for a Custom profile.

## volume_profile_iops_invalid
**Message**: The volume profile specified in the request cannot accept custom IOPS.

Your volume profile does not accept a Custom IOPS value. You might have specified a Tiered IOPS profile, which does not require that you specify an IOPS value. If you want to provide a specific IOPS value, use the Custom IOPS profile.

## volume_profile_name_missing
**Message**: The required volume profile name is missing in the request.

The volume profile name is missing in the request body when creating a volume or in the request parameter when getting a volume. Provide a volume profile name in your request and try again. If you don't know the volume profile name, specify `ibmcloud is volume-profiles` in the CLI or use `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` in the API request and search the list.

## volume_profile_not_found
**Message**: A volume profile with the specified name is not found.

A volume profile with that name could not be found. Verify that you provided the correct volume profile name. If you don't know the volume profile name, specify `ibmcloud is volume-profiles` in the CLI or use `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` in the API request and search the list.

## volume_quota_reached
**Message**: The volume quota has been reached.

You are out of quota for the current account. Try deleting some volumes or contact [customer support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support) to increase your quota. See the documentation for information about [quotas for the Virtual Private Cloud infrastructure](/docs/infrastructure/vpc?topic=vpc-quotas).

## volume_resource_group_id_invalid
**Message**: The resource group ID specified in the request is not valid.

The resource group ID that you specified in the request is not valid. Verify the correct resource group ID. From the CLI, use the `ibmcloud is volume VOLUME_ID` command. The resulting information will include the resource group and resource group ID. See the [IBM Cloud CLI for VPC Reference](/docs/infrastructure-service-cli-plugin?topic=infrastructure-service-cli-vpc-reference#storage) for more information.

## volume_resource_group_not_authorized
**Message**: The current action is not authorized on the specified resource group.

You are not authorized to list or create volumes in the specified resource group. Use a different resource group for which you have permission or request permission from your administrator for list and create privileges on the resource group.

## volume_resource_group_not_found
**Message**: A resource group with the specified ID could not be found.

The resource group ID could not be found because you might have entered it incorrectly or it does not exist. Verify the correct resource group ID. From the CLI, use the `ibmcloud is volume VOLUME_ID` command. The resulting information will include the resource group and resource group ID. See the [IBM Cloud CLI for VPC Reference](/docs/infrastructure-service-cli-plugin?topic=infrastructure-service-cli-vpc-reference#storage) for more information.

## volume_start_not_found
**Message**: A volume with the ID specified as the page start parameter is not found.

The volume ID that you specified in the start parameter of the list volume call could not be found. Verify that the volume ID is correct and whether you have access to the volume.

## volume_status_not_available
**Message**: The volume with the specified ID is not available.

The volume is not in an Available state; it's status might be in a Pending state. Wait for volume to become available and try again. If the volume is being deleted, use a different volume. To check the volume's status, specify `ibmcloud is volume VOLUME_ID` in the CLI or use `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD` in the API request.

## volume_still_attached
**Message**: The volume is still attached to an instance.

You cannot delete a volume that is still attached to a VSI. If you set automatic deletion for the volume, wait for the VSI to be deleted and volume will be detached and deleted. If you did not set automatic deletion, detach the volume manually.

## volume_template_invalid
**Message**: Invalid volume template provided.

You provided an invalid volume template for creating a volume. See the [Regional Infrastructure Service API Reference](https://{DomainName}/apidocs/rias){: new_window} to verify that you are providing the correct parameters in the request body.

## volume_update_invalid_request
**Message**: The volume update request is not valid.

The update request body property that you specified is not valid. See the [Regional Infrastructure Service API Reference](https://{DomainName}/apidocs/rias){: new_window} for information and correct API request syntax.

## vpc_acl_conflict
**Message**: Cannot delete the default network ACL, it is attached to a VPC.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpc_not_empty
**Message**: The VPC cannot be deleted because it is not empty.

All resources must be removed from a VPC before it can be deleted.

You could receive this error if you still have a gateway in your VPC, even when all subnets are deleted, because the gateway can exist in the VPC without a subnet. It may be necessary to use the CLI to check for orphaned resources.

If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpc_resource_separation
**Message**: The resources are in different VPCs.

## vpn_connection_cidr_not_created
**Message**: A CIDR block could not be added to the VPN connection `<vpn_connection_id>`.

Provide a valid CIDR that meets the requirements given by the specification.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_connection_cidr_not_deleted
**Message**: A CIDR block could not be deleted from the VPN connection `<vpn_connection_id>`.

Provide a valid CIDR that exists on the connection. A connection must have at least one local and peer CIDR.

To view the connection's CIDR blocks, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` and `peer_cidrs` fields.
Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
**Message**: The CIDR block was not found in the VPN connection `<vpn_connection_id>`.

Provide a valid CIDR that is in the connection.

To view the connection's CIDR blocks, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` and `peer_cidrs` fields.
Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_valid
**Message**: The CIDR block `<cidr_block>` does not represent a valid address.

The value must be a valid internal CIDR block with no host bits set.

To view the connection's CIDR blocks, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` and `peer_cidrs` fields.
Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
**Message**: The CIDR block `<cidr_block_1>` overlaps with `<cidr_block_2>`. Two peer CIDR blocks cannot overlap on the same VPC and two local CIDR blocks cannot overlap on the same connection.

Provide a valid CIDR value that meets the requirements given by the specification.

To view the connection's CIDR blocks, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` and `peer_cidrs` fields.
Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**Message**: The name `<vpn_connection_name>` is already in use by VPN connection `<vpn_connection_id>`.

Provide a different connection name. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_connection_invalid_name
**Message**: The name `<vpn_connection_name>` is not a valid VPN connection name.

A valid connection name starts with a letter, followed by letters, digits, underscores, or hyphens.

## vpn_connection_invalid_psk_format
**Message**: The pre-shared key (PSK) `<psk>` is not in the valid format.

A valid PSK should only contain 6 to 128 characters which are letters, digits, `-`,`+`,`&`,`!`,`@`,`#`,`$`,`%`,`^`,`*`,`(`,`)`,`,`,`.`,`:`,`_`.

## vpn_connection_local_cidrs_required
**Message**: At least one local CIDR block is required when creating a VPN connection.

Provide a valid local CIDR value that meets the requirements given by the specification.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}.

## vpn_connection_local_subnets_quota_exceeded
**Message**: The quota for local subnets across VPN connections exceeded for VPN gateway `<vpn_gateway_id>`.

The quotas per resource are specified on the [Quotas](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#vpn-quotas){: new_window} page.

To view the current local subnets across VPN connections, use the `GET /vpn_gateways/<vpn_gateway_id>/connections` API and check the `local_cidrs` field for each connection.
Equivalent CLI command: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**Message**: The quota of '<quota_limit>' local subnets for VPN connection is exceeded.

The quotas per resource are specified on the [Quotas](https://{DomainName}/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window} page.

To view the current local subnets for a VPN connection, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `local_cidrs` field.
Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_found
**Message**: The VPN connection `<vpn_connection_id>` could not be found.

Check whether the connection ID is correct. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_connection_peer_cidrs_required
**Message**: At least one peer CIDR block is required when creating a VPN connection.

Provide a valid peer CIDR value that meets the requirements given by the specification.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}.

## vpn_connection_peer_subnets_quota_exceeded
**Message**: The quota for peer subnets across VPN connections is exceeded for the VPN gateway `<vpn_gateway_id>`.

The quotas per resource are specified on the [Quotas](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#vpn-quotas){: new_window} page.

To view the current peer subnets across VPN connections, use the `GET /vpn_gateways/<vpn_gateway_id>/connections` API and check the `peer_cidrs` field for each connection.
Equivalent CLI command: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**Message**: The quota of `<quota_limit>` peer subnets for VPN connection is exceeded.

The quotas per resource are specified on the [Quotas](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#vpn-quotas){: new_window} page.

To view the current peer subnets for a VPN connection, use the `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API and check the `peer_cidrs` field.
Equivalent CLI command: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connections_quota_exceeded
**Message**: The quota for VPN connections is exceeded for the VPN gateway `<vpn_gateway_id>`.

The quotas per resource are specified on the [Quotas](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#vpn-quotas){: new_window} page.

To view the current VPN connections for a VPN gateway, use the `GET /vpn_gateways/<vpn_gateway_id>/connections` API.
Equivalent CLI command: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_static_route_not_created
**Message**: Failed to add a static route for the CIDR block `<peer_cidr>`.

Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_connection_static_route_not_deleted
**Message**: Failed to remove a static route for the CIDR block `<peer_cidr>`.

Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_connection_update_cidrs_not_permitted
**Message**: CIDR blocks cannot be changed when updating a connection. Please use the correct API when creating or deleting CIDRs.

Provide a valid request value that meets the requirements given by the specification.

For further instructions to fix this problem, refer to the [API documentation](https://{DomainName}/apidocs/rias){: new_window}.

## vpn_gateway_duplicate_name
**Message**: The name `<vpn_gateway_name>` is already in use by VPN gateway `<vpn_gateway_id>`.

Provide a different VPN gateway name. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_gateway_initialization_error
**Message**: The VPN gateway could not be initialized.

Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_gateway_invalid_name
**Message**: The name `<vpn_gateway_name>` is not a valid VPN gateway name.

A valid VPN gateway name starts with a letter, followed by letters, digits, underscores, or hyphens.

## vpn_gateway_invalid_state
**Message**: The VPN gateway `<vpn_gateway_id>` is in an invalid state for the requested operation.

VPN Gateway must be in `available` status before you can operate it. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_gateway_ip_create_error
**Message**: Unable to create an IP address for the VPN gateway.

Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_gateway_not_found
**Message**: The VPN gateway `<vpn_gateway_id>` could not be found.

Check whether the VPN gateway ID is correct. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_gateway_subnet_not_found
**Message**: Could not find the subnet `<subnet_id>` for the given VPN gateway.

You've referenced a subnet that does not exist. Please review your request to ensure that you specified the proper subnet ID. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_gateway_subnet_status_error
**Message**: The VPN gateway could not be created due to subnet status.

Provide a subnet that is in `available` status. Try again in a few minutes. If this problem persists, [contact support](/docs/infrastructure/vpc?topic=vpc-getting-help-and-support).

## vpn_gateways_quota_exceeded
**Message**: Quota for VPN gateways exceeded for the account and/or the region.

The quotas per resource are specified on the [Quotas](https://{DomainName}/docs/infrastructure/vpc?topic=vpc-quotas#vpn-quotas){: new_window} page.

To view the current VPN gateways, use the `GET /vpn_gateways` API.
Equivalent CLI command: `ibmcloud is vpn-gateways`

## zone_inconsistency
**Message**: The resources must belong to the same zone.

* When you attach a volume, make sure the volume and the instance are in the same zone.
* When you associate a floating IP, make sure the floating IP and the subnet are in the same zone.
