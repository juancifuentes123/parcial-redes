# Informe Técnico: Despliegue Multi-Contenedor Orquestado con Docker Compose

**Asignatura:** Redes y Comunicaciones  
**Universidad Militar Nueva Granada**  

---

## 1. Arquitectura y Topología de Red

La infraestructura está compuesta por 5 contenedores orquestados mediante Docker Compose, aislados internamente y expuestos de manera segura al exterior mediante un Proxy Inverso (Nginx).

### Diagrama de Flujo y Redes
* **`frontend_net` (Red Bridge):** Interconecta únicamente a `nginx` y `joomla`.
* **`backend_net` (Red Bridge):** Interconecta a `nginx`, `joomla`, `database`, `jupyter` y `grafana`.

```text
[ Cliente / Navegador Web ]
             |
         (Puerto 80)
             v
   +------------------+
   |   nginx_proxy    | (Único servicio expuesto)
   +--------+---------+
            |
   +--------+-----------------------+-----------------------+
   | (frontend_net)                 | (backend_net)         |
   v                                v                       v
+------------+            +-------------------+    +-----------------+
| joomla_cms | <--------> |    postgres_db    | <--| jupyter_notebook|
+------------+            +---------+---------+    +-----------------+
                                    ^
                                    |
                          +---------+---------+
                          | grafana_dashboard |
                          +-------------------+