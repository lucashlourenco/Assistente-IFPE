# Assistente Virtual IFPE

Este projeto consiste em um assistente virtual com interface web animada em 3D e reconhecimento de voz, integrado a um backend de inteligência artificial desenvolvido com Rasa Framework. O sistema pode ser executado em um computador pessoal ou em dispositivos embarcados como o Raspberry Pi.

## Repositórios

1. Backend: https://github.com/lucashlourenco/Assistente-IFPE
2. Frontend: https://github.com/Paulo-Fidelis/rasa_final

## 📋 Pré-requisitos Gerais

Para executar este projeto, você precisará de:

1.  **Python 3.9** (Versão recomendada para compatibilidade com as dependências do projeto).
2.  **Navegador Web Moderno** (Chrome, Edge, Firefox, etc) com suporte a WebGL e permissão de microfone.
3.  **Conexão com a Internet** (O frontend carrega bibliotecas como Three.js, FontAwesome e VLibras via CDN).

---

## 🖥️ Opção 1: Execução em PC (Windows/Linux/Mac)

### 1. Configuração do Backend (Rasa)

1.  Abra o terminal na pasta raiz do projeto (onde está o arquivo `requirements.txt`).
2.  (Recomendado) Crie e ative um ambiente virtual:
    * **Windows:**
        ```bash
        python -m venv venv
        .\venv\Scripts\activate
        ```
    * **Linux/Mac:**
        ```bash
        python3 -m venv venv
        source venv/bin/activate
        ```
3.  Instale as dependências:
    ```bash
    pip install -r requirements.txt
    ```
4.  Treine o modelo de Inteligência Artificial:
    ```bash
    rasa train
    ```
5.  **Inicie o servidor Rasa** (Mantenha este terminal aberto):
    ```bash
    rasa run --enable-api --cors "*"
    ```
    *Aguarde a mensagem: `Rasa server is up and running.`*

### 2. Execução do Frontend

1.  Certifique-se de que o arquivo `index.html` e o modelo `lucas_assistente.glb` estão na mesma pasta.
2.  Abra um **segundo terminal** na pasta do frontend.
3.  Inicie um servidor HTTP local (necessário para carregar o modelo 3D sem bloqueio de CORS):
    ```bash
    python -m http.server 8000
    ```
4.  Acesse no navegador: [http://localhost:8000](http://localhost:8000).

---

## 🍓 Opção 2: Execução no Raspberry Pi

Esta configuração é ideal para criar um quiosque ou totem de atendimento.

### Hardware Recomendado
* **Raspberry Pi 4 (4GB+)** ou **Raspberry Pi 5**.
* Cartão MicroSD (32GB+) com **Raspberry Pi OS (64-bit)**.
* Microfone USB e Caixas de Som.

### 1. Preparação do Sistema (No Raspberry Pi)

Abra o terminal do Pi e instale as dependências de sistema para áudio e compilação:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3-dev python3-pip python3-venv libatlas-base-dev libasound2-dev portaudio19-dev
```

### Configuração
1. Copie projeto para Pi.  
2. `cd Assistente-IFPE`, crie venv: `python3 -m venv venv && source venv/bin/activate` 
3. `pip install --upgrade pip && pip install -r requirements.txt` 

### Treinamento (Estratégia)
Treine no PC (`rasa train`), copie `models/*.tar.gz` para Pi (evita lentidão). 

### Início Backend
```bash
rasa run --enable-api --cors --host 0.0.0.0
```

### Frontend (Acesso Remoto)
1. IP do Pi: `hostname -I` (ex: 192.168.1.15). 
2. Edite `index.html`: mude `localhost:5005` para `192.168.1.15:5005`. 
3. `python3 -m http.server 8000` 
4. Acesse: http://192.168.1.15:8000. 

**Nota Microfone**: Use HTTPS ou localhost para voz remota; ngrok ou navegador no Pi. 

## 🤖 Como Utilizar

- **Texto**: Digite e Enter. 
- **Voz**: Clique microfone, fale (preenche automático). 
- **Áudio**: Botão laranja Pause/Play. 
- **Libras**: Widget lateral para LIBRAS. 

## 🛠️ Solução de Problemas

| Problema                  | Solução                                                                 |
|---------------------------|-------------------------------------------------------------------------|
| Avatar não aparece       | Verifique `lucasassistente.glb` e use `http.server` (não abra HTML direto).  |
| "Processando..." infinito| Confirme Rasa rodando com `--cors`; IP correto no Pi.             |
| Erro microfone           | Permissão no navegador; HTTPS para remoto (exceto localhost).     |
| Rasa não instala no Pi   | Use Raspberry Pi OS 64-bit (32-bit falha no TensorFlow).          | 
