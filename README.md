# Apoia Já

Sistema web para controlar o empréstimo e a devolução de materiais de laboratórios e salas de aula.

<p align="center">
  <img alt="Status do projeto" src="https://img.shields.io/badge/status-documenta%C3%A7%C3%A3o%20de%20MVP-orange" />
  <img alt="Tecnologia" src="https://img.shields.io/badge/tech-C%23-blue" />
  <img alt="Escopo" src="https://img.shields.io/badge/escopo-MVP%20Senac-lightgrey" />
</p>

## Visão geral

Em laboratórios e salas de aula, o controle de materiais costuma ser feito em caderno, planilha ou pela memória. Isso dificulta o acompanhamento de quem está com cada item, a identificação de atrasos e a prevenção de perdas.

Este projeto propõe uma solução web para o responsável pelo laboratório registrar materiais e solicitantes, controlar empréstimos e devoluções e consultar rapidamente o que está disponível, emprestado ou em atraso.

> Status: documentação de requisitos do MVP (Exercício 01 · Aula 01 · Descoberta de Requisitos · Senac · C#). Ainda não há implementação, testes ou deploy realizados.

## Problema e contexto

O processo manual gera falhas como:

- indisponibilidade de informação sobre o status de cada material;
- dificuldade para acompanhar prazos e atrasos;
- risco de perda ou extravio de itens;
- demora na resposta às solicitações de materiais.

O sistema foi pensado para um ambiente simples e controlado: um único laboratório, um único perfil de acesso e um responsável como usuário principal.

## Persona

### Marcos — técnico de laboratório

- Dor: não sabe quem está com cada material e só descobre atrasos quando alguém precisa do item.
- Cenário: alguém solicita um material e ele precisa verificar rapidamente se o item está disponível e registrar o empréstimo.
- Métrica de sucesso: registrar um empréstimo em até 60 segundos, sem treinamento.

A persona e a entrevista que originaram o projeto são simuladas, sendo utilizadas como exercício de levantamento de requisitos.

## Objetivo do MVP

O MVP permite que o responsável pelo laboratório:

- cadastre materiais;
- cadastre solicitantes;
- registre empréstimos;
- registre devoluções;
- visualize materiais disponíveis e em atraso;
- consulte o histórico de movimentações.

## Funcionalidades principais

- Cadastro de materiais com nome, código de patrimônio, categoria e estado de conservação;
- Busca por nome, código ou situação do item;
- Cadastro de solicitantes com nome, tipo e contato;
- Registro de empréstimo com prazo de devolução;
- Registro de devolução e atualização automática da disponibilidade;
- Listagem de empréstimos em aberto;
- Destaque visual para itens em atraso;
- Consulta do histórico por material e por solicitante.

## Requisitos

### Requisitos funcionais

Prioridade pelo método MoSCoW: Must (essencial), Should (importante) e Could (desejável).

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF01 | Cadastrar materiais com nome, código de patrimônio, categoria e estado de conservação. | Must |
| RF02 | Consultar materiais por nome, código ou situação (disponível ou emprestado). | Must |
| RF03 | Editar e excluir materiais. A exclusão é bloqueada se houver empréstimo em aberto. | Should |
| RF04 | Cadastrar e consultar solicitantes com nome, tipo (aluno, professor ou funcionário) e um contato. | Must |
| RF05 | Registrar empréstimo com material disponível, solicitante e prazo de devolução. | Must |
| RF06 | Registrar devolução, com observação opcional sobre o estado do material. | Must |
| RF07 | Listar empréstimos em aberto com material, solicitante e prazo. | Must |
| RF08 | Destacar visualmente os empréstimos atrasados. | Should |
| RF09 | Consultar o histórico de empréstimos por material e por solicitante. | Could |

### Requisitos não funcionais

| ID | Requisito | Como verificar |
| --- | --- | --- |
| RNF01 | Desempenho: listagens e buscas respondem em até 2 s com até 1.000 materiais e 5.000 empréstimos. | Medir o tempo com uma massa de dados de teste. |
| RNF02 | Responsividade: interface utilizável de 360 px até desktop, sem rolagem horizontal. | Testar no Chrome e no Firefox, em celular e computador. |
| RNF03 | Usabilidade: um responsável sem treinamento registra um empréstimo em até 60 s na primeira tentativa. | Testar com pelo menos 2 pessoas que nunca usaram o sistema. |
| RNF04 | Persistência: os dados continuam íntegros após reiniciar a aplicação. | Cadastrar dados, reiniciar e conferir. |
| RNF05 | Segurança: o acesso exige login e as senhas são armazenadas com hash. | Inspecionar o banco e tentar acessar sem autenticação. |
| RNF06 | Privacidade: coletar apenas nome, tipo e um contato do solicitante (sem CPF, RG ou outros documentos). | Revisar os campos do cadastro de solicitantes. |

## Histórias de usuário

- US01 · Registrar empréstimo (Must): como responsável pelo laboratório, quero registrar o empréstimo de um material a um solicitante, para saber quem está com cada item.
- US02 · Registrar devolução (Must): como responsável pelo laboratório, quero registrar a devolução de um material, para que ele volte a ficar disponível.
- US03 · Consultar material (Must): como responsável pelo laboratório, quero buscar um material e ver se está disponível ou com quem está, para atender rapidamente às solicitações.
- US04 · Identificar atrasos (Should): como responsável pelo laboratório, quero ver os empréstimos atrasados em destaque, para cobrar as devoluções antes que os materiais façam falta.

## Critérios de aceite

### US01 · Registrar empréstimo

- Só materiais disponíveis podem ser selecionados.
- Material, solicitante e prazo são obrigatórios. Se faltar algo, o sistema mostra uma mensagem clara e não salva.
- O prazo não pode ser anterior à data atual.
- Após salvar, o material passa para emprestado e aparece na lista de empréstimos em aberto.

### US02 · Registrar devolução

- A devolução só pode ser registrada para um empréstimo em aberto.
- Data e hora da devolução são registradas automaticamente.
- Após registrar, o material volta para disponibilidade e sai da lista de empréstimos em aberto.
- O mesmo empréstimo não pode ser devolvido duas vezes.

### US03 · Consultar material

- A busca funciona por nome parcial ou código de patrimônio.
- O resultado mostra a situação atual do material.
- Se estiver emprestado, mostra o solicitante e o prazo de devolução.
- Se nada for encontrado, o sistema informa isso claramente.

### US04 · Identificar atrasos

- Um empréstimo está atrasado quando o prazo é anterior à data atual e ele ainda está em aberto.
- Os atrasados aparecem destacados na lista de empréstimos em aberto.
- Após a devolução, o empréstimo deixa de aparecer como atrasado.

## O que não será incluído no MVP

- autoatendimento ou reserva de materiais por alunos e professores;
- aluguel de equipamentos ou cobrança pelo uso;
- notificações por e-mail, SMS ou WhatsApp;
- leitura de QR code ou código de barras;
- multas, penalidades ou bloqueio de solicitantes;
- vários laboratórios ou vários perfis de acesso;
- relatórios gráficos e exportação de dados;
- integração com sistemas acadêmicos;
- aplicativo móvel nativo;
- controle de manutenção, baixa ou depreciação de patrimônio.

## Tecnologias previstas

- C#
- ASP.NET Core (ou estrutura equivalente de aplicação web em C#)
- Persistência com banco relacional
- Interface responsiva para desktop e mobile

## Estrutura do projeto

Como o projeto ainda está em fase de especificação, a estrutura será definida conforme a implementação inicial. A ideia principal é manter uma arquitetura simples e organizada para o MVP, com foco na gestão de materiais, solicitantes e empréstimos.

## Contribuição

Este repositório está sendo utilizado como base para o desenvolvimento do MVP da disciplina. Sugestões, melhorias e ajustes na documentação podem ser enviados por meio de pull requests, desde que mantenham o foco no escopo definido.

## Licença

Ainda não foi definida uma licença pública para o projeto. Em breve, essa seção poderá indicar o tipo de licença adotado.
