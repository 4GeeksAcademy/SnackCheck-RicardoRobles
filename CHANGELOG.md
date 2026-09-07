# Changelog

### [0.1.0] - Versión Inicial

Esto es lo que contiene el workflow SnackCheck.

- Un webhook que recibe el código de barra de un producto.
- validaciones para revisar que el código exista y sea numérico.
- Conexión HTTP a la API de Open Food Facts para traer los datos del producto.
- Validación por si el producto tiene datos nutricionales.
- Clasificación de azúcar, sal y grasa en Alto, Medio o Bajo según umbrales definidos por el cliente.
- Combina esos tres resultados junto con el Nutriscore del producto para sacar un veredicto final (Saludable, Moderado o No Saludable).
- Agrega un modelo de IA (Groq) para que redacte el mensaje final que se le devuelve al usuario.
- Agrega mensajes de error para cada validación, con un ejemplo de código de barra válido para que se pueda reintentar.