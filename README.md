# **Arquitetura de Sistema: Med Schedule (Gestão de Clínicas)**

## **1\. Funcionalidades Principais**

* **Autenticação e Autorização:** Cadastro, login e recuperação de senha seguros.  
* **Agendamento Online:** Busca por médicos (por especialidade) e marcação de consultas nos horários disponíveis.  
* **Gestão de Horários (Agenda):** Médicos e secretárias podem definir e alterar seus blocos de horários de atendimento.  
* **Notificações:** Envio de lembretes de consultas via e-mail/SMS para evitar faltas.  
* **Histórico de Consultas:** Prontuário simplificado contendo o histórico de consultas de cada paciente.

## **2\. Tipos de Usuários e Permissões**

* **Paciente:** Pode se cadastrar, visualizar médicos, agendar, remarcar ou cancelar suas próprias consultas, e visualizar seu próprio histórico.  
* **Médico:** Pode visualizar sua agenda de atendimentos, definir seus horários disponíveis e registrar anotações no histórico do paciente atendido.  
* **Administrador / Secretária:** Tem controle total. Pode gerenciar o cadastro de médicos, pacientes, especialidades e manipular a agenda de qualquer profissional.

## **3\. Diagrama de Arquitetura**

O sistema segue o modelo clássico de arquitetura em 3 camadas, focado em desacoplamento e facilidade de manutenção.
<img width="4948" height="3720" alt="Single Page Application API-2026-06-03-223444" src="https://github.com/user-attachments/assets/bebd4fae-e733-4e77-8ac1-46417e389552" />
## **4\. Entidades Principais e Relacionamentos**
O modelo de dados baseia-se em cinco entidades fundamentais:
<img width="5695" height="4270" alt="Healthcare User Management-2026-06-03-223515" src="https://github.com/user-attachments/assets/f1d601a4-c4a9-4743-9dde-2a34f153ec06" />
## **5\. Endpoints da API (Rotas REST)**

### **Autenticação (/api/v1/auth)**

* POST /auth/register \- Cadastro de novos usuários (Pacientes).  
* POST /auth/login \- Autenticação de usuário (Retorna Token JWT).

### **Médicos e Especialidades (/api/v1/medicos)**

* GET /medicos \- Lista médicos (com filtros por especialidade e nome).  
* GET /medicos/{id}/horarios \- Lista horários disponíveis de um médico específico.  
* POST /medicos \- Cadastro de médico (Apenas Admin).

### **Agendamentos (/api/v1/consultas)**

* GET /consultas \- Lista consultas (Paciente vê as suas; Médico vê as suas; Admin vê todas).  
* POST /consultas \- Cria um novo agendamento.  
* PUT /consultas/{id} \- Atualiza o status de uma consulta (ex: Cancelar ou Concluir com anotações).

## **6\. Tecnologias Sugeridas**

* **Frontend:** Framework Web Moderno (React.js ou Vue.js) com TailwindCSS para estilização rápida.  
* **Backend:** Framework Web corporativo ou ágil (Node.js com NestJS, Java com Spring Boot ou Python com FastAPI).  
* **Banco de Dados:** Banco Relacional (PostgreSQL ou MySQL), ideal devido à forte consistência necessária para evitar agendamentos duplicados no mesmo horário.  
* **Autenticação:** JWT (JSON Web Tokens) para comunicação Stateless segura entre o client e a API.

## **7\. Fluxos Principais (Descrição Textual)**

### **Fluxo de Agendamento de Consulta:**

1. O **Paciente** realiza o login no sistema e recebe um Token JWT.  
2. O Paciente navega pela tela de busca, seleciona uma **Especialidade** e escolhe um **Médico**.  
3. O frontend faz uma requisição GET /medicos/{id}/horarios e exibe as horas vagas.  
4. O Paciente escolhe um horário e clica em "Confirmar", disparando um POST /consultas.  
5. O **Backend** inicia uma transação no banco de dados:  
   * Verifica se o horário ainda está disponível.  
   * Se sim, insere o registro na tabela CONSULTA com o status "AGENDADO".  
   * Se não, retorna um erro de conflito (HTTP 409).  
6. O sistema dispara um evento em segundo plano para enviar o e-mail de confirmação ao paciente.

## **📜 Prompts Utilizados e Ferramentas (Exemplo de Registro)**

Como parte do entregável exigido pela prática, segue o mapeamento de como ferramentas de IA poderiam ser consultadas para acelerar esse processo:

1. **Ferramenta:** ChatGPT / Claude  
   * **Prompt para Escopo de Dados:** \> *"Atue como um Engenheiro de Software Sênior. Preciso criar um sistema de agendamento de consultas médicas. Quais devem ser as entidades principais do banco de dados e seus relacionamentos para garantir que um médico não tenha duas consultas no mesmo horário? Forneça os atributos principais de cada tabela."*  
2. **Ferramenta:** ChatGPT (com saída em sintaxe Mermaid)  
   * **Prompt para Diagramação:***"Com base nas entidades mapeadas anteriormente (Usuario, Paciente, Medico, Especialidade, Consulta), gere o código de um Diagrama de Entidade-Relacionamento (ER) utilizando a sintaxe Mermaid para que eu possa visualizar graficamente."*  
3. **Ferramenta:** ChatGPT / Copilot  
   * **Prompt para Endpoints REST:***"Crie uma lista com as principais rotas REST (URL, Verbo HTTP e uma breve descrição do comportamento) para o módulo de agendamento de consultas desse sistema, separando o que o Paciente pode acessar do que o Administrador acessa."*

