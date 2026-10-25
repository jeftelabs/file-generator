# Desenvolvimento e ferramentas

## Requisitos

O `package.json` declara Node.js `>=18.16.0`. O pacote usa TypeScript ESM, `bytes` para interpretar tamanhos textuais e fixtures copiadas ao build. Instale dependências locais com `npm install`; em CI use `npm ci`.

## Scripts npm

| Script | Função |
| --- | --- |
| `npm run lint` | Executa ESLint. |
| `npm test` | Roda lint pelo `pretest` e testes com cobertura `c8`. |
| `npm run quick:test` | Executa `bin/test.ts` com `@jeftelabs/exec`, sem wrapper de cobertura. |
| `npm run typecheck` | Executa `tsc --noEmit`. |
| `npm run build` | Linta, limpa, compila ESM e declarações, e copia fixtures ao `build/`. |
| `npm run compile` | Executa etapas de compilação; lifecycle `precompile`/`postcompile` também é aplicado pelo npm. |
| `npm run copy:files` | Copia os arquivos-modelo para `build/`. |
| `npm run clean` | Remove `build/`. |
| `npm run format` | Aplica Prettier ao repositório. Revise o diff depois. |

## Testes

`bin/test.ts` seleciona `tests/**/*.spec.ts` e ativa o plugin de asserção. Testes dos seis formatos baseados em fixtures comparam MIME/tamanho e usam `file-type` para inspecionar o buffer. Os testes CSV verificam MIME, tamanho e nome; `utils.spec.ts` verifica string de tamanho, bytes e erro de parse. Cobertura não substitui a validação de que documentos completos podem ser abertos; essa integridade não é objetivo das fixtures.

## Fixtures e build

As fixtures em `src/files/*/fake.*` são fontes necessárias em runtime. `copy:files` usa `copyfiles --flat` para colocá-las diretamente em `build/`, que é incluído no pacote pelo campo `files`. Não remova nem renomeie fixtures sem atualizar `package.json`, geradores, testes e documentação.

## CI e publicação

`.github/workflows/checks.yml` configura checagem de mensagens Conventional Commit. `.github/workflows/release.yml` cria releases via release-please, constrói e empacota; `deploy.yml` publica no GitHub Packages e npm em eventos de release. Confirme a configuração atual e os segredos exigidos nos workflows antes de alterar publicação.
