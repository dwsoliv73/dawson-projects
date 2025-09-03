# Portfólio - Dawson Monteiro ☁️👨‍💻

Estudante de Ciência da Computação (UFF), com certificação AWS Cloud Practitioner e IBM Data Science for Bussiness. Tenho experiência prática em desenvolvimento de APIs, NLP, Docker e computação em nuvem. Apaixonado por Inteligência Artificial, segurança da informação e soluções escaláveis.

---

## 🚀 Projetos em Destaque

### 1. 🧠 Chatbot Jurídico com RAG e AWS
Solução serverless com LangChain/Bedrock para consultas jurídicas via Telegram, processando PDFs com embeddings ChromaDB.
## 🎯 Objetivo

Criar um bot no Telegram que faça uso de uma API para consultar documentos jurídicos e responder sobre eles.

## ⚒️ Infraestrutura

- API em EC2, rodando com Flask, servida por Gunicorn e com Nginx como proxy reverso.
- Bucket S3 para armazenamento dos documentos jurídicos on-line.
- Aplicação para consumir a API que se comunica com o telegram.

Temos 3 aplicações dockerizada:

- API do processamento do ChatBot (Flask + Gunicorn)
- Aplicação para comunicação com o Telegram
- Proxy Reverso (Nginx)

A aplicação é Dockerizada e pode ser subida para a AWS com uso de Terraform.

---

## Subindo e Rodando a API

> Pré-requisitos: para testes locais, tenha instalado em sua máquina Docker. Para subir para a AWS, é necessário ter instalado Terraform.

1. Faça download do repositório, seja manualmente ou com uso de `git`.

2. Obtenha um *key pair* na AWS. Ele será usado pela EC2 para permitir conexões SSH.
  
    Para isso, vá em `EC2 > Network & Security > Key Pairs`. Clique em `Create Key Pair`. Escolha um nome, o tipo como "RSA" e o formato como ".pem". Clique, novamente em `Create Key Pair`. **Será feito o download de um arquivo .pem**. Guarde-o, pois será necessário usá-lo para acessar a instância EC2.

3. No diretório onde for feito o download, mova para a pasta de `terraform/`:

    ```bash
    terraform init
    terraform plan
    ```
    Várias perguntas serão feitas para preencher os dados necessários para subir a infraestrutura. Entre eles, o nome do Key Pair criado anteriormente! Também, o nome do Bucket - o qual deve ser único - e o endereço IP a que permitir conexões SSH.
    > Obs.: o endereço IP próprio pode ser visualizado no próprio console da AWS ao editar uma inbound ou outbound rule, em um Security Group e selecionar "My IP" em "Source". Cuidado, entretanto, para não realizar modificações indesejadas. Na dúvida, cancele.
    
    Caso toda a infraestrutura esteja adequada, podem-se aplicar as mudanças com:
    ```bash
    terraform apply
    ```
  
  4. Se as mudanças foram corretamente aplicadas, deve ter sido criado um bucket em sua conta AWS com o nome especificado. Faça upload dos documentos jurídicos desejados para ele.
  
  5. Mova de volta para o diretório raiz (onde estiver o repositório). Crie o arquivo `.env`, com base em `.env.example`:
  
      ```bash
      cp .env.example .env
      ```
      Preencha o arquivo `.env` gerado com as informações desejadas. Faça uso do que desejar do exemplo.

  6. Copie o código para a EC2:

      ```bash 
      rsync -avrz -e "ssh -i <CAMINHO_ARQUIVO_PEM>" <CAMINHO_DIRETORIO_ATUAL> ec2-user@<IP_PUBLICO_DA_EC2>:/home/ec2-user/api/ --exclude-from=copy-ignore.txt
      ```
      > Obs.: Troque os valores entre '<' e '>' pelos dados correspondentes.
    
  7. Por último, entre na EC2 com SSH e vá para /api/:
      ```bash 
      ssh -i "<CAMINHO_ARQUIVO_PEM>" ec2-user@<IP_PUBLICO_DA_EC2>
      ```
      ```bash 
        cd api/
      ```
      E, se não houver sido feito automaticamente, rode o script de inicialização na EC2. Ele se encontra em `terraform/ec2/start-script.sh`.
      ```bash 
      <comandos_do_script>
      ```

Após esses passos, é espero que o servidor esteja rodando normalmente. Isso ficará evidente pelos logs emitidos após execução do script de inicialização.
Caso não rode automaticamente, use `docker compose up --build`.

---

## ⚙️ Como criar e configurar o Bot do Telegram?

### 1. Criação do Bot no Telegram

1. Abra o app do Telegram e procure por [@BotFather](https://t.me/BotFather).
2. Envie o comando `/start` e depois `/newbot`.
3. Escolha o nome de exibição do seu bot.
4. Escolha um nome de usuário para o seu bot (deve terminar em `bot` - ex: `Chatbot_Juridico_bot`).
5. Copie e salve o **token** que o BotFather fornecer, pois ele será usado no `.env.telegrambot`.

### 2. Configurações possíveis com o BotFather:

* `/setdescription` – Define a descrição do seu bot.
* `/setabouttext` – Define o texto "Sobre" exibido no perfil do bot.
* `/setuserpic` – Define uma foto de perfil.
* `/deletebot` – Remove permanentemente o bot.
* `/setcommands` – Define comandos personalizados visíveis no menu do bot.

  **Exemplos de uso do `/setcommands`:**

  ```
  start - Inicia o bot
  ajuda - Mostra informações sobre o funcionamento do bot
  sobre - Exibe informações sobre o projeto
  ```
---

## 💻 Utilizando o Sistema

### Acesse o Bot através dos seguintes link de convite pra conversa:

- https://t.me/juridico_grupo3_bot
  
### Após acessar o bot sinta-se livre para começar o envio de mensagens!

![Exemplo de conversa do Bot](./assets/chat-bot.jpg)

## 🥔 Desenvolvimento da avaliação

Utilizamos o Trello como ferramenta de gerenciamento de tarefas, aliado a reuniões diárias (dailys), nas quais o progresso era compartilhado entre os integrantes da equipe.

Para enfrentar o desafio, dividimos o projeto em três frentes principais:

- Infraestrutura

- Criação e utilização do RAG

- Integração com o Telegram

Trabalhamos de forma paralela nessas três áreas até que restassem apenas tarefas de integração e correção de erros, que foram resolvidas até a conclusão do projeto.
  
🔗 [Repositório](https://github.com/Compass-pb-aws-2025-JANEIRO/sprints-7-8-pb-aws-janeiro/tree/grupo-3)

### 2. 🧾 API de Processamento de Notas Fiscais
API REST em Python que extrai dados de notas fiscais usando Amazon Textract + NLP (SpaCy/NLTK), com armazenamento em S3 e logs via CloudWatch. 
<img width="3808" height="2228" alt="image" src="https://github.com/user-attachments/assets/b398feaa-a3b5-4c2f-95f2-d8a47c1d7d0a" />
 
🎯 Objetivo
Desenvolver uma API que recebe imagens de notas fiscais, extrai dados estruturados via AWS Textract, aplica técnicas de NLP (Spacy/NLTK) e refina a saída com uma LLM. Os resultados são armazenados em bucket S3, organizados por tipo de pagamento, e monitorados via CloudWatch.

📍 Rotas
POST /api/v1/invoice
Processamento:

Extração de texto e informações com AWS Textract;
Identificação de campos (CNPJ, valor total, forma de pagamento, etc.);
Refinamento dos dados com LLM (Nova Micro);
Armazenamento de imagem em bucket S3: notas com pagamento em dinheiro/pix → pasta /dinheiro, demais → /outros.
Entrada: Não há conteúdo em body. Envie o próprio arquivo na requisição ou o faça por meio de um formulário.

Saída:

{
   "nome_emissor": "nome do emissor",
   "CNPJ_emissor": "CNPJ do emissor",
   "endereco_emissor": "endereço do emissor",
   "CNPJ_CPF_consumidor": "CNPJ ou CPF do consumidor",
   "data_emissao": "DD/MM/YYYY",
   "numero_nota_fiscal": "número de nota fiscal",
   "serie_nota_fiscal": "série de nota fiscal",
   "valor_total": "valor total",
   "forma_pgto": "dinheiropix | outro"
}
Obs.: Métricas e logs referentes às execuções podem ser encontrados no CloudWatch ou com Grafana.

Obs.2: Valores desconhecidos serão preenchidos com "None".

Ferramentas Utilizadas:

S3, Textract, CloudWatch, Lambda, API Gateway, Step Functions;
NLTK e LLM
GET /
Descrição:

Página web para uso da API.
Entrada: Não há.

Saída: Arquivo HTML (text/html).

⚙️ Como Executar?
Pré-requisitos
Conta AWS com permissões para os recursos gerados/utilizados;
Python 3.12 e venv ;
Credenciais AWS configuradas (via AWS CLI ou por variáveis de ambiente);
AWS CLI e SAM CLI.
Passo-a-Passo
Clone o repositório:

git clone https://github.com/Compass-pb-aws-2025-JANEIRO/sprints-4-5-6-pb-aws-janeiro.git
cd sprints-4-5-6-pb-aws-janeiro
Configure o ambiente virtual:

# Crie o ambiente (nomeado 'python')
python -m venv python

# Entre no ambiente
source python/bin/activate  # Linux
python/Scripts/activate # Windows
Instale as dependências:

pip install -r requirements.txt
Configure as credenciais AWS.

Faça o deploy da aplicação:

sam build
sam deploy --guided
Responda às perguntas de sam deploy --guided para fazer o deploy para a AWS.
🔗 [Repositório](https://github.com/Compass-pb-aws-2025-JANEIRO/sprints-4-5-6-pb-aws-janeiro/tree/grupo-3)

### 3. 🌐 Sistema de Coleta de RSS com NodeJS
API que extrai, transforma e armazena feeds RSS em JSON no S3. Deploy com Docker e interface HTML.  
🔗 [Repositório](https://github.com/Compass-pb-aws-2025-JANEIRO/sprints-2-3-pb-aws-janeiro/tree/grupo-4)

### 4. 🧾 Cadastro de Usuários com JavaScript (Sprint 1)
Aplicação front-end simples com cadastro, listagem e remoção de usuários no `LocalStorage`. Explora manipulação de DOM, uso de padrão Factory e armazenamento no navegador.  
🔗 [Repositório](https://github.com/Compass-pb-aws-2025-JANEIRO/sprint-1-pb-aws-janeiro/tree/dawson-monteiro)

### 5. 📊 Question Answering sobre Dados Tabulares
Sistema de perguntas e respostas sobre datasets `.parquet` com técnicas clássicas de NLP (TF-IDF, heurísticas, análise de colunas com pandas). Gera previsões automáticas a partir de perguntas livres em linguagem natural e realiza matching com templates semânticos.  
🔗 [Repositório](https://github.com/dwsoliv73/question-answering-tabular)

---

## 📜 Certificações

- ✅ **AWS Cloud Practitioner**  
🔗 [Credly Badge](https://www.credly.com/badges/06a16251-6b0d-4d07-8699-061ad119ec14/linked_in_profile)

---

- ✅ **Data Science for Business**  
🔗 https://www.credly.com/badges/2b9942b1-bae8-47d5-b1fd-baed2bf84fa4/public_url

---

## 🛠️ Tecnologias

- **Linguagens**: Python, JavaScript, Java, HTML/CSS  
- **Cloud & DevOps**: AWS (S3, EC2, CloudWatch), Docker, Git  
- **IA & NLP**: LangChain, Amazon Bedrock, SpaCy, NLTK, TF-IDF  
- **Outros**: REST APIs, Textract, RAG pipelines, Pandas, ChromaDB

---

## 📫 Contato

- 📧 dawsonm@id.uff.br  
- 📱 +55 (21) 99525-2211  
- 🌐 [LinkedIn](https://www.linkedin.com/dawson-monteiro/)
