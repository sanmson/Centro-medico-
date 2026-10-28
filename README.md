# ClinicaMZ — versão sem servidor (HTML, CSS e JavaScript)

Cada ecrã é um ficheiro HTML. Não há servidor: abra **`index.html`** com duplo clique no **Chrome ou no Edge**.
Os dados ficam guardados numa base de dados dentro do navegador (localStorage). O Node.js é opcional e só serve para gerar os dados iniciais.

## Como usar

1. Descompacte a pasta.
2. Abra `index.html`.
3. Entre com um dos utilizadores abaixo.

| Perfil | E-mail | Palavra-passe |
|---|---|---|
| Administrador | admin@clinicamz.local | clinica123 |
| Recepcionista | recepcao@clinicamz.local | clinica123 |
| Enfermeiro | enfermeiro@clinicamz.local | clinica123 |
| Médico (Clínica Geral) | neusa@clinicamz.local | clinica123 |
| Médico (Pediatria) | rui@clinicamz.local | clinica123 |
| Técnico de Laboratório | laboratorio@clinicamz.local | clinica123 |
| Farmacêutico | farmacia@clinicamz.local | clinica123 |

Na primeira abertura a base é criada com 5 pacientes e 5 marcações (3 hoje e 2 amanhã).

## Fluxo para experimentar

1. **Recepção:** Pacientes → novo; Agenda → marcar; «Registar chegada».
2. **Enfermeiro:** Triagem → sinais vitais.
3. **Médico:** Consultas → abrir → anamnese, diagnóstico, receita, exames → concluir.
4. **Laboratório:** regista o resultado.
5. **Farmácia:** dispensa a receita (abate o stock).
6. **Recepção:** Facturação → emitir factura → pagamento total ou parcial.
7. **Administrador:** utilizadores, relatórios (CSV e «Guardar PDF»), auditoria, anular factura, cópia de segurança.

## Ficheiros

```
clinicamz-web/
├── index.html                login
├── painel.html               painel por perfil
├── pacientes.html            lista e pesquisa          paciente-form.html   novo / editar
├── agenda.html               agenda e marcações        lembretes.html       lembretes SMS
├── triagem.html              fila de triagem           triagem-form.html    sinais vitais
├── consultas.html            consultas do médico       consulta.html        anamnese, receita, exames
├── historico.html            pesquisa de histórico     historico-paciente.html
├── laboratorio.html          resultados de exames
├── farmacia.html             stock e dispensa
├── facturacao.html           lista de facturas         factura.html         detalhe e pagamentos
├── utilizadores.html         utilizadores e cópia de segurança
├── relatorios.html           relatórios (CSV / PDF)    auditoria.html       rasto de auditoria
├── css/estilo.css            azul marinho e preto; janelas com barra de título, grupos, separadores e barra de botões; Times New Roman
├── js/
│   ├── db.js                 BASE DE DADOS: tabelas, chaves, transacções (o «link de conexão»)
│   ├── dados_iniciais.js     dados de arranque (gerado pelo Node.js)
│   ├── seguranca.js          hash SHA-256 com sal, sessão de 2 min, menu por perfil
│   ├── util.js               datas, dinheiro, escape de HTML
│   ├── repositorio.js        consultas à base de dados
│   ├── servicos.js           regras de negócio RN01 a RN10 + auditoria
│   ├── ui.js                 menu lateral, verificação de sessão e perfil
│   └── paginas/              um script por ecrã
└── node/gerar_dados.js       opcional: recria dados_iniciais.js
```

**Ligação à base de dados:** todos os ecrãs carregam `js/db.js` e usam a mesma base. Cada ecrã segue as camadas do relatório: ecrã (`paginas/`), regras (`servicos.js`) e acesso a dados (`repositorio.js` e `db.js`).

## Node.js (opcional, sem servidor)

```
node node/gerar_dados.js                 # recria js/dados_iniciais.js com a palavra-passe clinica123
node node/gerar_dados.js MinhaSenha      # com outra palavra-passe inicial
```

Depois, em Utilizadores → «Repor dados de exemplo», o navegador passa a usar o ficheiro novo.

## Cópia de segurança

Os dados vivem **neste navegador e neste computador**. Em Utilizadores, use «Descarregar cópia de segurança» com frequência e «Restaurar cópia» para recuperar. Se limpar os dados do navegador, a informação perde-se.

## Limitações (por não haver servidor)

- **Segurança:** o hash das palavras-passe e o controlo de perfis correm no navegador, por isso servem para demonstração. Qualquer pessoa com acesso ao computador pode ver ou alterar os dados pelas ferramentas do navegador. Para dados reais de pacientes é preciso um servidor com base de dados.
- **Um só computador:** cada computador tem a sua própria base. Para partilhar dados entre postos é preciso um servidor.
- **Navegador:** testado no Chromium (Chrome e Edge). O Firefox pode tratar cada ficheiro `file://` como origem separada e não partilhar os dados entre ecrãs. Nesse caso use Chrome ou Edge.
- **SMS (RF16):** só prepara as mensagens (para pacientes com consentimento e sem dados clínicos). O envio real exige um serviço externo.
