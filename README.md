# Cyber
pentesting port scanner


#!/usr/bin/python3

import socket
import sys
import time
import threading

usage = "python3 pentesting.py TARGET START_PORT END_PORT"

print("-"*60)
print("Port Scanner Using Python")
print("-"*60)

start_time = time.time()

if(len(sys.argv) != 4):
    print(usage)
    sys.exit()

try:
    target = socket.gethostbyname(sys.argv[1])
except socket.gaierror: 
    print("Name resolution error")
    sys.exit()

start_port = int(sys.argv[2])
end_port = int(sys.argv[3])

print("Scanning Target", target)

def scan_port(port):

    #print("Scanning port:", port)
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(2)
    conn = s.connect_ex((target, port))
    if(not conn):
        print("Port {} is OPEN".format(port))
    s.close()


for port in range(start_port, end_port+1):

    thread = threading.Thread(target = scan_port, args = (port,))
    thread.start()

end_time = time.time()
print("Time Elapsed", end_time - start_time,'s')
