# ✨ Feature Request

## 📌 Título

Módulo de Reportes – Exportación a Excel

---

## 📋 Descripción

Agregar la posibilidad de exportar los reportes de transacciones filtrados en la interfaz a un archivo Excel (.xlsx) descargable por el usuario.

---

## 🎯 Objetivo / Valor

- **Problema que resuelve:** Actualmente los usuarios solo pueden visualizar los reportes en pantalla o exportarlos a PDF, lo que dificulta el análisis y la manipulación de datos en herramientas externas.
- **Beneficio:** Permite a los usuarios trabajar con los datos en Excel, aplicar filtros adicionales, generar gráficos y compartir fácilmente la información con otras áreas.

---

## 🖥️ Alcance

- **Incluye:**

    - Botón “Exportar a Excel” en la vista de reportes.
    - Generación de archivo `.xlsx` con las columnas visibles en la tabla.
    - Descarga directa en el navegador.

- **No incluye:**
    - Exportación a otros formatos (CSV, JSON).
    - Personalización avanzada de columnas.
    - Programación de exportaciones automáticas.

---

## 🔄 Flujo esperado / Ejemplo de uso

1. Usuario aplica filtros en la vista de reportes (ej. rango de fechas, tipo de transacción).
2. Usuario hace clic en el botón “Exportar a Excel”.
3. El sistema genera un archivo `.xlsx` con los datos filtrados y lo descarga automáticamente en el navegador.

---

## 📂 Referencias

- Mockup en Figma: [link al diseño]
- Documentación relacionada: [link a la wiki interna]
- Issue vinculado: #123 (solicitud de exportación en PDF)

---

## 📊 Impacto

- **Usuarios beneficiados:** Todos los analistas financieros (~50 usuarios activos).
- **Prioridad:** Alta (solicitado por el área de Finanzas, bloquea procesos manuales).
- **Dependencias:** Servicio de generación de reportes, librería Apache POI para Excel.

---

## ✅ Criterios de aceptación

- [ ] El botón “Exportar a Excel” aparece en la vista de reportes.
- [ ] El archivo generado contiene exactamente las columnas visibles en la tabla.
- [ ] El archivo se descarga en formato `.xlsx` válido y puede abrirse en Excel/LibreOffice.
- [ ] Los filtros aplicados en la UI se reflejan en el archivo exportado.
