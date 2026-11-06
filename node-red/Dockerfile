
# =====================================================
# IXON Node-RED ARM64
# Node-RED 5.0.0 / Node.js 24
#
# Bestehende Module:
# - OPC UA
# - Dashboard
# - InfluxDB
# - Modbus
# - FTP
#
# Neu:
# - SQLite 2.0.1
# - Native ARM64-Kompilierung
# - SQLite-Funktionstest beim Build
# =====================================================

FROM nodered/node-red:5.0.0-24

# -----------------------------------------------------
# SQLite Build-Abhaengigkeiten
# -----------------------------------------------------

USER root

RUN apk add --no-cache \
    python3 \
    make \
    g++ \
    linux-headers

# Zurueck zum normalen Node-RED-Benutzer
USER node-red

WORKDIR /usr/src/node-red

# -----------------------------------------------------
# Bisherige Node-RED-Erweiterungen
# -----------------------------------------------------

RUN npm install \
    --no-audit \
    --no-fund \
    node-red-contrib-opcua@0.2.352 \
    node-red-dashboard@3.6.6 \
    node-red-contrib-influxdb@0.7.0 \
    node-red-contrib-modbus@5.62.1 \
    node-red-contrib-ftp-client@0.1.11

# -----------------------------------------------------
# Neu: SQLite fuer Keyence-Referenzstatistik
# -----------------------------------------------------

# Native Bibliothek fuer die Containerarchitektur
# kompilieren und installieren.

RUN npm_config_build_from_source=true \
    npm install \
    --no-audit \
    --no-fund \
    node-red-node-sqlite@2.0.1

# -----------------------------------------------------
# SQLite-Funktionstest beim GitHub-Build
# -----------------------------------------------------

RUN node -e "\
const m=require('node:module'); \
const r=m.createRequire(require.resolve('node-red-node-sqlite/package.json')); \
const sqlite3=r('sqlite3'); \
const db=new sqlite3.Database(':memory:'); \
db.get('SELECT 1 AS ok',(err,row)=>{ \
if(err||!row||row.ok!==1){ \
console.error(err||row); \
process.exitCode=1; \
} \
db.close(); \
});"

# -----------------------------------------------------
# Bestehende Flow-Konfiguration beibehalten
# -----------------------------------------------------

COPY flows.json /data/flows.json

# -----------------------------------------------------
# Ports
# -----------------------------------------------------

EXPOSE 1880

# -----------------------------------------------------
# Node-RED starten
# -----------------------------------------------------

CMD ["npm", "start"]

