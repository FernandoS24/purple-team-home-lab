# Splunk SPL Queries — Purple Team Home Lab

## Query 1 — Detección de escaneo de puertos (Nmap)
index=* "CONEXION" "SRC=192.168.0.128"
**Detecta:** Todo el tráfico desde Kali hacia Metasploitable2.
**Eventos detectados:** 3,077

---

## Query 2 — Detección de ataque FTP vsftpd
index=* "CONEXION" "DPT=21" "SRC=192.168.0.128"
**Detecta:** Conexiones al puerto 21 desde el atacante.
**Eventos detectados:** 46

---

## Query 3 — Detección de fuerza bruta FTP (Hydra)
index=* "CONEXION" "DPT=21" "SRC=192.168.0.128"
**Detecta:** Cientos de intentos de login al FTP.
**Eventos detectados:** 5,431

---

## Dashboard
**Nombre:** Purple Team - Detecciones
**Splunk:** http://localhost:8000
