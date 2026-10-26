---
name: contrato
description: Gera contrato de prestação de serviço em HTML pronto pra virar PDF, com cláusulas numeradas em português (CLÁUSULA PRIMEIRA, SEGUNDA...), preâmbulo das partes, parcelas automáticas na cláusula de honorários e assinatura. Vai salvando as cláusulas descritas como catálogo reutilizável pros próximos contratos. Use quando o usuário pedir pra montar contrato, "faz o contrato pro cliente X", "gera o contrato desse projeto", ou quiser editar um contrato já gerado.
---

# Contrato

Gera um contrato de prestação de serviço em HTML, pronto pra virar PDF (Ctrl+P ou PDF
automático), com cláusulas numeradas por extenso em português, igual a um contrato formal
assinado entre duas partes. Mesmo princípio da skill `proposta-comercial`: documento
preto e branco, só a logo carrega a identidade visual, nada de cor nem card.

## Fase 0 — Logo e dados do responsável (só na primeira vez, ou se mudar)

O documento é sempre texto preto em fundo branco, igual qualquer contrato formal. A
identidade do usuário entra só pela logo no topo, nada de cor nem fonte de marca.

- Procurar a pasta de marca (`Marca/`, `marca/`, `brand/`, `identidade/`) e achar o arquivo
  de logo. Abrir pra conferir se é a versão clara, que funciona em fundo branco
- Conferir se o arquivo tem fundo transparente (PNG com alpha). Se tiver fundo sólido,
  avisar o usuário e oferecer: remover o fundo com alguma skill de imagem já instalada, se
  existir; seguir assim mesmo se o fundo já for branco puro; ou avisar que vai aparecer uma
  caixa ao redor da logo e perguntar se segue ou se ele manda outro arquivo
- Ler `_contexto/empresa.md` (ou `AGENTS.md`/`CLAUDE.md`) pra puxar nome do responsável
  técnico ou legal, CPF ou CNPJ, registro profissional (CREA, CAU, CFT ou equivalente) e
  nome do negócio, endereço, se precisar pro preâmbulo
- Mostrar o que encontrou e perguntar só o que faltou. Nunca escrever nada antes de
  confirmar

## Fase 1 — Cláusulas do contrato

- Perguntar quais cláusulas o usuário costuma usar num contrato desse tipo, pra ele
  descrever com as próprias palavras: objeto, prazo, forma de pagamento, obrigações de cada
  parte, rescisão, propriedade do material entregue, confidencialidade, foro, qualquer
  outra que for comum no tipo de serviço dele
- Checar se já existe `contratos/catalogo-clausulas.md` na pasta do projeto
  - Existe: mostrar as cláusulas já salvas e perguntar quais o usuário quer usar nesse
    contrato, complementando com o que for novo
  - Não existe: criar o arquivo com o que o usuário descreveu agora, uma cláusula por
    bloco, com um título curto pra cada
- Toda cláusula nova que aparecer num contrato entra no catálogo também. O catálogo cresce
  sozinho com o uso, nunca precisa ser montado na mão antes de começar a usar a skill
- Na hora de escolher quais cláusulas entram nesse contrato específico, oferecer a escolha
  como lista numerada em texto (o usuário digita os números separados por vírgula). Se o
  ambiente onde a skill está rodando tiver algum recurso nativo de escolha múltipla (uma
  caixa de seleção, não só texto), pode usar no lugar da lista, mas a lista numerada é
  sempre a opção que tem que funcionar em qualquer ambiente, inclusive sem esse recurso

## Fase 2 — Dados do contrato

Perguntar numa mensagem só:

1. Número do contrato (se esse contrato vier de uma proposta já gerada pela skill
   `proposta-comercial`, sugerir o mesmo número dela; senão, olhar o maior número já usado
   em `contratos/` e sugerir o próximo)
2. As duas partes: nome, CPF/CNPJ e endereço do contratante (cliente) e do contratado (o
   próprio usuário, já puxado na Fase 0 se possível)
3. Objeto do contrato (o que está sendo contratado, pode vir de uma proposta já aprovada)
4. Valor total e forma de pagamento: quantas parcelas, valor ou percentual de cada uma, e o
   gatilho de cada parcela (assinatura, entrega, data fixa)
5. Prazo de execução: data fixa, ou "X dias a partir da assinatura" (nesse caso a data
   final é calculada na hora, porque é a própria assinatura que dispara a contagem)
6. Foro (cidade/comarca que resolve qualquer disputa), se não tiver no catálogo

Regra dura: nunca inventar valor, prazo, parte ou condição. O que não for informado entra
no documento como `[a confirmar]`, visível.

## Fase 3 — Esqueleto antes de gerar

Mostrar em bullets a lista de cláusulas escolhidas, na ordem que vão entrar, com a frase
central de cada uma, e esperar aprovação do usuário antes de montar o HTML.

## Fase 4 — Gerar o documento

Estrutura fixa, igual a um contrato formal:

```
CONTRATO DE PRESTAÇÃO DE SERVIÇO Nº {número}/{ano com 2 dígitos}

Pelo presente instrumento particular, de um lado {nome do contratado}, {CPF/CNPJ},
{registro profissional, se houver}, doravante denominado CONTRATADO, e de outro lado
{nome do contratante}, {CPF/CNPJ}, {endereço}, doravante denominado CONTRATANTE, têm
entre si justo e contratado o que segue:

CLÁUSULA PRIMEIRA - {TÍTULO DA CLÁUSULA EM MAIÚSCULA}
{texto da cláusula}

CLÁUSULA SEGUNDA - {TÍTULO}
{texto da cláusula}

[... uma cláusula por bloco escolhido, numeração em português sempre em ordem,
recalculada do zero toda vez que o conjunto de cláusulas mudar]

E por estarem assim justas e contratadas, as partes assinam o presente instrumento.

{Cidade/UF}, {data por extenso ou DD/MM/AAAA}.


_______________________________
{Nome do contratado}
{Registro profissional}
{Nome do negócio}


_______________________________
{Nome do contratante}
```

Regras de formatação do HTML (literais, não é sugestão de estilo):

- **Documento, não site.** Fundo branco (`#ffffff`), texto preto (`#000000`), uma fonte só,
  sem variação de cor em lugar nenhum do corpo. O único elemento visual fora do texto é a
  logo no topo
- **Proibido:** caixa ou retângulo colorido atrás de texto, barra ou borda lateral
  colorida, número de cláusula dentro de círculo ou badge, qualquer cor de destaque,
  fundo cinza ou de qualquer cor em bloco. Se o rascunho ficar parecendo o preview de uma
  landing page, está errado, refazer
- **Não consultar `design-guide.md` nem qualquer arquivo de marca pra cor ou fonte do
  corpo.** Serve só pra achar o caminho da logo na Fase 0
- Logo centralizado no topo de cada página, do jeito que o arquivo já é
- Título do contrato centralizado, negrito, preto, logo abaixo do logo
- Preâmbulo das partes em texto corrido, sem negrito
- Cada cabeçalho de cláusula (`CLÁUSULA PRIMEIRA - OBJETO`, `CLÁUSULA SEGUNDA - PRAZO`
  etc.) em negrito preto, numeração sempre por extenso em português (primeira, segunda,
  terceira... até trigésima, depois "trigésima primeira" e assim por diante), nunca em
  algarismo nem em círculo colorido
- Parcelas da cláusula de pagamento/honorários listadas uma por linha, texto normal
- Bloco de assinatura: duas linhas de assinatura (contratado e contratante), cada uma
  centralizada sob sua própria linha em branco, texto preto
- Linha de cidade e data antes das assinaturas, alinhada à esquerda ou centralizada (igual
  ao padrão de um contrato impresso), texto preto
- Página em formato A4 retrato (vertical), `@page { size: A4 portrait; }` no CSS, margem de
  impressão normal
- **Quebra de página por conteúdo, não por cláusula inteira.** Uma cláusula longa pode
  começar numa página e continuar na seguinte. O que não pode: cabeçalho de cláusula
  sozinho no fim da página (`break-after: avoid` nele), frase cortada no meio dentro de um
  item de lista (`break-inside: avoid` só no item, nunca na cláusula inteira), e o bloco de
  assinatura sempre inteiro, sem quebrar no meio
- Arquivo único, HTML autocontido (CSS inline), nomeado
  `contratos/Contrato nº {número}_{ano} - {Cliente}.html` (criar a pasta `contratos/` se
  não existir)

## Fase 5 — Entregar

- Abrir o arquivo HTML pro usuário conferir antes de gerar o PDF
- Depois que o usuário aprovar o HTML, gerar o PDF automaticamente, sem pedir pra ele
  apertar Ctrl+P na mão. Usar um navegador headless (ex. Playwright/Chromium, A4
  retrato/vertical, margens padrão) pra imprimir o próprio HTML direto em
  `contratos/Contrato nº {número}_{ano} - {Cliente}.pdf`, mesmo nome do HTML, só trocando
  a extensão
- Se não houver navegador headless disponível, avisar o usuário e perguntar se ele quer que
  a skill instale o que falta agora (ex. `npm install -D playwright` seguido de
  `npx playwright install chromium`). Só instalar depois do "sim" explícito. Se preferir
  não instalar, seguir com Ctrl+P (ou Cmd+P no Mac) como alternativa manual
- Confirmar no fim que o PDF foi gerado e onde está salvo

## Regras

1. Nunca inventar valor, prazo, parte ou cláusula. O que faltar fica `[a confirmar]`
2. O catálogo de cláusulas é vivo: pergunta, salva, reaproveita. Nunca pede pro usuário
   montar um catálogo do zero antes de poder usar a skill
3. Um contrato por cliente. Nada de reaproveitar contrato antigo só trocando o nome sem
   revisar cláusulas e valor
4. Numeração das cláusulas sempre recalculada do zero, em ordem, por extenso em português.
   Se uma cláusula for removida ou adicionada, renumerar todas, nunca deixar buraco
5. Seguir à risca a estrutura da Fase 4. É isso que dá cara de contrato de verdade, não de
   texto corrido
6. Abrir no navegador e olhar antes de entregar: logo claro em fundo claro não dá erro, só
   some. Se sumiu, usar a versão escura do logo ou o nome em texto
