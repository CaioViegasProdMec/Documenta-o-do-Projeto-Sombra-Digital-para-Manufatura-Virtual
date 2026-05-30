# Sombra-Digital-para-Manufatura-Virtual
Sombra Digital com Visão Computacional para Rastreamento de Objetos no Unity

# Nome do Projeto

> Uma breve descrição do seu projeto (1-2 frases)

## 📋 Índice
- [Sobre](#sobre)
- [Arquitetura](#arquitetura)
- [Tecnologias](#tecnologias)
- [Pré-requisitos](#pré-requisitos)
- [Instalação](#instalação)
- [Como Usar](#como-usar)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Resultados](#resultados)
- [Autor](#autor)
- [Licença](#licença)

## 🎯 Sobre o Projeto

Este projeto implementa uma **Sombra Digital (Digital Shadow)** para manufatura virtual no Unity 3D.

**Objetivo:** Rastrear um objeto "real" (cubo vermelho) em uma fábrica simulada e replicar sua posição em tempo real em uma representação virtual.

**Fluxo de dados:**
Unity (câmera) → Frames → Python/YOLO → Coordenadas (UDP) → ShadowCube

**Contexto:** Trabalho de Conclusão de Curso (TCC) em Engenharia de Produção Mecânica na Universidade Federal de Santa Catarina.

## 🏗️ Arquitetura do Sistema

┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Unity (Real)  │ ──→ │  Python (YOLO)  │ ──→ │  Unity (Shadow) │
│   Fábrica 3D    │     │  Processamento  │     │   Sombra       │
└─────────────────┘     └─────────────────┘     └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
    Salva frames           Detecta cubo            Move shadow
    na pasta               Envia UDP               em tempo real

## 💻 Tecnologias

| Tecnologia | Versão | Finalidade |
| :--- | :--- | :--- |
| Unity | 2022.3 LTS | Motor gráfico e simulação |
| C# | .NET Standard 2.1 | Scripts Unity |
| Python | 3.10+ | Processamento de imagem |
| YOLOv8 | Ultralytics | Detecção de objetos |
| OpenCV | 4.13.0 | Segmentação por cor |

## ⚙️ Pré-requisitos

### Hardware
- Processador: Intel i5 ou superior
- RAM: 16GB (mínimo 8GB)
- GPU: Recomendado (para YOLO)

### Software
- Windows 10/11 (ou Linux/macOS)
- Unity Hub + Unity 2022.3 LTS
- Python 3.10 ou superior
- Git (opcional)

## 📦 Instalação

### 1. Clone o repositório
\\\bash
git clone https://github.com/seu-usuario/sombra-digital.git
cd sombra-digital
\\\

### 2. Instale as dependências Python
```
pip install ultralytics opencv-python numpy
```

### 3. Abra o projeto Unity
- Abra o Unity Hub
- Clique em "Open Project"
- Selecione a pasta do projeto

### 4. Configure as pastas
```
# Crie a pasta compartilhada (se não existir)
mkdir SharedFrames
```

## 🚀 Como Usar

### Passo 1: Gerar o dataset
1. Abra a cena `Factory_Dataset.unity`
2. Anexe `FrameCapture.cs` à câmera
3. Aperte Play e mova o cubo pela esteira
4. As imagens serão salvas em `dataset/images/`

### Passo 2: Anotar as imagens
```
python calibrar_hsv.py   # Ajuste os valores HSV
python anotar.py         # Gera as bounding boxes
python verificar.py      # Verifique o resultado
```

### Passo 3: Treinar o YOLO
```
yolo task=detect mode=train model=yolov8n.pt data=data.yaml epochs=50
```

### Passo 4: Executar o sistema completo
```
# Terminal 1 (Python)
python yolo_processor.py

# Terminal 2 (Unity)
# Aperte Play no Unity
```
## 📁 Estrutura do Projeto

```
📁 sombra-digital/
│
├── 📁 Assets/
│   ├── 📁 Scripts/
│   │   ├── FrameCapture.cs
│   │   ├── FrameExporter.cs
│   │   ├── ShadowReceiver.cs
│   │   └── UnityMainThreadDispatcher.cs
│   ├── 📁 Scenes/
│   │   └── Factory_Main.unity
│   └── 📁 Materials/
│
├── 📁 SharedFrames/              # Pasta compartilhada
│   └── current_frame.png
│
├── 📁 Python/
│   ├── calibrar_hsv.py
│   ├── anotar.py
│   ├── verificar.py
│   └── yolo_processor.py
│
├── 📁 Dataset/                   # Dataset gerado
│   ├── images/
│   ├── labels/
│   └── data.yaml
│
└── 📄 README.md
```
