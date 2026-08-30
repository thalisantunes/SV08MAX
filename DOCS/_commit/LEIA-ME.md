# Como fechar o commit (5 comandos, no Windows)

Os dois commits já estão escritos e testados num clone paralelo, mas o **push
não sai daqui**: o shell da ponte roda numa VM Linux e não enxerga o gerenciador
de credenciais do Windows.

A árvore de trabalho em `C:\Temp\SOVOL-SV08MAX` **já está com todo o conteúdo**
— inclusive o `buffer_stepper.cfg` corrigido. Falta só registrar.

## Antes: apagar o lock órfão

Um `git status` meu estourou o tempo (a ponte é lenta com 7.249 arquivos) e
deixou um `index.lock` para trás. A pasta montada não me deixa apagar arquivo,
mas para você é trivial:

```
cd C:\Temp\SOVOL-SV08MAX
del .git\index.lock
```

## Os commits

```
git add DOCS DEMON .gitignore
git commit -F DOCS\_commit\01-docs.txt

git add home/sovol/printer_data/config/buffer_stepper.cfg
git commit -F DOCS\_commit\02-buffer.txt

git push
```

No Windows isso roda rápido — a lentidão era da ponte, não da sua máquina.

## Plano B

Se preferir não recriar os commits, o `riser-e-buffer.bundle` tem os dois
prontos, com autoria e mensagens:

```
git fetch DOCS\_commit\riser-e-buffer.bundle HEAD:refs/heads/tmp
git reset --hard tmp
git branch -d tmp
git push
```

O `reset --hard` é seguro aqui: o conteúdo dos commits é idêntico ao que já está
na pasta, e `_work/` e `Referências/` estão no .gitignore, então não são tocados.

## Depois de commitar

Esta pasta `DOCS/_commit/` pode ser apagada — ela só existe para esta entrega.
