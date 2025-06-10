# Detecção de Bovinos com YOLO11n

Este projeto utiliza o modelo YOLO11n (You Only Look Once) para detectar e rastrear bovinos em vídeos, gerando relatórios de contagem por frame.

## 📋 Descrição

O sistema processa vídeos de bovinos e realiza:
- Detecção automática de bovinos usando YOLO11n
- Rastreamento temporal dos animais
- Contagem de bovinos por frame
- Geração de relatório CSV com estatísticas
- Identificação do número máximo de bovinos detectados

## 🚀 Tecnologias Utilizadas

- **Python 3.x**
- **OpenCV (cv2)** - Processamento de imagens e vídeos
- **Pandas** - Manipulação de dados
- **Ultralytics YOLO** - Modelo de detecção de objetos
- **CSV** - Geração de relatórios

## 📦 Instalação

### Pré-requisitos
```bash
pip install ultralytics
pip install opencv-python
pip install pandas
```

### Configuração
1. Clone ou baixe o arquivo `detecção_de_bovinos.py`
2. Certifique-se de ter o vídeo de entrada (exemplo: `vacas.mp4`)
3. Execute o script

## 🎯 Como Usar

### 1. Preparação do Modelo
O script inicializa e treina o modelo YOLO11n:
```python
model = YOLO("yolo11n.pt")
# Treinamento com dataset COCO8 por 115 épocas
results = model.train(data="coco8.yaml", epochs=115, imgsz=640)
```

### 2. Processamento do Vídeo
```python
# Processa o vídeo com rastreamento
results = model.track(source="/content/vacas.mp4", 
                     show=False, 
                     save=True, 
                     name='test_result', 
                     persist=True, 
                     classes=19)  # Classe 19 = bovinos
```

### 3. Análise dos Resultados
O sistema extrai informações de cada detecção:
- **Coordenadas das caixas delimitadoras** (xywh, xyxy)
- **Nomes das classes detectadas**
- **Scores de confiança**
- **Contagem por frame**

## 📊 Saídas Geradas

### Arquivo CSV
O script gera `contagem_vacas.csv` com:
- **Frame**: Número do frame no vídeo
- **Numero de Vacas**: Quantidade de bovinos detectados

### Relatório Final
- Número máximo de bovinos detectados simultaneamente
- Vídeo processado salvo com detecções

## 🔧 Configurações Principais

| Parâmetro | Valor | Descrição |
|-----------|-------|-----------|
| `epochs` | 115 | Número de épocas de treinamento |
| `imgsz` | 640 | Tamanho da imagem para processamento |
| `classes` | 19 | Classe COCO para bovinos |
| `persist` | True | Mantém rastreamento entre frames |

## 📈 Métricas de Avaliação

O modelo fornece métricas de precisão:
- **mAP (mean Average Precision)** - Precisão média geral
- **mAP@0.5** - Precisão com IoU threshold 0.5
- **mAP@0.75** - Precisão com IoU threshold 0.75
- **mAPs** - Precisões por classe

## 🎥 Exemplo de Uso

```python
# Carregar modelo treinado
model = YOLO("/content/runs/detect/train/weights/best.pt")

# Processar vídeo
results = model.track(source="seu_video.mp4", classes=19)

# Analisar resultados
for result in results:
    num_vacas = len(result.boxes) if result.boxes else 0
    print(f"Vacas detectadas: {num_vacas}")
```

## 📁 Estrutura de Arquivos

```
projeto/
├── detecção_de_bovinos.py    # Script principal
├── vacas.mp4                 # Vídeo de entrada
├── contagem_vacas.csv        # Relatório de saída
├── runs/                     # Resultados do treinamento
│   └── detect/
│       └── train/
│           └── weights/
│               └── best.pt   # Modelo treinado
└── test_result/              # Vídeo processado
```

## 🐄 Classes Detectadas

O modelo utiliza a classe 19 do dataset COCO, que corresponde a:
- **Bovinos** (vacas, touros, bezerros)

## ⚠️ Observações Importantes

1. **Vídeo de Entrada**: Certifique-se de que o caminho do vídeo está correto
2. **Recursos Computacionais**: O treinamento pode ser intensivo, considere usar GPU
3. **Qualidade do Vídeo**: Vídeos com melhor qualidade resultam em detecções mais precisas
4. **Iluminação**: Condições de iluminação adequadas melhoram a performance

## 🔍 Troubleshooting

### Problemas Comuns
- **Erro de caminho**: Verifique se o vídeo existe no caminho especificado
- **Memória insuficiente**: Reduza o tamanho da imagem (`imgsz`)
- **Modelo não encontrado**: Certifique-se de que o YOLO11n foi baixado corretamente

## 🤝 Contribuições

Contribuições são bem-vindas! Áreas para melhorias:
- Otimização de hiperparâmetros
- Implementação de outras classes de animais
- Interface gráfica para visualização
- Análise temporal avançada

## 📄 Licença

Este projeto utiliza a licença da Ultralytics YOLO. Consulte a documentação oficial para mais detalhes.

## 📚 Referências

- [Ultralytics YOLO](https://github.com/ultralytics/ultralytics)
- [COCO Dataset](https://cocodataset.org/)
- [OpenCV Documentation](https://opencv.org/)

---

*Desenvolvido para detecção e monitoramento de bovinos usando técnicas de visão computacional e deep learning.*
