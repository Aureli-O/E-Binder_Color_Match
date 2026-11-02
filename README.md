# Binder Color Match do Aurelis

Organize o seu fichário de cartas Pokémon por cor. Envie as imagens das cartas (fotos, uma foto com várias cartas ou busca em uma API) e o programa sugere a página e a posição de cada uma, para que as cartas vizinhas tenham cores parecidas.

É um **arquivo HTML único**, sem instalação, sem dependências e sem servidor.

---

## Recursos

**Adicionar cartas**
- Fotos avulsas (várias de uma vez) ou uma **foto com várias cartas**, dividida em grade ou por detecção automática.
- **Busca na TCGdex** (gratuita, sem chave, vários idiomas) para baixar a imagem oficial.
- Dá para adicionar quantas vezes quiser; as cartas se acumulam.

**Organizar**
- Páginas **3×3** (9 cartas) ou **spread 3×6** (18 cartas).
- **Organizar por cor**, **Refinar** e **Sugerir trocas** (com botão *Aplicar*).
- **Harmonia (ΔE)** no status: distância média entre cartas vizinhas (menor = mais parecidas).
- **Recalcular cores** refaz a paleta das cartas já adicionadas.

**Controle manual**
- **Arrastar e soltar** para trocar cartas, inclusive entre páginas.
- **Travar carta** (`L`) para que ela não saia da posição, útil para artes que se completam.
- **Remover carta** (`R`) envia para a **lixeira**, de onde pode ser restaurada.
- **Cadeado por página** mantém as cartas dela no lugar; **cor por página** reúne ali as cartas mais próximas da cor escolhida.

**Dados**
- Salvamento automático no navegador.
- **Exportar** gera um backup `.json` e uma imagem `.png` com todas as páginas; **Importar** restaura o backup.

---

## Como abrir

| | Link publicado no Claude | Arquivo HTML aberto no navegador |
|---|---|---|
| Fotos, organização, memória, exportar/importar | ✅ | ✅ |
| **Buscar cartas na API** | ❌ bloqueado | ✅ |

O link publicado bloqueia acesso a sites externos, então a busca só funciona no **arquivo HTML baixado**, aberto com duplo clique (Chrome, Edge, Firefox ou Safari atualizados).

> A memória é separada entre o link e o arquivo local. Para levar a coleção de um para o outro, use **Exportar** e depois **Importar**.

---

## Passo a passo

1. **Adicione as cartas**: `＋ Cartas`, `📷 Foto com várias cartas` (ajuste grade, margem e sensibilidade em *Ajustes da foto*) ou `🔎 Buscar cartas na API`.
2. **Escolha o tamanho da página** e clique em **Organizar por cor**.
3. **Ajuste do seu jeito**: arraste, trave, remova, escolha cor e cadeado por página.
4. **Refine** e confira a harmonia no status.
5. **Exporte** quando terminar.

A caixa **Organizar ao adicionar** reorganiza tudo a cada adição, respeitando o que estiver travado. Desmarcada, as cartas novas entram no final sem mexer nas páginas atuais.

**Dicas para fotos com várias cartas:** use fundo liso, cartas afastadas e foto de cima. Se a detecção automática errar, use *Dividir em grade*.

---

## Atalhos

| Ação | Como |
|---|---|
| Trocar duas cartas | Arrastar uma sobre a outra |
| Selecionar uma carta | Clicar nela |
| Travar / destravar a selecionada | `L` (ou botão 🔒) |
| Remover a selecionada | `R` (ou botão 🗑️) |
| Restaurar da lixeira | Clicar na miniatura |
| Fechar janelas | `Esc` |

Cartas travadas, e as de uma página travada, não podem ser movidas nem removidas até serem destravadas.

---

## Como a combinação de cores funciona

1. **Paleta.** Cada imagem é convertida para **CIELAB**, sem a borda e sem preto/branco puros, e um **k-means ponderado** (até 4 cores) gera a paleta, dando mais peso às cores vivas.
2. **Distância.** Compara as paletas de duas cartas, com a luminosidade pesando menos que o matiz.
3. **Energia da página.** Soma a coesão entre todas as cartas, a vizinhança lado a lado e acima/abaixo e, se houver cor escolhida, a distância até ela.
4. **Otimização.** Um **recozimento simulado** testa trocas entre posições livres e guarda a melhor configuração, seguido de um polimento dentro de cada página.
5. **Ordem das páginas.** Em arco-íris a partir do vermelho, com as neutras no final. Páginas com cadeado ou cor mantêm o seu número.

---

## Memória e privacidade

- Tudo é salvo em **IndexedDB**, meio segundo após cada mudança: cartas, posições, travas, cores, lixeira e ajustes.
- Suas fotos **não são enviadas a nenhum servidor**. A única comunicação externa é a busca, que consulta `api.tcgdex.net` e baixa imagens de `assets.tcgdex.net`.
- **Limpar** pede um segundo clique e apaga tudo, inclusive a lixeira. **Importar** substitui o fichário atual.

---

## Limitações e problemas comuns

- **Arrastar e soltar** costuma não funcionar com toque no celular.
- Limpar os dados do site apaga o fichário; **Redetectar** só vale para a última foto da sessão.
- A TCGdex oculta cartas sem imagem; se uma não aparecer em português, tente English.
- A estimativa de cor é aproximada: cartas holográficas, reflexos e fundos irregulares atrapalham.
- A melhoria de ~20% na harmonia veio de testes com **cores simuladas**, não de uma coleção real.

| Sintoma | O que fazer |
|---|---|
| "Failed to fetch" na busca | Você está no link publicado: baixe o arquivo HTML e abra direto |
| Erro HTTP 5xx ou "tempo esgotado" | Instabilidade da API (o app já tenta 3 vezes): tente de novo mais tarde |
| "Não consegui baixar as imagens" | O servidor bloqueou a leitura (CORS): tente outra carta ou idioma |
| Detecção erra o número de cartas | Ajuste a *Sensibilidade* e use *Redetectar*, ou troque para a grade |
| "Memória indisponível" | O navegador bloqueou o armazenamento (ex.: modo anônimo): use **Exportar** |

---

## Estrutura

```
binder-color-match.html   # aplicação completa (HTML + CSS + JavaScript)
README.md
```

Sem build e sem dependências. Usa Canvas 2D, IndexedDB, `fetch` e drag and drop nativo.

---

## Créditos e aviso legal

- Dados e imagens fornecidos pela [TCGdex](https://tcgdex.dev), uma API aberta e gratuita.
- Projeto pessoal, **não afiliado** à The Pokémon Company, Nintendo, Creatures ou Game Freak. Pokémon e as imagens das cartas pertencem aos seus donos; use-as apenas para organizar a sua coleção.

### Caso tenha alguma recomendação ou sugestão, mande para o email: aurelianoclaudio29@gmail.com