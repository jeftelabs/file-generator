# @jeftelabs/file-generator

[![npm version](https://img.shields.io/npm/v/@jeftelabs/file-generator?style=for-the-badge&logo=npm)](https://www.npmjs.com/package/@jeftelabs/file-generator)
[![license](https://img.shields.io/npm/l/@jeftelabs/file-generator?style=for-the-badge)](./LICENSE)

Gere buffers de arquivos fictícios em memória para testar uploads, limites de tamanho e validação inicial de tipo no Node.js. Os geradores retornam conteúdo, tamanho, nome e MIME sem gravar os arquivos gerados no disco.

## Instalação

```sh
npm install @jeftelabs/file-generator
```

O pacote é ESM e requer Node.js 18.16.0 ou superior.

## Uso

```ts
import fileGenerator from '@jeftelabs/file-generator'

const file = await fileGenerator.generatePng('1mb', 'avatar.png')

console.log(file.name) // avatar.png
console.log(file.mime) // image/png
console.log(file.size) // 1048576 bytes
console.log(file.contents) // Buffer
```

Todos os métodos são assíncronos e retornam `Promise<{ contents: Buffer; size: number; name: string; mime: string }>`. O segundo argumento opcional define o nome; sem ele, o pacote cria um nome com UUID e extensão correspondente.

## Formatos disponíveis

| Método | Extensão padrão | MIME |
| --- | --- | --- |
| `generateCsv(size, name?)` | `.csv` | `text/csv` |
| `generateDocx(size, name?)` | `.docx` | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` |
| `generateGif(size, name?)` | `.gif` | `image/gif` |
| `generateJpg(size, name?)` | `.jpg` | `image/jpeg` |
| `generatePdf(size, name?)` | `.pdf` | `application/pdf` |
| `generatePng(size, name?)` | `.png` | `image/png` |
| `generateXlsx(size, name?)` | `.xlsx` | `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` |

## Tamanho

O primeiro argumento aceita um número de bytes ou uma string reconhecida pela dependência `bytes`, como `'300kb'` ou `'1mb'`:

```ts
const small = await fileGenerator.generateCsv(512_000)
const large = await fileGenerator.generatePdf('5mb')
```

A saída nunca é menor que os bytes iniciais do formato: se o tamanho solicitado for menor que a fixture, o buffer mantém o tamanho mínimo da fixture. Para um valor numérico, `size` corresponde ao comprimento final do `contents` em bytes.

## Testar uma requisição multipart

Passe `contents`, `name`, `mime` e `size` ao cliente HTTP ou à biblioteca multipart que seu teste usa. Por exemplo, com uma implementação FormData compatível com Node:

```ts
const file = await fileGenerator.generatePng('1mb', 'avatar.png')
const form = new FormData()
form.append('avatar', new Blob([file.contents], { type: file.mime }), file.name)
```

A integração exata depende do cliente HTTP e da implementação de `FormData` adotados pela aplicação.

## Limitações

Os formatos binários usam fixtures curtas para fornecer bytes iniciais reconhecíveis por ferramentas de detecção de tipo. Para alcançar tamanhos maiores, os bytes da fixture são repetidos no buffer. Isso serve para testes de upload e sniffing inicial, mas não torna o resultado uma imagem/documento completo para abrir, editar, renderizar ou analisar seu conteúdo. O CSV também contém somente uma pequena amostra repetida quando ampliado.

O código não grava os buffers gerados no sistema de arquivos. Não use os resultados como arquivos de produção nem para validar leitores completos de PDF, planilhas, documentos ou imagens.

## Desenvolvimento

```sh
npm install
npm test
npm run typecheck
npm run build
```

`npm test` executa o lint pelo lifecycle `pretest` e, em seguida, os testes com cobertura. `npm run build` compila o pacote e copia as fixtures para `build/`, que são necessárias em runtime para os geradores binários.

## Documentação

Consulte [`docs/README.md`](./docs/README.md) para a documentação detalhada de uso, API, arquitetura, testes, build e manutenção, escrita para pessoas e agentes de IA.
