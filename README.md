# SnackCheck-RicardoRobles
Primer Proyecto del Boot Camp IA Engeneering - entrega Ricardo Robles

Este es un workflow que hecho en n8n como práctica. La idea es simple: le pasas el código de barra de un snack y te dice si es saludable, moderado o no saludable.

### ¿Qué hace?
1. Recibe el código de barra por un webhook (POST).
2. Valida que el código exista y que sea un número.
3. Busca el producto en la API de Open Food Facts.
4. Revisa si el producto trae información nutricional.
5. Clasifica el azúcar, la sal y la grasa del producto en Alto, Medio o Bajo (según unos umbrales definidos).
6. Junta esos tres resultados y también compara contra el Nutriscore que trae el producto (si tiene).
7. Con todo eso arma un veredicto final: Saludable, Moderado o No Saludable, quedándose siempre con el peor de los dos resultados.
8. Al final le pide a un modelo de IA que redacte un mensaje explicando el resultado, y esa es la respuesta que se devuelve.
Cómo se usa

### Mandar una petición POST así:
POST [/webhook/nutrition-check](https://ricardorobles.app.n8n.cloud/webhook-test/nutrition-check)
{
  "barcode": "3017620422003"
}

### ¿Cómo se clasifica cada nutriente?

Umbrales por cada 100g de producto:

- Azúcar: más de 22.5g es Alto, entre 5g y 22.5g es Medio, menos de 5g es Bajo.
- Sal: más de 1.5g es Alto, el resto es Bajo.
- Grasa: más de 17.5g es Alto, el resto es Bajo.

Si dos o más nutrientes salen Alto, el resultado es No Saludable. Si sale solo uno Alto, es Moderado. Si ninguno es Alto, es Saludable.

### Manejo de errores
Si algo sale mal en el camino (falta el código, no es número, no se encuentra el producto, o no hay datos nutricionales suficientes), el flujo corta ahí mismo y devuelve un mensaje explicando qué pasó, junto con un ejemplo de código de barra válido para volver a intentar.

### ¿De dónde salen los datos?
Los datos del producto vienen de la API de Open Food Facts, que es gratis y no necesita autenticación.
El mensaje final lo escribe un modelo de IA (Groq), pero solo para redactar el texto, no participa en decidir si el producto es saludable o no.

### Cosas que podría mejorar más adelante
Que cada nutriente pese distinto en el puntaje final (ahora todos suman lo mismo y podrían agregar una capa más compleja a esa validación).
Guardar resultados ya consultados para no llamar la API de nuevo con el mismo código de barra.