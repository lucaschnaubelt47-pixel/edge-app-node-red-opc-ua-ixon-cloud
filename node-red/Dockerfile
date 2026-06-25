# Node-RED 5.x is required: its node loader uses dynamic import() instead of
# require(), so it can load node-red-contrib-opcua's ESM-only node-opcua deps.
# Node-RED 4.x fails to load them with ERR_REQUIRE_ESM.
FROM nodered/node-red:5.0.0-24

RUN npm install \
  node-red-contrib-opcua@0.2.352 \
  node-red-dashboard@3.6.6

# Additional Node-RED protocol nodes (commented for inspiration):
# Uncomment the relevant lines below to add support for other protocols as needed:

# Modbus (for Modbus TCP communication with PLCs)
# RUN npm install node-red-contrib-modbus

# Siemens S7 (for communication with Siemens PLCs)
# RUN npm install node-red-contrib-s7

# Ethernet/IP (for Rockwell/Allen-Bradley PLCs)
# RUN npm install node-red-contrib-ethernet-ip

# Mitsubishi MELSEC (for Mitsubishi PLC communication)
# RUN npm install node-red-contrib-mcprotocol

COPY flows.json /data/flows.json

# Expose Node-RED's default port
EXPOSE 1880
# Expose OPCUA port
# EXPOSE 50000

# Expose Modbus TCP port
# EXPOSE 502

# Expose Siemens S7 ports
# EXPOSE 102

# Expose Ethernet/IP ports
# EXPOSE 44818

# Expose Mitsubishi MELSEC port
# EXPOSE 1025

CMD ["npm", "start"]
