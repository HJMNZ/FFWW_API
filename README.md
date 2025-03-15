from flask import Flask, request, jsonify, render_template
from flask_swagger_ui import get_swaggerui_blueprint

app = Flask(__name__)

# Configuración de Swagger
SWAGGER_URL = "/docs"
API_URL = "/static/swagger.yaml"  # Ruta correcta a swagger.yaml

swagger_ui_blueprint = get_swaggerui_blueprint(SWAGGER_URL, API_URL)
app.register_blueprint(swagger_ui_blueprint, url_prefix=SWAGGER_URL)

# Endpoint raíz
@app.route('/', methods=['GET'])
def raiz():
    return jsonify({"mensaje": "El servidor está funcionando correctamente"})

# Endpoint largo
@app.route('/largo', methods=['POST'])
def largo():
    data = request.json  # Correcto
    texto = data.get('texto')
    return jsonify({"largo": len(texto)})

# Endpoint frase
@app.route('/frase', methods=['GET'])
def frase():
    frase = request.args.get('input')
    return render_template('frase.html', frase=frase)
    
if __name__ == '__main__':
    app.run(debug=True)


-----------------------------------------------------

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Frase Motivadora</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            font-family: Arial, sans-serif;
        }
    </style>
</head>
<body>
    <h1>{{ frase }}</h1>
</body>
</html>

--------------------------------swagger.yaml
openapi: 3.0.0
info:
  title: API de Ejemplo con Flask
  description: API que permite calcular el largo de un texto y renderizar frases en HTML.
  version: 1.0.0

paths:
  /:
    get:
      summary: Verifica que el servidor está funcionando
      description: Devuelve un mensaje de prueba para comprobar el estado del servidor.
      responses:
        "200":
          description: Respuesta exitosa con un mensaje de prueba

  /largo:
    post:
      summary: Calcula la longitud de un texto
      description: Recibe un texto y devuelve su cantidad de caracteres.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                texto:
                  type: string
                  example: "Hola Flask!"
      responses:
        "200":
          description: Devuelve la longitud del texto
          content:
            application/json:
              schema:
                type: object
                properties:
                  longitud:
                    type: integer
                    example: 11

  /frase:
    get:
      summary: Muestra una frase en una página HTML
      description: Renderiza un template HTML con una frase motivadora.
      responses:
        "200":
          description: Página HTML con una frase motivadora
