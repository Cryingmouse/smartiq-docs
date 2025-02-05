# **Network Model**

``` mermaid
classDiagram
  class NetworkDevice {
    - **String** _name_
    - **String** _mac_address_
    - **String** _speed_
    - **String** _device_type_
    - **String** _pci_address_
    - **Bool** _is_rdma_enabled_
    + purchaseParkingPass()
    + addNetworkDevice()
  }
  
  class DNSRecord {
    - **IPv4Address | IPv6Address** _primary_ip_address_
    - **List[IPv4Address, IPv6Address]** _secondary_ip_address_
  }
  
  class NetworkInterface {
    - **String** _name_
    - **List[IPv4Interface|IPv6Interface]** _ip_interfaces_
    - **DNSRecord** _dns_record_
  }
  
  class Zone {
    - **String** _name_
    - **IPPool** _floating_ipv4_segment_
    - **IPPool** _floating_ipv6_segment_
    - **IPPool** _static_ipv4_segment_
    - **IPPool** _static_ipv6_segment_
    - **List[NetworkInterface]** _network_interfaces_
  }
  
  class Subnet {
    - **String** _name_
    - **IPv4Address** _ipv4_gateway_
    - **IPv6Address** _ipv6_gateway_
    - **IPv4Network** _ipv4_network_
    - **IPv6Network** _ipv6_network_
    - **List[Zone]** _zones_
  }
  
  class NetworkPlane {
    - **String** _name_
    - **List[NetworkDevice]** _network_devices_
    - **List[Subnet]** _subnets_
  }
  
  
  NetworkPlane o-- NetworkDevice
  NetworkInterface .. DNSRecord: linked
  Zone o-- NetworkInterface
  Subnet o-- Zone
  NetworkPlane o-- Subnet
```
