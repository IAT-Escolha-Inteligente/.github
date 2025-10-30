
# GIT Guideline

Padronizar o Git gira em torno de duas coisas principais:

1. Um Modelo de Ramificação (Branching Model): Como e quando as branches são criadas, nomeadas e mescladas.

2. Convenções de Commits: Como as mensagens de commit são escritas.

Dessa forma um guia prático para a equipe é uma versão simplificada e popular do GitHub Flow, combinada com Conventional Commits



## 1 - O Modelo de Ramificação (Branching Model)

O objetivo é manter a branch main (ou master) sempre estável e pronta para um "deploy" ou "release". Todo trabalho novo é feito em branches separadas.

**Regras de Ouro**:
A branch main é sagrada: Ninguém deve enviar commits (fazer git push) diretamente para a main. Todo código só entra na main através de um Pull Request (ou Merge Request no GitLab).

Crie branches a partir da main: **Todo novo trabalho** (uma nova funcionalidade, correção de bug, etc.) começa como uma **nova branch** baseada na versão mais atual da main.

Pull Requests (PRs) são obrigatórios: Quando o trabalho na sua branch estiver concluído, você abre um Pull Request para mesclar sua branch de volta à main.

Exija Revisão: O Pull Request deve ser revisado por pelo menos um outro membro da equipe. Isso é crucial para "code review", encontrar bugs e compartilhar conhecimento.

Mescle e Exclua: Após a aprovação (e idealmente, após a passagem de testes automatizados), o PR é mesclado na main e a branch de trabalho é excluída (recomendado, mas não obrigatório).


## 2 - Convenções de Nomenclatura (Branches)

Para manter a organização, os nomes das branches devem ser padronizados. Uma prática comum é usar prefixos:

Funcionalidade nova: ```<dev>/feature/<nome-da-funcionalidade>``` (ex: ```rafael/feature/login-com-google```)

Correção de bug: ```<dev>/fix/<nome-da-correcao>``` (ex: ```rafael/fix/calculo-imposto-incorreto```)

Melhoria/Refatoração: ```<dev>/refactor/<o-que-foi-refatorado>``` (ex: ```rafael/refactor/otimizar-consulta-sql```)

Documentação: ```<dev>/docs/<documentacao-adicionada>``` (ex: ```rafael/docs/atualizar-readme-api```)

Tarefas (ex: CI/CD): ```<dev>/chore/<nome-da-tarefa>``` (ex: ```rafael/chore/configurar-github-actions```)

Por quê? Fica óbvio para todos o que está acontecendo em cada branch apenas lendo o nome.



## 3 - O Padrão de Commits (Conventional Commits)

Este é o "padrão de ouro" da indústria. Ele força mensagens de commit claras e permite a automação (como gerar um ```CHANGELOG``` automaticamente).

A estrutura é: ```tipo(escopo): descrição curta```

* tipo: Define o que o commit faz. Os mais comuns são:

    * ```feat```: Uma nova funcionalidade (feature).

    * ```fix```: Uma correção de bug.

    * ```docs```: Mudanças apenas na documentação.

    * ```style```: Mudanças de formatação (ponto e vírgula, espaços em branco), sem alterar a lógica.

    * ```refactor```: Refatoração de código (não corrige bug nem adiciona feature).

    * ```test```: Adicionando ou corrigindo testes.

    * ```chore```: Atualizações de build, configuração, etc. (ex: chore: atualiza dependencias)

* ```(escopo)``` (Opcional): Onde a mudança ocorreu (ex: feat(api): ..., fix(login): ...).

* ```descrição```: Uma breve descrição do que foi feito, em letra minúscula e no imperativo.

Exemplos de bons commits curtos:

* ```feat: adicionar login com google```

* ```fix(pagamentos): corrigir erro de arredondamento no total```

* ```docs: atualizar manual de instalação do ROS```

* ```refactor(controle-motor): simplificar lógica de controle de torque```

* ```test: adicionar testes de unidade para o parser de dados```

* ```chore: adicionar lint ao pipeline de CI```


Exemplos de um bom commit completo (com corpo e rodapé):
```plaintext
feat(api): implementar autenticação JWT

O sistema agora usa JSON Web Tokens para autenticação de endpoints.
Isso substitui o sistema antigo de sessão baseado em cookies.

BREAKING CHANGE: Todos os clientes da API devem agora enviar um
token "Authorization: Bearer <token>" no cabeçalho. Os endpoints
antigos de /login (baseado em sessão) foram desativados.

Closes #123
```

* É composto por descrição curta: ```feat(api): implementar autenticação JWT```

* Por descrição completa: ```O sistema agora usa JSON Web Tokens para autenticação de endpoints.
Isso substitui o sistema antigo de sessão baseado em cookies.```

* Nota explicativa/alerta: ```BREAKING CHANGE: Todos os clientes da API devem agora enviar um
token "Authorization: Bearer <token>" no cabeçalho. Os endpoints
antigos de /login (baseado em sessão) foram desativados.```

* Ação da ```issue``` que a ```branch``` está relacionada seguida do número da ```issue```: ```Closes #123```


Por quê? O histórico do ```git log``` se torna um documento legível que conta a história do projeto, facilitando a depuração e a geração de notas de release.



## 4 - Resumo: O Fluxo de Trabalho Diário da Equipe
Aqui está um "Guia Rápido" que você pode compartilhar com a equipe:

1. **Sincronize**: Antes de começar, atualize sua branch main local:

```Bash
git checkout main
git pull origin main
```


2. **Crie sua Branch**: Crie uma nova branch para sua tarefa usando o padrão de nomenclatura:

```Bash
git checkout -b de/feature/minha-nova-funcionalidade
```


3. **Trabalhe e Commite**: Faça seu trabalho. Faça commits pequenos e claros usando o padrão Conventional Commits:

```Bash
git add .
git commit -m "feat(novo-modulo): adicionar lógica inicial"
# ... trabalha mais ...
git add .
git commit -m "refactor(novo-modulo): mover lógica para um serviço"
```



4. Envie: Envie sua branch para o repositório remoto:

```Bash
git push -u origin dev/feature/minha-nova-funcionalidade
```



5. **Abra o Pull Request (PR)**: Vá para o GitHub/GitLab/Bitbucket e abra um Pull Request da sua branch para a ```main```.

* Escreva uma boa descrição do PR, explicando o que foi feito e por que.
* Marque os membros da equipe para revisarem.

6. Revise e Discuta: A equipe revisa o código, sugere mudanças e discute. Faça os ajustes necessários (novos commits) na sua branch (eles aparecerão automaticamente no PR).

7. Mescle (Merge): Após a aprovação, clique em "Merge" (geralmente usando a opção "Squash and Merge" se você quiser agrupar todos os seus commits em um só na main, ou "Rebase and Merge" para um histórico linear).

8. Limpe: Exclua a branch (geralmente há um botão para isso após o merge).




## 5 - Ferramentas para Automatizar a Padronização

Para garantir que todos sigam as regras:

1. **Branch Protection Rules (GitHub/GitLab)**: Configure a branch main para:

    * **Exigir Pull Requests** (proibir push direto).

    * **Exigir aprovações** (ex: 1 ou 2 revisores).

    * **Exigir que os testes de CI passem** antes de permitir o merge.

2. **Linters e Formatters**: Use ferramentas como ESLint, Prettier, Black (para Python) com Git Hooks (husky, por exemplo) para formatar o código automaticamente antes do commit.

3. **Commitlint**: Uma ferramenta que pode ser adicionada ao seu projeto para verificar se as mensagens de commit seguem o padrão Conventional Commits.

Começar com os passos 1, 2 e 3 já colocará sua equipe em um nível muito mais profissional e organizado.


