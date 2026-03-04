# Instalación de módulos — Orden 1 a 1 (obligatorio)

Instalar y validar **un módulo a la vez**:
1. cer_base
2. cer_catalog_github
3. cer_pricing
4. cer_booking
5. cer_documents
6. cer_communications
7. cer_portal

Comando base:
```bash
docker compose exec -T odoo odoo -d manzano_dev -c /etc/odoo/odoo.conf --db_host=db --db_user=odoo --db_password=odoo_manzano_pw -i <modulo> --stop-after-init
docker compose restart odoo
```
