📦 Controle de Empréstimo de Materiais

Sistema web para controlar o empréstimo e a devolução de materiais de laboratórios e salas de aula.

Status: documentação de requisitos do MVP (Exercício 01, Aula 01 · Descoberta de Requisitos · Senac · C#). Nada foi implementado ou testado ainda, por isso os critérios de aceite seguem desmarcados.

01 · Contexto

Em laboratórios e salas de aula, o controle de materiais costuma ser feito em caderno, planilha ou de memória, o que dificulta saber quem está com cada item, perceber atrasos e evitar perdas. Este projeto propõe um sistema web em que o responsável pelo laboratório cadastra materiais e solicitantes, registra empréstimos e devoluções e consulta rapidamente o que está disponível, emprestado ou atrasado. O público é o responsável pelo laboratório, único usuário do sistema no MVP; alunos, professores e funcionários apenas solicitam materiais e não têm conta. As restrições são: um único laboratório, um único perfil de acesso, desenvolvimento individual em C# dentro do prazo da disciplina, persistência dos dados e interface responsiva.

02 · Persona

Marcos, técnico de laboratório (persona primária)

	
Dor	Não sabe quem está com cada material e só descobre os atrasos quando alguém precisa do item.
Cenário	Alguém pede um material. Marcos precisa conferir se está disponível e registrar o empréstimo rapidamente.
Métrica de sucesso	Registrar um empréstimo em até 60 segundos, sem treinamento.

A persona e a entrevista que a originou são simuladas, usadas como exercício de levantamento de requisitos.

03 · RF

Prioridade pelo método MoSCoW: Must (essencial), Should (importante) e Could (desejável).

ID	Requisito	Prioridade
RF01	Cadastrar materiais com nome, código de patrimônio, categoria e estado de conservação.	Must
RF02	Consultar materiais por nome, código ou situação (disponível ou emprestado).	Must
RF03	Editar e excluir materiais. A exclusão é bloqueada se houver empréstimo em aberto.	Should
RF04	Cadastrar e consultar solicitantes com nome, tipo (aluno, professor ou funcionário) e um contato.	Must
RF05	Registrar empréstimo com material disponível, solicitante e prazo de devolução.	Must
RF06	Registrar devolução, com observação opcional sobre o estado do material.	Must
RF07	Listar empréstimos em aberto com material, solicitante e prazo.	Must
RF08	Destacar visualmente os empréstimos atrasados.	Should
RF09	Consultar o histórico de empréstimos por material e por solicitante.	Could
04 · RNF
ID	Requisito	Como verificar
RNF01	Desempenho: listagens e buscas respondem em até 2 s com até 1.000 materiais e 5.000 empréstimos.	Medir o tempo com uma massa de dados de teste.
RNF02	Responsividade: interface utilizável de 360 px até desktop, sem rolagem horizontal.	Testar no Chrome e no Firefox, em celular e computador.
RNF03	Usabilidade: um responsável sem treinamento registra um empréstimo em até 60 s na primeira tentativa.	Testar com pelo menos 2 pessoas que nunca usaram o sistema.
RNF04	Persistência: os dados continuam íntegros após reiniciar a aplicação.	Cadastrar dados, reiniciar e conferir.
RNF05	Segurança: o acesso exige login e as senhas são armazenadas com hash.	Inspecionar o banco e tentar acessar sem autenticação.
RNF06	Privacidade: coletar apenas nome, tipo e um contato do solicitante (sem CPF, RG ou outros documentos).	Revisar os campos do cadastro de solicitantes.
05 · US
US01 · Registrar empréstimo (Must): como responsável pelo laboratório, quero registrar o empréstimo de um material a um solicitante, para saber quem está com cada item.
US02 · Registrar devolução (Must): como responsável pelo laboratório, quero registrar a devolução de um material, para que ele volte a ficar disponível.
US03 · Consultar material (Must): como responsável pelo laboratório, quero buscar um material e ver se está disponível ou com quem está, para atender rápido às solicitações.
US04 · Identificar atrasos (Should): como responsável pelo laboratório, quero ver os empréstimos atrasados em destaque, para cobrar as devoluções antes que os materiais façam falta.
06 · AC

US01 · Registrar empréstimo

 Só materiais Disponíveis podem ser selecionados.
 Material, solicitante e prazo são obrigatórios. Se faltar algo, o sistema mostra uma mensagem clara e não salva.
 O prazo não pode ser anterior à data atual.
 Após salvar, o material passa para Emprestado e aparece na lista de empréstimos em aberto.

US02 · Registrar devolução

 A devolução só pode ser registrada para um empréstimo em aberto.
 Data e hora da devolução são registradas automaticamente.
 Após registrar, o material volta para Disponível e sai da lista de empréstimos em aberto.
 O mesmo empréstimo não pode ser devolvido duas vezes.

US03 · Consultar material

 A busca funciona por nome parcial ou código de patrimônio.
 O resultado mostra a situação atual do material.
 Se estiver emprestado, mostra o solicitante e o prazo de devolução.
 Se nada for encontrado, o sistema informa isso claramente.

US04 · Identificar atrasos

 Um empréstimo está atrasado quando o prazo é anterior à data atual e ele ainda está em aberto.
 Os atrasados aparecem destacados na lista de empréstimos em aberto.
 Após a devolução, o empréstimo deixa de aparecer como atrasado.
07 · OUT

O que não será feito no MVP:

Autoatendimento ou reserva de materiais por alunos e professores
Aluguel de equipamentos ou cobrança pelo uso (possível expansão futura, depois do MVP)
Notificações por e-mail, SMS ou WhatsApp
Leitura de QR code ou código de barras
Multas, penalidades ou bloqueio de solicitantes
Vários laboratórios ou vários perfis de acesso
Relatórios gráficos e exportação de dados
Integração com sistemas acadêmicos
Aplicativo móvel nativo
Controle de manutenção, baixa ou depreciação de patrimônio
