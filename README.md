# 🌎 Órbita de Notícias RSS

**Agregador de feeds RSS 2.0 e Atom desenvolvido em Rust**

O Órbita RSS é uma aplicação web desenvolvida para reunir, organizar e acompanhar notícias de diferentes fontes por meio de feeds RSS 2.0 e Atom. A interface apresenta as fontes em um globo giratório e organiza os artigos em uma grade de três colunas, com até quatro linhas visíveis por tela.

## 🎓 Informações acadêmicas

- **Instituição:** Faculdade AEMS — Três Lagoas/MS
- **Curso:** Engenharia da Computação
- **Período:** 6º período
- **Integrantes:**
  - Samuel Dias Medeiros
  - Robert William

## 🛠️ Tecnologias utilizadas

- **Rust:** linguagem principal do projeto.
- **Cargo:** gerenciamento e execução do projeto.
- **Axum:** servidor HTTP e rotas da API.
- **SQLite:** armazenamento persistente dos dados.
- **HTML, CSS e JavaScript:** desenvolvimento da interface web.
- **Canvas:** renderização do globo giratório.
- **RSS 2.0 e Atom:** formatos utilizados para obter notícias.
- **Cliente HTTP e parser XML:** consulta e interpretação dos feeds.

## 📋 Requisitos

- Rust estável e Cargo instalados.
- Acesso à internet para consultar os feeds.
- Navegador web atualizado.

## ▶️ Como executar

No terminal, dentro do diretório do projeto, execute:

```powershell
cargo run
```

Após iniciar o servidor, abra o navegador e acesse:

[http://127.0.0.1:3000](http://127.0.0.1:3000)

O banco de dados `orbita.sqlite` será criado no diretório do projeto para armazenar as fontes, os artigos, o estado de leitura e os favoritos.

Para executar os testes automatizados, utilize:

```powershell
cargo test
```

## 📰 Como utilizar

1. **Adicionar fonte:** informe o nome, a URL do feed RSS/Atom e as coordenadas geográficas aproximadas da fonte.
2. **Atualizar notícias:** utilize a opção **Atualizar tudo** para consultar os feeds cadastrados.
3. **Atualização automática:** a aplicação tenta atualizar os feeds a cada 15 minutos enquanto o servidor estiver em execução.
4. **Visualizar notícias:** alterne entre a visão global e a lista de artigos.
5. **Pesquisar e filtrar:** pesquise por palavras, filtre por fonte, visualize somente artigos não lidos ou acesse os favoritos.
6. **Favoritar artigos:** clique na estrela para adicionar ou remover um artigo dos favoritos.
7. **Marcar como lida:** utilize a opção correspondente para atualizar o estado de leitura do artigo.

As coordenadas representam a localização atribuída à fonte, não necessariamente o local do acontecimento noticiado. O globo é uma visualização ilustrativa desenvolvida com Canvas, sem integração com serviços de mapas ou necessidade de chave de API.

## 🏗️ Arquitetura do projeto

A aplicação está organizada em módulos para separar as responsabilidades:

- `src/main.rs`: inicialização do servidor HTTP, definição das rotas e validação dos dados recebidos.
- `src/feeds.rs`: download dos feeds, interpretação dos formatos RSS/Atom, atualização concorrente e prevenção de duplicatas.
- `src/db.rs`: inicialização e acesso ao banco de dados SQLite.
- `src/models.rs`: definição das estruturas de dados compartilhadas pela aplicação e pela API.
- `static/`: arquivos HTML, CSS e JavaScript responsáveis pela interface web.

### Fluxo principal

```text
Navegador
    |
    v
Interface Web (HTML, CSS e JavaScript)
    |
    v
API HTTP (Axum)
    |
    v
Cliente HTTP e Parser RSS/Atom
    |
    v
Banco de Dados SQLite
```

O núcleo de leitura, deduplicação e persistência é implementado em Rust, enquanto a interface web permite a interação com as funcionalidades da aplicação.

## ⚙️ Decisões técnicas

### 1. Concorrência

A utilização de `buffer_unordered(5)` permite consultar até cinco feeds simultaneamente. Dessa forma, uma fonte lenta não impede que as demais consultas sejam concluídas.

### 2. Persistência de dados

O SQLite armazena as informações de maneira persistente, dispensando a instalação e a manutenção de um servidor de banco de dados separado.

### 3. Atualização eficiente

Quando o servidor do feed fornece os cabeçalhos `ETag` ou `Last-Modified`, a aplicação utiliza os valores nas requisições seguintes por meio de `If-None-Match` e `If-Modified-Since`.

Quando o servidor retorna HTTP 304 (Not Modified), não é necessário baixar e interpretar novamente o conteúdo do feed.

### 4. Prevenção de duplicatas

O link do artigo é utilizado como chave principal de identificação, com normalização do esquema e do domínio, remoção do fragmento e da barra final.

A caixa do caminho é preservada, pois alguns servidores diferenciam letras maiúsculas e minúsculas.

Quando um artigo não possui link, a identificação combina o ID do feed, o título normalizado, a data e o resumo. Uma restrição `UNIQUE` no SQLite ajuda a impedir inserções duplicadas.

### 5. Tratamento de falhas

Erros de rede, timeout, respostas HTTP inválidas e XML malformado são registrados individualmente por feed e incrementam o contador `failure_count`.

Essas falhas não encerram o servidor nem interrompem a atualização das demais fontes.

## 🧪 Testes automatizados

O projeto inclui testes unitários para verificar:

- Interpretação de feeds RSS 2.0.
- Interpretação de feeds Atom.
- Deduplicação de artigos com e sem link.
- Validação de URLs.
- Persistência de fontes, artigos e favoritos após fechar e reabrir o banco SQLite.

Os testes podem ser executados com o comando `cargo test`.

## ⚠️ Limitações conhecidas

- A atualização periódica funciona somente enquanto o processo do servidor estiver em execução.
- A posição das fontes no globo deve ser informada manualmente no cadastro.
- O globo representa a localização atribuída à fonte, não a localização real de cada notícia.
- O protótipo não possui geocodificação automática de artigos.
- Não há sincronização de dados entre dispositivos.
- A importação de listas de feeds no formato OPML ainda não está implementada.
- A API limita a resposta a 200 artigos por consulta.

## 🤖 Uso de inteligência artificial

Ferramentas de inteligência artificial foram utilizadas como apoio durante o desenvolvimento, auxiliando na estruturação do protótipo, na sugestão da separação dos módulos e na revisão da documentação.

Os integrantes são responsáveis por compreender, executar, testar, validar e adaptar o código desenvolvido, garantindo o funcionamento adequado da aplicação e o entendimento das tecnologias utilizadas.

## 👨‍💻 Desenvolvido por

**Samuel Dias Medeiros e Robert William**

Curso de Engenharia da Computação — 6º período  
Faculdade AEMS  
Três Lagoas — Mato Grosso do Sul

---

*Projeto acadêmico desenvolvido como protótipo de agregador de notícias utilizando Rust e tecnologias web.*
