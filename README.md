# whatsapp-ci

Executa os testes do repositório privado `devjotaduo/whatsapp` em runners hospedados pelo GitHub, gratuitos em repositório público. Criado em 15/11/2026, quando o runner próprio do `oracle-sp` ficou fora.

O workflow `Vigia` olha a origem a cada 10 minutos com a chave de leitura (`git ls-remote`) e dispara o `Tests` para cada commit novo da `main` ou de pull request; os commits já vistos ficam em `vistos.txt` na branch `estado`. É o caminho que funciona hoje, porque a conta não tem minutos de runner hospedado nos repositórios privados. O `Tests` também roda por um push em `run/pr-<número>-<commit>-<execução>-<tentativa>` (testa o merge do pull request) ou `run/sha-<commit>-<execução>-<tentativa>` (um commit da `main`), feito pelo `ci-hospedado.yml` da origem com a chave de deploy `CI_TRIGGER_KEY`; apaga a branch ao terminar. Também roda à mão:

```bash
gh workflow run tests.yaml --repo devjotaduo/whatsapp-ci -f ref=<branch>
```

O código é lido com uma chave só de leitura guardada no secret `SOURCE_DEPLOY_KEY`. Os logs das execuções são públicos: uma falha de typecheck, lint ou teste imprime trechos do código da origem.
