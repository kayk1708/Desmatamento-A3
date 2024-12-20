import tensorflow as tf
import numpy as np
from tensorflow.keras.models import load_model
from tensorflow.keras.preprocessing.image import img_to_array, load_img

# Carregar modelo treinado para detecção de desmatamento (modelo pré-treinado necessário)
model = load_model('modelo_desmatamento.h5')

# Função para processar a imagem e realizar a predição
def analisar_imagem(imagem_caminho):
    imagem = load_img(imagem_caminho, target_size=(224, 224))  # Ajustar tamanho da imagem
    imagem_array = img_to_array(imagem) / 255.0  # Normalizar os valores da imagem
    imagem_array = np.expand_dims(imagem_array, axis=0)  # Expandir dimensão para predição

    # Realizar predição com o modelo
    predicao = model.predict(imagem_array)
    
    # Interpretação do resultado
    if predicao[0] > 0.5:
        return "Desmatamento identificado."
    else:
        return "Área em condição normal."

# Função para calcular a área desmatada (exemplo simplificado)
def calcular_area_desmatada(imagem_caminho):
    imagem = load_img(imagem_caminho, target_size=(224, 224))  # Carregar imagem no tamanho padrão
    imagem_array = img_to_array(imagem)  # Converter imagem para array

    # Exemplo: calcular proporção de pixels "afetados" (simulação)
    desmatamento_pix = np.sum(imagem_array[:, :, 1] < 100)  # Exemplo com base no canal verde
    total_pix = imagem_array.shape[0] * imagem_array.shape[1]
    proporcao = desmatamento_pix / total_pix

    # Supor que a imagem representa 1 km²
    area_total_km2 = 1.0
    area_desmatada = proporcao * area_total_km2
    return area_desmatada

# Função para sugerir soluções com base no diagnóstico e cálculos
def sugerir_solucao(diagnostico, area_desmatada):
    if diagnostico == "Desmatamento identificado.":
        if area_desmatada > 0.5:
            return [
                "Realizar reflorestamento imediato em grande escala.",
                "Implementar sistemas de monitoramento contínuo na região.",
                "Investir em políticas públicas para reduzir o desmatamento."
            ]
        else:
            return [
                "Promover reflorestamento na área afetada.",
                "Engajar a comunidade local em ações de preservação.",
                "Aplicar sanções para atividades ilegais na área."
            ]
    else:
        return [
            "Manter monitoramento regular para evitar futuros desmatamentos.",
            "Investir em iniciativas de preservação ambiental na região."
        ]

# Exemplo de uso
if _name_ == "_main_":
    caminho_imagem = "imagem_floresta.jpg"  # Substituir pelo caminho da imagem
    diagnostico = analisar_imagem(caminho_imagem)
    print(f"Diagnóstico: {diagnostico}")

    if diagnostico == "Desmatamento identificado.":
        area_desmatada = calcular_area_desmatada(caminho_imagem)
        print(f"Área estimada desmatada: {area_desmatada:.2f} km²")
    else:
        area_desmatada = 0.0

    solucoes = sugerir_solucao(diagnostico, area_desmatada)
    print("\nSoluções sugeridas:")
    for solucao in solucoes:
        print(f"- {solucao}")
