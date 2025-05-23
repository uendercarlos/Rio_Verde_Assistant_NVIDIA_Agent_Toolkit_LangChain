# 🌾 Rio Verde Assistant

> **Um assistente IA especializado em Rio Verde (GO) usando NVIDIA Agent Toolkit com LangChain**

[![NVIDIA Agent Toolkit](https://img.shields.io/badge/NVIDIA-Agent%20Toolkit-green?style=flat&logo=nvidia)](https://github.com/NVIDIA/AIQToolkit)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-blue?style=flat&logo=python)](https://python.org)
[![LangChain](https://img.shields.io/badge/LangChain-ReAct%20Agent-orange?style=flat)](https://langchain.com)
[![Status](https://img.shields.io/badge/Status-✅%20Funcionando-brightgreen?style=flat)](#)

---

## 🎯 **Objetivo**

Criar um agente inteligente que responde perguntas sobre **Rio Verde (GO)** - a "Capital do Agronegócio" do Brasil - usando:
- **NVIDIA Agent Toolkit** 
- **LangChain ReAct Agent**
- **Llama 3.1** via NVIDIA NIM

---

## 📋 **Pré-requisitos**

- WSL com Ubuntu instalado
- Python 3.11 ou superior
- Conta NVIDIA com API Key → [build.nvidia.com](https://build.nvidia.com/)

---

## 🚀 **Instalação Passo a Passo**

### **1. Clone o Repositório**
```bash
cd ~
git clone https://github.com/NVIDIA/AIQToolkit.git
cd AIQToolkit
```

### **2. Verifique/Instale Python 3.11**
```bash
# Verifique versão atual
python3 --version

# Se for menor que 3.11, instale:
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.11 python3.11-venv python3.11-dev

# Confirme instalação
python3.11 --version
```

### **3. Crie e Ative Ambiente Virtual**
```bash
# Crie ambiente virtual
python3.11 -m venv venv

# Ative (deve aparecer "(venv)" no prompt)
source venv/bin/activate
```

### **4. Instale AIQ Toolkit Base**
```bash
# Atualize pip
pip install --upgrade pip

# Instale toolkit base
pip install -e ".[agents]"
```

### **5. 🔥 IMPORTANTE: Instale Plugin LangChain**
```bash
# Esta é a parte crucial!
pip install aiqtoolkit-langchain

# Instale dependências necessárias
pip install langgraph langchain langchain-community
```

### **6. Configure API Key NVIDIA**
```bash
# Substitua pela sua chave real
export NVIDIA_API_KEY="sua-api-key-aqui"

# Verifique se foi configurada
echo $NVIDIA_API_KEY
```

### **7. Navegue para Diretório de Exemplos**
```bash
cd examples/simple
```

### **8. Crie Estrutura de Configuração**
```bash
# Crie diretório
mkdir -p configs

# Crie arquivo de configuração otimizado
nano configs/rio_verde_otimizado.yml
```

### **9. Configuração YAML Otimizada**

Cole o conteúdo abaixo no arquivo `configs/rio_verde_otimizado.yml`:

```yaml
functions:
  current_datetime:
    type: current_datetime

llms:
  nim_llm:
    type: nim
    model_name: meta/llama-3.1-70b-instruct
    temperature: 0.1
    max_tokens: 1024

workflow:
  type: react_agent
  tool_names: [current_datetime]
  llm_name: nim_llm
  verbose: true
  retry_parsing_errors: false
  max_retries: 1
  system_prompt: |
    Você é um assistente especialista em Rio Verde, Goiás, Brasil.
    Rio Verde é conhecida como a "Capital do Agronegócio" do Brasil!
    
    INFORMAÇÕES PRINCIPAIS:
    - População: ~230.000 habitantes
    - Localização: 229km de Goiânia, Sudoeste de Goiás
    - Maior produtor de grãos do estado
    - Sede da TECNOSHOW (maior feira do agronegócio)
    - PIB forte baseado em agricultura e pecuária
    - Universidades: UnirV, IFG
    - Clima tropical semi-úmido
    
    INSTRUÇÕES:
    1. Para perguntas sobre Rio Verde: responda diretamente com seu conhecimento
    2. Para perguntas sobre data/hora: use a ferramenta current_datetime
    3. Seja conciso e direto
    4. SEMPRE termine com "Final Answer: [sua resposta]"
    
    Ferramentas disponíveis: {tool_names}
    {tools}
```

**Comandos para salvar:**
- `Ctrl+O` → salvar
- `Enter` → confirmar
- `Ctrl+X` → sair

### **10. 🎉 Teste o Assistente**
```bash
aiq run --config_file configs/rio_verde_otimizado.yml --input "Fale sobre Rio Verde"
```

---

## ✅ **Exemplo de Resultado Esperado**

```
Configuration Summary:
--------------------
Workflow Type: react_agent
Number of Functions: 1
Number of LLMs: 1

[AGENT]
Agent input: Fale sobre Rio Verde
Agent's thoughts:
Rio Verde é uma cidade localizada no sudoeste do estado de Goiás, Brasil. 
É conhecida como a "Capital do Agronegócio" do Brasil devido à sua forte 
produção agrícola e pecuária...

Final Answer: Rio Verde é uma cidade importante para o agronegócio brasileiro, 
conhecida por sua produção agrícola e pecuária, e sede de importantes 
instituições de ensino superior.

Workflow Result:
['Rio Verde é uma cidade importante para o agronegócio brasileiro...']
```

---

## 🧪 **Comandos de Teste**

```bash
# Teste produtos agrícolas
aiq run --config_file configs/rio_verde_otimizado.yml --input "Quais produtos Rio Verde produz?"

# Teste economia
aiq run --config_file configs/rio_verde_otimizado.yml --input "Como está a economia de Rio Verde?"

# Teste data/hora (usa ferramenta current_datetime)
aiq run --config_file configs/rio_verde_otimizado.yml --input "Que horas são agora?"

# Teste educação
aiq run --config_file configs/rio_verde_otimizado.yml --input "Quais universidades tem em Rio Verde?"
```

---

## 🚨 **Pontos Cruciais**

### ✅ **O que é ESSENCIAL:**
1. **Python 3.11+** - Toolkit não funciona com versões antigas
2. **Plugin LangChain** - `pip install aiqtoolkit-langchain`
3. **Dependências LangChain** - `langgraph`, `langchain`, `langchain-community`
4. **API Key NVIDIA** - Configurada como variável de ambiente
5. **Ambiente Virtual Ativado** - Sempre ver `(venv)` no prompt
6. **Configuração Otimizada** - `retry_parsing_errors: false` e `max_retries: 1`
7. **Final Answer** - Instrução clara no prompt para formato correto

### ❌ **Erros Comuns:**
- **"react_agent not found"** → Faltou instalar `aiqtoolkit-langchain`
- **"langgraph module not found"** → Faltou instalar `langgraph`
- **Python version error** → Precisa Python 3.11+
- **API Key error** → Não configurou `NVIDIA_API_KEY`
- **Loop infinito** → Use configuração otimizada com `retry_parsing_errors: false`

---

## 📁 **Estrutura do Projeto**

```
~/AIQToolkit/
├── examples/
│   └── simple/
│       ├── configs/
│       │   └── rio_verde_otimizado.yml  ← Arquivo principal
│       └── ...
├── venv/
└── ...
```

---

## 🔧 **Troubleshooting**

### **Erro: comando 'aiq' não encontrado**
```bash
# Verifique se está no ambiente virtual
which python
# Deve mostrar: /home/user/AIQToolkit/venv/bin/python

# Reinstale se necessário
pip install -e ".[agents]"
```

### **Erro: módulo langgraph**
```bash
pip install langgraph langchain langchain-community
```

### **Loop infinito de respostas**
```bash
# Use a configuração otimizada com:
# retry_parsing_errors: false
# max_retries: 1
```

---

## ✨ **Diferenciais da Configuração Otimizada**

| Configuração | Valor | Motivo |
|--------------|-------|--------|
| `retry_parsing_errors` | `false` | Evita loops infinitos |
| `max_retries` | `1` | Máximo 1 tentativa |
| `system_prompt` | Estruturado | Instruções claras |
| `Final Answer:` | Obrigatório | Formato correto |

---

## 📊 **Exemplos de Respostas**

### **Pergunta:** "Quais produtos Rio Verde produz?"
**Resposta:** "Soja, milho, trigo, carne bovina e leite."

### **Pergunta:** "Fale sobre Rio Verde"
**Resposta:** "Rio Verde é uma cidade importante para o agronegócio brasileiro, conhecida por sua produção agrícola e pecuária, e sede de importantes instituições de ensino superior."

---

## 🚀 **Próximos Passos**

- [ ] **Busca web em tempo real** - Integrar APIs de notícias
- [ ] **Interface web** - Dashboard com FastAPI
- [ ] **RAG personalizado** - Base de conhecimento local
- [ ] **Memória de conversação** - Contexto entre perguntas
- [ ] **Deploy em produção** - Container Docker

---

## 🏆 **Hackathon NVIDIA 2025**

**Principais diferenciais do projeto:**
- ✅ **Funciona perfeitamente** - Zero loops, respostas limpas
- ✅ **Foco regional específico** - Especialista em Rio Verde
- ✅ **Configuração otimizada** - Sem problemas de parsing
- ✅ **Tutorial reproduzível** - Passo a passo testado
- ✅ **Troubleshooting completo** - Soluções para problemas comuns
- ✅ **Respostas precisas** - Conhecimento sobre agronegócio

---

## 🤝 **Contribuições**

Contribuições são bem-vindas! 

1. Fork o projeto
2. Crie uma branch (`git checkout -b feature/nova-funcionalidade`)
3. Commit suas mudanças (`git commit -am 'Adiciona nova funcionalidade'`)
4. Push para a branch (`git push origin feature/nova-funcionalidade`)
5. Abra um Pull Request

---

## 📄 **Licença**

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

## 👨‍💻 **Autor**

**Uender e Cristina**
- LinkedIn: [Seu Perfil]([https://linkedin.com/in/seu-perfil](https://www.linkedin.com/in/uender-carlos/))
- Email: u.carlos3@gmail.com

---

## 🌟 **Agradecimentos**

- **NVIDIA** pelo Agent Toolkit e infraestrutura NIM
- **LangChain** pela integração de agentes
- **Comunidade Open Source** pelas contribuições

---

<div align="center">

**🌾 Rio Verde - Capital do Agronegócio encontra a IA de ponta! 🚀**

[![Made with ❤️](https://img.shields.io/badge/Made%20with-❤️-red.svg)](#)
[![NVIDIA Hackathon 2025](https://img.shields.io/badge/NVIDIA-Hackathon%202025-green.svg)](#)

</div>
