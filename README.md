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
