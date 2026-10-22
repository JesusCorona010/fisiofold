# FisioFold 📘

Tienda en línea de **FisioFold**, libros de bolsillo de anatomía para estudiantes de Fisioterapia: miología, osteología, artrología y citología en formato de 9 × 12 cm, con tapas duras y espiral.

🔗 **Demo:** https://TU-USUARIO.github.io/fisiofold/

---

## ✨ Características

- **Catálogo con filtros** por materia (Miología, Osteología, Artrología, Citología).
- **Visor de imágenes**: al tocar un libro se abre en grande, con botón para agregarlo al carrito.
- **Carrito de compras** con cantidades, barra de progreso para envío gratis y guardado automático en el navegador.
- **Checkout** con validación de datos, opción de recoger en campus o envío a domicilio, y forma de pago.
- **Pedidos por WhatsApp**: al confirmar, se abre WhatsApp con el pedido completo (libros, total, regalo, datos del cliente y entrega).
- **Regalo de lanzamiento** (FisioFold Match) incluido automáticamente en el resumen y en el mensaje del pedido.
- **Diseño responsivo**: catálogo en 2 columnas, botón flotante del carrito y checkout tipo *bottom sheet* en celular.
- **Accesible**: navegación con teclado, etiquetas ARIA y respeto a la preferencia de movimiento reducido.
- **Un solo archivo**: HTML, CSS, JavaScript e imágenes (WebP en base64) en `index.html`, sin dependencias ni servidor.

## 🛠️ Tecnologías

- HTML5 (con `<dialog>` nativo)
- CSS3 (variables, Grid, Flexbox, media queries, `env(safe-area-inset-*)`)
- JavaScript vanilla
- Fuentes: Sora e Inter (Google Fonts)

## 📁 Estructura

```
fisiofold/
├── index.html   # Toda la tienda: estructura, estilos, lógica e imágenes
└── README.md
```

## ⚙️ Configuración

Toda la configuración está al inicio del segundo `<script>` de `index.html`, en el bloque **CONFIGURACIÓN DE LA TIENDA**.

```js
const WHATSAPP = '52XXXXXXXXXX';   // 52 + 10 dígitos, sin espacios
const SHIPPING = 80;               // Costo de envío a domicilio (MXN)
const FREE_SHIPPING_FROM = 500;    // Envío gratis desde este subtotal (0 = desactivado)
const GIFT = 'FisioFold Match (memorama anatómico)'; // '' para quitar el regalo
```

### Editar o agregar un libro

Cada libro es un objeto dentro del arreglo `PRODUCTS`:

```js
{ id: 'hombro', subject: 'Miología', title: 'Hombro y brazo',
  desc: 'Todos los músculos del hombro y el brazo, con sus movimientos.',
  price: 149, category: 'Miología' },
```

| Campo      | Descripción                                                        |
|------------|--------------------------------------------------------------------|
| `id`       | Identificador único. También es la clave de su imagen en `IMAGES`. |
| `subject`  | Materia que se muestra arriba del título.                          |
| `title`    | Nombre del libro.                                                  |
| `desc`     | Descripción corta.                                                 |
| `price`    | Precio en MXN.                                                     |
| `category` | Filtro donde aparece. Los filtros se generan solos.                |
| `badge`    | *(Opcional)* Etiqueta sobre la foto, por ejemplo `'Nuevo'`.        |
| `oldPrice` | *(Opcional)* Precio anterior, se muestra tachado.                  |
| `image`    | *(Opcional)* Clave de imagen distinta al `id`.                     |

### Agregar la imagen de un libro nuevo

Las imágenes viven en el objeto `IMAGES` (primer `<script>`), como data URIs en WebP:

```js
const IMAGES = {
  hombro: { img: 'data:image/webp;base64,...', thumb: 'data:image/webp;base64,...' },
  ...
};
```

- `img`: foto del producto, se recomienda ~760 px de ancho.
- `thumb`: miniatura cuadrada para el carrito, ~160 px.

Para convertir una imagen puedes usar este script de Python (requiere `pip install pillow`):

```python
from PIL import Image
import base64, io

def to_data_uri(path, width, quality=74):
    im = Image.open(path).convert('RGB')
    im.thumbnail((width, 10000))
    buf = io.BytesIO()
    im.save(buf, 'WEBP', quality=quality)
    return 'data:image/webp;base64,' + base64.b64encode(buf.getvalue()).decode()

print(to_data_uri('mi-libro.png', 760))   # img
print(to_data_uri('mi-libro.png', 160))   # thumb
```

Después agrega la entrada en `IMAGES` con el mismo `id` del libro.

## 🛒 Flujo de compra

1. El cliente agrega libros al carrito.
2. En el checkout llena nombre, correo, WhatsApp, tipo de entrega y forma de pago.
3. Se abre WhatsApp con el pedido listo para enviar.
4. FisioFold responde con un link de Mercado Pago por el total exacto (tarjeta u OXXO) o con los datos para transferencia.

> No hay cobro automático: el pago se gestiona por WhatsApp. Para cobro directo en la página haría falta integrar Mercado Pago Checkout Pro o Stripe con un pequeño backend.

## 🚀 Despliegue

### GitHub Pages

1. Sube el repositorio a GitHub.
2. Ve a **Settings → Pages**.
3. En *Source* elige la rama `main` y la carpeta `/ (root)`.
4. En uno o dos minutos estará en `https://TU-USUARIO.github.io/fisiofold/`.

### Netlify Drop

Arrastra la carpeta del proyecto a [app.netlify.com/drop](https://app.netlify.com/drop) y listo.

## 💻 Desarrollo local

No necesita instalación. Abre `index.html` en el navegador, o usa un servidor local:

```bash
npx serve .
```

Para probar la vista móvil en Chrome: `F12` → `Ctrl + Shift + M`.

## 📄 Licencia

© 2026 FisioFold. Todos los derechos reservados. Las imágenes y el contenido de los libros son propiedad de FisioFold.
