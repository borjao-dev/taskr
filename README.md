# Listas

App de listas hierárquicas (níveis ilimitados) com prioridade, datas, busca e
sincronização entre dispositivos via GitHub Gist. Um único arquivo HTML,
sem build, sem dependências — pronto para GitHub Pages.

## Publicar no GitHub Pages

1. Crie um repositório novo (pode ser privado) e suba `index.html` e `data.json`
   na raiz (ou numa pasta `docs/`).
2. Nas configurações do repo: **Settings → Pages → Source**, selecione a
   branch e a pasta onde estão os arquivos.
3. Acesse a URL gerada (algo como `https://SEUUSER.github.io/SEUREPO/`).

`data.json` é usado só como **carga inicial** na primeira vez que você abre o
app num navegador sem dados salvos ainda (é a conversão da sua lista atual).
Depois disso tudo passa a viver no `localStorage` do navegador e, se você
configurar sync, no Gist.

## Sincronização entre PC e celular (Gist)

GitHub Pages é 100% estático — não existe "salvar no servidor" nele. A forma
de ter os dados sincronizados entre aparelhos sem precisar de um backend
próprio é usando um **Gist privado** como armazenamento, acessado via API do
GitHub direto do navegador.

Passo a passo:

1. Clique no indicador **"local"** no canto superior direito do app.
2. Clique no link para criar um **Personal Access Token** — já vem
   pré-configurado com o escopo mínimo necessário: **apenas `gist`**
   (não dá acesso a repositórios nem código).
3. Cole o token no campo indicado.
4. Deixe o campo "ID do Gist" em branco na primeira vez — o app cria um Gist
   privado novo automaticamente e salva o ID.
5. Em outro dispositivo (ex: celular), repita o processo, mas dessa vez
   **cole o ID do Gist** que foi criado no PC, usando o mesmo token (ou um
   novo token, tanto faz, desde que aponte pro mesmo Gist).

A partir daí, toda alteração é salva automaticamente no Gist (com um pequeno
atraso de ~1s para evitar salvar a cada tecla digitada), e ao abrir o app ele
sempre puxa a versão mais recente do Gist.

**Sobre segurança:** o token fica salvo em texto no `localStorage` do
navegador — não é criptografado. Por isso o escopo `gist` isolado é
importante: mesmo que alguém tenha acesso ao seu navegador, o token não dá
acesso a mais nada da sua conta GitHub. Se desconfiar de vazamento, revogue o
token em `github.com/settings/tokens` e crie outro.

**Limitação:** como não há backend real, se dois dispositivos editarem ao
mesmo tempo sem sincronizar entre si, o último a salvar sobrescreve o Gist
inteiro (padrão "last-write-wins": a última escrita sobrescreve a anterior). Para o seu uso (um usuário, poucos dispositivos)
isso raramente é problema — só evite editar em dois lugares simultaneamente.

## Funcionalidades

- **Hierarquia ilimitada**: qualquer item pode ter subitens, sem limite de
  profundidade.
- **Prioridade** (independente do nível): urgente / alta / média / baixa,
  indicada por uma barra colorida à esquerda do item. Pode ser aplicada a
  qualquer item em qualquer nível.
- **Checkbox em cada item**: marcar um item pai marca todos os filhos
  automaticamente.
- **Progresso por seção**: contador `feito/total` e barra visual no
  cabeçalho de cada seção raiz.
- **Datas**: campo opcional de vencimento por item, com destaque visual
  quando vencido (vermelho) ou próximo (amarelo, ≤2 dias).
- **Busca e filtros**: por texto, por prioridade, e alternância
  pendentes/tudo.
- **Colapsar/expandir**: por seção e por item individual (a seta aparece só
  em itens que têm filhos); botão "Recolher tudo" no topo.
- **Edição inline**: clique em qualquer texto para editar; Enter confirma,
  Esc cancela.
- **Arrastar e soltar**: reordena itens dentro da lista.
- **Exclusão em cascata**: apagar um item com filhos pede confirmação.

## Estrutura do JSON

```json
{
  "version": 1,
  "items": [
    {
      "id": "abc123",
      "title": "Nome da seção",
      "checked": false,
      "priority": null,
      "due": null,
      "children": [ /* mesma estrutura, recursiva */ ]
    }
  ]
}
```

## Dados iniciais

O `data.json` incluído já é a conversão da sua lista `Main_list` original
(309 itens, 240 já marcados como concluídos), preservando toda a hierarquia
de seções e subitens.
