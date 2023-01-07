import socket
import struct

# Send an ICMP echo request to the specified IP address and return the response
def send_icmp_echo_request(ip_address):
  # Create a raw socket using the ICMP protocol
  sock = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_ICMP)
  sock.settimeout(2)
  
  # Construct the ICMP echo request message
  icmp_echo_request = struct.pack('bbHHh', 8, 0, 0, 1, 1)
  
  # Send the request to the specified IP address
  sock.sendto(icmp_echo_request, (ip_address, 1))
  
  # Try to receive a response
  try:
    data, addr = sock.recvfrom(1024)
    return data
  except socket.timeout:
    return None

# Perform a TCP scan on the specified IP address and port
def send_tcp_scan(ip_address, port):
  # Create a socket using the SOCK_STREAM type
  sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  sock.settimeout(2)
  
  # Try to connect to the specified IP address and port
  try:
    sock.connect((ip_address, port))
    return True
  except socket.error:
    return False

# Create a list of IP addresses to scan
ip_addresses = ['192.168.1.1', '192.168.1.2', '192.168.1.3']

# Iterate through the list of IP addresses
for ip_address in ip_addresses:
  # Send an ICMP echo request and perform a TCP scan on the current IP address
  icmp_response = send_icmp_echo_request(ip_address)
  tcp_scan_result = send_tcp_scan(ip_address, 80)
  
  # If the ICMP echo request was successful or the TCP scan was successful, consider the host to be active
  if icmp_response is not None or tcp_scan
