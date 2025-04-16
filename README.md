# Projeto: AgroMonitor
# Descrição: Sistema Open Source de Monitoramento de Dados Agrícolas

# Estrutura inicial do backend (app.py usando Flask)

from flask import Flask, request, jsonify
import csv
import os

app = Flask(__name__)

data_file = 'data/exemplo.csv'

@app.route('/')
def home():
    return "AgroMonitor API - Monitoramento de Dados Agrícolas"

@app.route('/dados', methods=['GET'])
def listar_dados():
    if not os.path.exists(data_file):
        return jsonify([])
    with open(data_file, newline='') as csvfile:
        reader = csv.DictReader(csvfile)
        return jsonify(list(reader))

@app.route('/dados', methods=['POST'])
def adicionar_dado():
    novo_dado = request.json
    file_exists = os.path.isfile(data_file)
    with open(data_file, 'a', newline='') as csvfile:
        fieldnames = ['data', 'temperatura', 'umidade', 'chuva']
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
        if not file_exists:
            writer.writeheader()
        writer.writerow(novo_dado)
    return jsonify({"status": "sucesso", "dado": novo_dado}), 201

if __name__ == '__main__':
    app.run(debug=True)
