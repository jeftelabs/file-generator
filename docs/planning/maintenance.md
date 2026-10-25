# Planejamento e manutenção

## Obrigação de documentação

Toda criação ou mudança do projeto exige criar ou atualizar documentação detalhada na mesma tarefa, incluindo código, API, testes, fixtures, dependências, configuração, scripts, CI, build, exemplos e os próprios documentos. Registre cada página no índice `docs/README.md`; atualize também o README quando comportamento visível ao consumidor mudar.

## Checklist de mudança

1. Consulte `AGENTS.md`, este índice e as páginas relacionadas.
2. Confirme o contrato atual no gerador e no teste relevante.
3. Atualize comportamento e testes com foco no formato, tamanho, nome, MIME e conteúdo inicial.
4. Se alterar formato ou fixture, confirme que a identificação ainda corresponde ao MIME e revise tamanho mínimo.
5. Se alterar o fluxo de assets, confirme que o pacote publicado inclui os recursos necessários em runtime.
6. Atualize guia, API, arquitetura e README conforme impacto; evite páginas sem link no índice.
7. Rode verificações apropriadas e informe resultados; use `npm run build` ao mudar o fluxo de distribuição de fixtures.
8. Revise exemplos, links e `git diff`.

## Áreas sensíveis

- **Tamanho:** entradas numéricas e expressões textuais têm caminhos diferentes; o tamanho final pode ser maior que o pedido pelo limite da fixture.
- **Memória:** gerar um Buffer aloca a saída completa no processo.
- **Fixtures:** bins reconhecíveis não garantem integridade de arquivo completo; mantenha a documentação explícita sobre isso.
- **Build e publicação:** os geradores binários leem os `fake.*` junto do módulo, portanto assets fazem parte do pacote de runtime.
- **Nomes e MIME:** cada função declara o MIME e extensão esperados; não derive MIME apenas do nome fornecido.
- **CSV:** a amostra não cresce semanticamente com o tamanho; cresce apenas o buffer.

## Não oferecido atualmente

O projeto não fornece APIs para gravar em disco, gerar conteúdo semântico arbitrário, criar arquivos plenamente editáveis, validar uploads HTTP ou receber/devolver streams. Não prometa esses comportamentos sem implementação e testes específicos.
