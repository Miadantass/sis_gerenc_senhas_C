# Sistema de Gerenciamento de Senhas em C

## Identificação do estudante

- **Nome:** Maria Eduarda Dantas
- **Instituição:** UDF
- **Disciplina/Projeto:** Programação em C 
# Sistema de Gerenciamento de Senhas — Clínica de Especialidades Médicas

> Programa de linha de comando em C, com menu interativo e painel de atendimento,
> para emissão e chamada priorizada de senhas em uma clínica de especialidades médicas.

---

## Índice

1. [Descrição do projeto](#descrição-do-projeto)
2. [Objetivo](#objetivo)
3. [Funcionalidades implementadas](#funcionalidades-implementadas)
4. [Funções e regras de negócio desenvolvidas](#funções-e-regras-de-negócio-desenvolvidas)
5. [Bibliotecas utilizadas](#bibliotecas-utilizadas)
6. [Organização do código](#organização-do-código)
7. [Conceitos de programação aplicados](#conceitos-de-programação-aplicados)
8. [Tratamento de erros](#tratamento-de-erros)
9. [Instruções de compilação e execução](#instruções-de-compilação-e-execução)
10. [Exemplos de uso](#exemplos-de-uso)
11. [Identificação da estudante](#identificação-da-estudante)

---

## Descrição do projeto

Este projeto é um **sistema de gerenciamento de senhas em linguagem C** para
uma **clínica de especialidades médicas**, executado via terminal. Ao rodar o
programa, o usuário visualiza um painel com a próxima senha a ser atendida e um
menu com as opções `01 - Retirar senha` e `02 - Registrar atendimento`.

Na retirada de senha, o sistema coleta nome, idade, gênero (perguntando se há
gestação quando o gênero for feminino) e se há necessidade especial, gerando o
código de espera correspondente (`U001`, `P001`, `N001` etc.). O painel
atualiza constantemente a próxima senha a ser atendida conforme a ordem de
prioridade.

## Objetivo

O objetivo do projeto é aplicar, em um sistema prático de fila e atendimento,
os principais fundamentos da linguagem C trabalhados em sala:

- Uso de **variáveis**, **tipos de dados** (`int`, `char`) e **estruturas** (`struct`);
- **Entrada e saída de dados** formatada e segura (`fgets`, `scanf`, `printf`, `snprintf`);
- **Estruturas condicionais** (`if...else`);
- **Estruturas de repetição** (`while`, `do...while`);
- **Funções** para controle de fila circular, classificação de prioridade, leitura validada e exibição do painel;
- **Tratamento de erros**, incluindo filas vazias ou cheias, opções inválidas e entradas malformatadas.

## Funcionalidades implementadas

- Painel contínuo exibindo a próxima senha a ser atendida e o quantitativo aguardando por categoria (`U`, `P` e `N`).
- Opção `01 - Retirar senha` com coleta de dados do paciente e emissão de código sequencial por categoria (`U001`, `P001`, `N001`).
- Classificação automática de prioridade:
  - **U (Urgente):** pacientes com mais de 80 anos.
  - **P (Preferencial):** gestantes (gênero feminino) e pacientes com necessidade especial.
  - **N (Normal):** demais pacientes.
- Opção `02 - Registrar atendimento` que chama/registra a senha de maior prioridade na fila e avança o painel para a próxima.
- Validação de entradas para evitar travamentos com dados inválidos ou campos vazios.

## Funções e regras de negócio desenvolvidas

| Função / Regra no código | Responsabilidade |
|---|---|
| `inicializar_fila` | Inicializa índices, contador, capacidade e prefixo (`U`, `P` ou `N`) de cada fila |
| `fila_vazia` / `fila_cheia` | Verifica o estado atual de ocupação da fila circular |
| `limpar_buffer` / `ler_texto` | Lê strings com segurança e limpa caracteres residuais do buffer de entrada |
| `ler_inteiro` / `ler_genero` / `ler_sim_nao` | Valida e normaliza entradas numéricas, de gênero e respostas `S/N` |
| `definir_prioridade` | Classifica o paciente em `U` (> 80 anos), `P` (gestante ou necessidade especial) ou `N` |
| `enfileirar` / `desenfileirar` | Insere no fim e remove do início da fila circular, formatando códigos como `N001` |
| `obter_proxima_fila` | Seleciona a próxima fila a ser chamada seguindo a ordem de prioridade `U` > `P` > `N` |
| `exibir_painel` | Mostra a senha que será atendida a seguir, dados do paciente e totais por fila |
| `retirar_senha` | Executa a opção `01`, cadastra o paciente e emite a senha |
| `registrar_atendimento` | Executa a opção `02`, registra o atendimento da senha atual e atualiza a fila |

Todas essas funções estão reunidas no código-fonte em C e orquestradas pelo
laço principal em `main()`.

## Bibliotecas utilizadas

| Biblioteca | Uso no projeto |
|---|---|
| `stdio.h` | Entrada e saída padrão: `printf()`, `scanf()`, `fgets()`, `getchar()` e `snprintf()` para formatação dos códigos de senha (`%c%03d`). |
| `stdlib.h` | Utilitários padrão da linguagem C e suporte geral de execução do programa. |
| `string.h` | Manipulação de strings e memória: `strlen()`, `memset()` e cópia/leitura segura de campos textuais. |
| `ctype.h` | Conversão de caracteres com `toupper()` para padronizar respostas de gênero (`F`, `M`, `O`) e confirmação (`S`, `N`). |

## Organização do código

O projeto está estruturado em torno de estruturas de dados (`Paciente` e
`FilaSenhas`) e funções organizadas por responsabilidade:
