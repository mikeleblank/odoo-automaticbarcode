# 📘 Manual Completo – Generación Automática de Códigos de Barra en Odoo
### 🏷️ Versión profesional optimizada para GitHub

---

## 📝 Descripción general
Este documento explica cómo configurar **Odoo** para generar automáticamente **códigos de barras secuenciales (Code128)** al crear o actualizar productos sin código.  

Ideal para comercios, depósitos, logística y cualquier operación que requiera etiquetado automático.

---

# 🔧 1. Activar Modo Desarrollador
1. Ir a **Ajustes**
2. Desplazarse hasta el final
3. Click en **“Activar modo desarrollador”**

Si no aparece, agregar `?debug=1` al final de la URL.

---

# 🔢 2. Crear Secuencia Automática
Ruta:

**Ajustes → Técnico → Secuencias & Identificadores → Secuencias → Crear**

Configurar:

| Campo | Valor |
|-------|--------|
| Nombre | Secuencia Código de Barras |
| Código técnico | product.barcode.sequence |
| Prefijo | P |
| Relleno | 6 |
| Próximo número | 1 |

Ejemplo generado:

```
P000001
P000002
P000003
```

---

# 🛠️ 3. Crear Automatización en Odoo Studio
1. Inventario → Productos  
2. Abrir cualquier producto  
3. Click en **Odoo Studio**  
4. Ir a **Automatizaciones**  
5. Crear nueva regla

---

# ⚙ 4. Configurar Condición y Acción

## Encabezado
- **Nombre:** Generar código de barras automático  
- **Modelo:** product.template

## Disparadores
- ✔ Al crear  
- ✔ Al actualizar  

## Condición
```
[('barcode', '=', False)]
```

---

# 🧩 5. Acción → Ejecutar Código

Pegar el siguiente código:

```python
for rec in records:
    if not rec.barcode:
        sequence = env['ir.sequence'].next_by_code('product.barcode.sequence')
        rec.write({'barcode': sequence})
```

✔ Compatible con Odoo Online  
✔ No produce errores “forbidden opcode”  
✔ No sobrescribe códigos existentes  

---

# 🧪 6. Prueba del Funcionamiento
1. Crear producto nuevo  
2. Dejar vacío el campo Código de barras  
3. Guardar  

Resultado:

```
P000001
```

Nuevo producto:

```
P000002
```

---

# 🛡 7. Opcionales Recomendados

### Campo obligatorio
En Studio → seleccionar **barcode** → activar **Requerido**.

---

# 🖨️ 8. Impresión con TSC TE200
Odoo imprime etiquetas desde:

**Inventario → Productos → Imprimir → Etiquetas**

Compatible con:
- TSC TE200  
- Zebra ZD series  
- Cualquier impresora térmica 4"

---

# 📂 9. Estructura sugerida para el repositorio

```
docs/
 ├── manual_codigos_barra_odoo.md
 ├── ejemplos/
 ├── imagenes/
 └── plantillas/
README.md
```

---

# 🎉 Manual Completado
Este README.md está listo para subir directamente a GitHub.
