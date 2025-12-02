# LectorDeFacturas-N8N
# Sub Reto – Lectura Inteligente de Facturas (Cobros GdO)

Proyecto desarrollado en el marco del **PRE Reto “Lectura Inteligente de Planos Técnicos”**, como aproximación al uso de IA para la lectura automática de documentos técnicos y administrativos.

El objetivo del Sub Reto es automatizar la lectura de **facturas de servicios públicos en PDF**, utilizando un flujo orquestado en **n8n** que integra servicios de Google y modelos de IA.

---

## 🎯 Objetivo de la solución

Diseñar e implementar un flujo automatizado que:

- Reciba facturas en formato PDF enviadas por correo.
- Convierta el PDF a imágenes por página.
- Utilice **Gemini 2.5 Flash** para extraer la información clave.
- Almacene los resultados en **Google Sheets** de forma estructurada.

---

## 🧩 Tecnologías utilizadas

- [n8n](https://n8n.io/) – Orquestación del flujo
- Gmail API – Lectura de correos con adjuntos PDF
- Google Drive API – Almacenamiento de archivos
- Google Sheets API – Registro estructurado de datos
- [PDF.co](https://pdf.co/) – Conversión PDF → imagen (PNG)
- Gemini **2.5 Flash** – IA para análisis de las imágenes (facturas)

---

## ⚙️ Funcionamiento del flujo

1. El usuario envía un correo con un archivo PDF adjunto a:  
   **`agentefacturacio001@gmail.com`**

2. Desde n8n se ejecuta **manualmente** el workflow de procesamiento.

3. El flujo:
   - Lee el correo más reciente no leído con adjunto PDF.
   - Descarga el archivo y lo sube a una carpeta de Google Drive (*Facturas Entrantes*).

4. Se envía el PDF a **PDF.co**, que:
   - Separa el documento por páginas.
   - Genera una URL de imagen (PNG) para cada página.

5. Cada imagen es analizada por **Gemini 2.5 Flash**, usando un prompt diseñado para extraer:
   - `numero_contrato`
   - `direccion`
   - `codigo_referencia`
   - `total_pagar`
   - `empresa`
   - `periodo_facturado`
   - `fecha_vencimiento`

6. Un nodo de **Code** en n8n:
   - Limpia el texto devuelto por la IA.
   - Extrae únicamente el JSON válido.
   - Normaliza campos y tipos (números sin símbolos, strings, etc.).

7. El flujo crea un **Google Sheets** con el nombre del PDF original y agrega una fila por cada factura identificada.

8. Finalmente:
   - El PDF se mueve a una carpeta `Procesadas` en Google Drive.
   - El correo se marca como **leído** para evitar reprocesarlo.

---

## 📂 Estructura del repositorio

```text
/Reto_Lectura_Inteligente_de_Facturas

├── 00_Ficha_Reto.pdf
│
├── 01_Prototipo/
│   ├── prompts/
│   ├── flujo_datos.png
│
├── 02_Validacion/
│   ├── resultados.xlsx
│   ├── evaluacion.pdf
│
├── 03_Entrega_Final/
│   ├── informe_final.pdf
│   ├── presentacion.pptx
│   ├── demo.mp4
│
└── README.md
````

---

## 📊 Resultados de la experimentación

- PDF de prueba: 10 páginas (10 facturas).
- Tiempo de ejecución aproximado: 2 min 10 s.
- Extracción exitosa de los campos clave definidos en el prompt.
- Consolidación automática en Google Sheets con un archivo por cada PDF procesado.

---

## 🚧 Limitaciones conocidas

- Dependencia de la calidad del escaneo (facturas muy borrosas pueden fallar).
- El flujo está diseñado para facturas de servicios públicos con estructura similar.
- Incremento del tiempo de procesamiento al aumentar el número de páginas.

---

## 📌 Próximos pasos

- Generalizar el flujo para nuevos tipos de facturas y documentos.
- Agregar validaciones y controles de calidad sobre los datos extraídos.
- Integrar un dashboard de consulta (por ejemplo, usando Looker Studio o similar).
- Extender la solución hacia el reto original de lectura de planos técnicos.

---
