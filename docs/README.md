# Documentação de @jeftelabs/file-generator

A documentação descreve a implementação atual para pessoas e agentes de IA. Para comportamento exato, consulte os geradores e testes; mantenha estas páginas sincronizadas com qualquer mudança.

## Navegação

- [Guia de uso](./guides/usage.md): instalação, geração, envio multipart, tamanhos e limites.
- [Referência da API](./references/api.md): métodos públicos, argumentos e shape do retorno.
- [Referência de desenvolvimento](./references/development.md): scripts, testes, build, fixtures e workflows.
- [Arquitetura e fluxo](./architecture/overview.md): módulos, criação dos buffers e distribuição das fixtures.
- [Planejamento e manutenção](./planning/maintenance.md): checklist de alterações e áreas sensíveis.

## Mapa do projeto

| Caminho | Responsabilidade |
| --- | --- |
| `index.ts` | Export default do objeto de geradores. |
| `src/files/<format>/generate.ts` | Implementa cada método público. |
| `src/files/<format>/fake.*` | Fixture inicial para o buffer do formato. |
| `src/files/utils.ts` | Converte tamanhos legíveis, preenche buffer e resolve diretório de fixture. |
| `tests/*.spec.ts` | Testa metadados, tamanho, fixture e sniffing de tipo. |
| `bin/test.ts` | Configura e executa o runner de testes. |
| `package.json` | Export, dependências, scripts e configuração de build. |
| `tsconfig.json` | Configura saída TypeScript e declarações em `build/`. |
| `.github/workflows/` | Define validação de commits, release e publicação. |

## Manutenção deste índice

Liste aqui cada página nova em `docs/`, com uma descrição curta. Ao mudar API, exemplos ou limitações, atualize a página correspondente e o README.
