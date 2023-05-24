Base URL for the Prisma Access Insights API: https://pan.dev/access/docs/insights/getting_started-20/
To fetch the super set of the values sent in the filters: https://pan.dev/access/docs/insights/query_filters/
# Sample Request and Response Snippets

## Case 1: Fetch the Compute Location-SPN mapping and fetch the RN Site Status
REQUEST: POST /api/sase/v2.0/resource/custom/query/remotenetworks/rn_list
REQUEST HEADERS:
	User-Agent: python-requests/2.28.2 (PRISMA SASE SDK v6.1.2b1)
	Accept-Encoding: gzip, deflate
	Accept: application/json
	Connection: keep-alive
	authorization: Bearer exxxxxxxxxxxxxxxxxxxxxx
	X-PANW-Region: americas
	prisma-tenant: xxxxxxxxx       <<<<<<<<<<<<<<<<<<<<<< TSG ID of the tenant
	Content-Type: application/json
	Content-Length: 348
REQUEST BODY:
{
    "properties": [
        {
            "property": "edge_location_display_name",
            "alias": "location_name"
        },
        {
            "property": "site_name"
        },
        {
            "property": "ipsec_node"
        },
        {
            "property": "site_state_name"
        }
    ],
    "filter": {
        "operator": "AND",
        "rules": [
            {
                "property": "event_time",
                "operator": "last_n_days",
                "values": [
                    30
                ]
            },
            {
                "property": "node_type",
                "operator": "in",
                "values": [
                    48
                ]
            }
        ]
    }
}


RESPONSE DATA:
{
    "header": {
        "createdAt": "2023-05-23T17:52:54Z",
        "dataCount": 5,
        "requestId": "8xxxxxxxxxxxxxxxxxxxxxxxx",
        "queryInput": {
            "time_range": "last 30 day(s)",
            "event_time": {
                "from": "2023-04-23T00:00:00Z",
                "to": "2023-05-23T17:51:59Z",
                "from_epoch": 1682208000000,
                "to_epoch": 1684864319000
            }
        },
        "status": {
            "subCode": 200
        }
    },
    "data": [
        {
            "location_name": "US Southwest",
            "site_name": "AUXXXXXXXXXXXXXXXXXX",
            "ipsec_node": "us-southwest-pecan",
            "site_state_name": "Up"
        },
        {
            "location_name": "US Central",
            "site_name": "AUXXXXXXXXXXXXXXXX",
            "ipsec_node": "us-CENTRAL-KIWI",
            "site_state_name": "Up"
        } 
    ]
}

## Case 2: : Fetch the RN Location status
REQUEST: POST /api/sase/v2.0/resource/query/edge_location_current_status
REQUEST HEADERS:
	User-Agent: python-requests/2.28.2 (PRISMA SASE SDK v6.1.2b1)
	Accept-Encoding: gzip, deflate
	Accept: application/json
	Connection: keep-alive
	authorization: Bearer exxxxxxxxxxxxxxxxxxxxxxx
	X-PANW-Region: americas
	prisma-tenant: xxxxxxxxx      <<<<<<<<<<<<<<<<< TSG  ID of the tenant
	Content-Type: application/json
	Cookie: JSESSIONID=xxxxxxxxxxxxxx
	Content-Length: 206
REQUEST BODY:
{
    "properties": [
        {
            "property": "rn_state_instance"
        },
        {
            "property": "edge_location_display_name"
        }
    ],
    "filter": {
        "rules": [
            {
                "property": "rn_state_instance",
                "operator": "in",
                "values": [
                    0,
                    1,
                    2
                ]
            }
        ]
    },
    "count": 100
}

RESPONSE DATA:
{
    "header": {
        "createdAt": "2023-05-23T17:52:56Z",
        "dataCount": 3,
        "requestId": "xxxxxxxxxxxxxxxxxxxxx",
        "status": {
            "subCode": 200
        }
    },
    "data": [
        {
            "rn_state_instance": 1,
            "edge_location_display_name": "US Southwest"
        },
        {
            "rn_state_instance": 1,
            "edge_location_display_name": "US Central"
        }
    ]
}

## Case 3: Tunnel Status ,SPN throughput, SPN service IP (instance_public_ip)
REQUEST: POST /api/sase/v2.0/resource/custom/query/tunnels/tunnel_list
REQUEST HEADERS:
	User-Agent: python-requests/2.28.2 (PRISMA SASE SDK v6.1.2b1)
	Accept-Encoding: gzip, deflate
	Accept: application/json
	Connection: keep-alive
	authorization: Bearer exxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
	X-PANW-Region: americas
	prisma-tenant: xxxxxxxxxxxxxxxxxx
	Content-Type: application/json
	Cookie: JSESSIONID=xxxxxxxxxxxxxxxxxxxxx
	Content-Length: 237
REQUEST BODY:
{
    "filter": {
        "operator": "AND",
        "rules": [
            {
                "property": "event_time",
                "operator": "last_n_days",
                "values": [
                    30
                ]
            },
            {
                "property": "tunnel_state",
                "operator": "in",
                "values": [
                    1
                ]
            },
            {
                "property": "node_type",
                "operator": "in",
                "values": [
                    48
                ]
            }
        ]
    }
}


RESPONSE DATA:
{
    "header": {
        "createdAt": "2023-05-23T17:53:09Z",
        "dataCount": 7,
        "requestId": "2xxxxxxxxxxxxxxxxxxxx",
        "queryInput": {
            "time_range": "last 30 day(s)",
            "event_time": {
                "from": "2023-04-23T00:00:00Z",
                "to": "2023-05-23T17:52:59Z",
                "from_epoch": 1682208000000,
                "to_epoch": 1684864379000
            }
        },
        "status": {
            "subCode": 200
        }
    },
    "data": [
        {
            "node_type": 48,
            "okyo_tunnel_type": 0,
            "tunnel_name": "Axxxxxxxx",
            "node_type_name": "Remote Network",
            "site_name": "Axxxxxxxxxxxxxxxxxx",
            "edge_location_display_name": "US Southwest",
            "instance_name": "Fxxxxxxxxxxxxxxxxxx",
            "cloud_region_name": "us-west-201",
            "cloud_provider": "gcp",
            "instance_state": 1,
            "instance_state_name": "Up",
            "instance_public_ip": "x.x.x.x",
            "sub_node_type": 0,
            "tunnel_disconnection": 19,
            "tunnel_connection_duration": 233760000,
            "avg_throughput": 24.19,
            "peak_throughput": 1896.8,
            "tunnel_state": 1,
            "tunnel_state_name": "Up",
            "tunnel_monitoring_state": 1,
            "tunnel_monitoring_state_name": "Up",
            "tunnel_type_name": "Remote Network"
        },
        {
            "node_type": 48,
            "okyo_tunnel_type": 0,
            "tunnel_name": "Axxxxxxxxxxxxxxxxxxxx",
            "node_type_name": "Remote Network",
            "site_name": "Axxxxxxxxxxxxxxxxxxxxxxx",
            "edge_location_display_name": "US Southwest",
            "instance_name": "Fxxxxxxxxxxxxxxxxxxxxxx",
            "cloud_region_name": "us-west-201",
            "cloud_provider": "gcp",
            "instance_state": 1,
            "instance_state_name": "Up",
            "instance_public_ip": "x.x.x.x",
            "sub_node_type": 0,
            "tunnel_disconnection": 15,
            "tunnel_connection_duration": 233640000,
            "avg_throughput": 358.51,
            "peak_throughput": 21344.23,
            "tunnel_state": 1,
            "tunnel_state_name": "Up",
            "tunnel_monitoring_state": 2,
            "tunnel_monitoring_state_name": "Not Configured",
            "tunnel_type_name": "Remote Network"
        },
        
        {
            "node_type": 48,
            "okyo_tunnel_type": 0,
            "tunnel_name": "Rxxxxxxxxxx",
            "node_type_name": "Remote Network",
            "site_name": "RN-FW",
            "edge_location_display_name": "US Central",
            "instance_name": "Fxxxxxxxxxxxxxxxxxxxx",
            "cloud_region_name": "us-east-2",
            "cloud_provider": "gcp",
            "instance_state": 1,
            "instance_state_name": "Up",
            "instance_public_ip": "1xxxxxxxxxxxxxx",
            "sub_node_type": 0,
            "tunnel_disconnection": 3,
            "tunnel_connection_duration": 251760000,
            "avg_throughput": 0.8,
            "peak_throughput": 8684.54,
            "tunnel_state": 1,
            "tunnel_state_name": "Up",
            "tunnel_monitoring_state": 2,
            "tunnel_monitoring_state_name": "Not Configured",
            "tunnel_type_name": "Remote Network"
        },
      
    ]
}

## Case 4: Agg BW remaining in a compute location
REQUEST: POST /api/sase/v2.0/resource/custom/query/remotenetworks/rn_bandwidth_allocated
REQUEST HEADERS:
	User-Agent: python-requests/2.28.2 (PRISMA SASE SDK v6.1.2b1)
	Accept-Encoding: gzip, deflate
	Accept: application/json
	Connection: keep-alive
	authorization: Bearer exxxxxxxxxxxxxxxxxxx
	X-PANW-Region: americas
	prisma-tenant: xxxxxxxxx
	Content-Type: application/json
	Cookie: JSESSIONID=Lxxxxxxxxxxxxxxxxx
	Content-Length: 187
REQUEST BODY:
{
    "filter": {
        "rules": [
            {
                "property": "event_time",
                "operator": "last_n_days",
                "values": [
                    90
                ]
            },
            {
                "property": "edge_location_display_name",
                "operator": "not_equals",
                "values": [
                    "no_data"
                ]
            }
        ]
    }
}

RESPONSE DATA:
{
    "header": {
        "createdAt": "2023-05-23T17:53:11Z",
        "dataCount": 1,
        "requestId": "26xxxxxxxxxxxxxxxxx",
        "status": {
            "subCode": 200
        }
    },
    "data": [
        {
            "allocated_bw": 2500000.0,     >>>>>>>>>>>>>>>>>>>> BW allocated presently at the PA locations
            "allocated_aggregated_bw": 9000000.0 >>>>>>>>>>>>>> total aggregated BW
        }
    ]
}

## Case 5: Fetching min,max,avg,90th percentile per SPN in the past 1 hour
REQUEST: POST /api/sase/v2.0/resource/custom/query/remotenetworks/rn_spn_node_peak_bw_minute
REQUEST HEADERS:
	User-Agent: python-requests/2.28.2 (PRISMA SASE SDK v6.1.2b1)
	Accept-Encoding: gzip, deflate
	Accept: application/json
	Connection: keep-alive
	authorization: Bearer exxxxxx
	X-PANW-Region: americas
	prisma-tenant: xxxxxxxx
	Content-Type: application/json
	Cookie: JSESSIONID=Lxxxxxxxxxxxx
	Content-Length: 219
REQUEST BODY:
{
    "filter": {
        "rules": [
            {
                "property": "event_time",
                "operator": "between",
                "values": [
                    1684860791546,
                    1684864391546
                ]
            }
        ]
    },
    "histogram": {
        "property": "event_time",
        "enableEmptyInterval": true,
        "range": "minute",
        "value": "3"
    }
}


RESPONSE DATA:
{
    "header": {
        "createdAt": "2023-05-23T17:53:13Z",
        "dataCount": 119,
        "requestId": "0xxxxxxxxxxxxxxx",
        "queryInput": {
            "time_range": "custom",
            "event_time": {
                "from": "2023-05-23T16:53:11Z",
                "to": "2023-05-23T17:53:11Z",
                "from_epoch": 1684860791000,
                "to_epoch": 1684864391000
            },
            "histogram": true,
            "histogram_range": "minute",
            "histogram_value": "3"
        },
        "status": {
            "subCode": 200
        }
    },
    "data": [
        {
            "histogram_time": 1684860791000,
            "event_time": 1684860791000,
            "aggregate_region_display_name": "US Southwest",
            "spn_name": "us-southwest-pecan",
            "site_name": "Axxxxxxxxx",
            "tunnel_throughput_greatest": 46.626933333333334
        },
        {
            "histogram_time": 1684860791000,
            "event_time": 1684860791000,
            "aggregate_region_display_name": "US East",
            "spn_name": "us-east-cottonwood",
            "site_name": "Axxxxxxxxxxxxx",
            "tunnel_throughput_greatest": 938.5056
        },
       ..................
        {
            "histogram_time": 1684864391000
        }
    ]
}
+-------------------------+----------------+--------------------+----------------------------+----------------+
| SPN Name                | Min Throughput | Average Throughput | 90th percentile Throughput | Max Throughput |
+=========================+================+====================+============================+================+
| us-southwest-pecan      | 2.82           | 365.65             | 705.85                     | 1071.98        |
+-------------------------+----------------+--------------------+----------------------------+----------------+
| us-east-cottonwood      | 0.0            | 184.21             | 468.4                      | 938.51         |
+-------------------------+----------------+--------------------+----------------------------+----------------+
| us-central-kiwi         | 0.0            | 0.04               | 0.1                        | 0.1            |
+-------------------------+----------------+--------------------+----------------------------+----------------+
| us-central-rhododendron | 1.11           | 1.15               | 1.16                       | 1.16           |
+-------------------------+----------------+--------------------+----------------------------+----------------+
