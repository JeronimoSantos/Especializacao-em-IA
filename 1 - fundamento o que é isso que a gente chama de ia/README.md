# 1 Fundamentos: o que é isso que a gente chama de IA?

## Qual a diferença entre um Modelo e um Agente de IA?

Um modelo é uma representação simplificada da realidade ou um sistema computacional projetado para analisar dados, identificar padrões e gerar informações, operando de forma isolada e estática

Já um agente é uma entidade autônoma (computacional ou em modelos baseados em agentes) que percebe seu ambiente, toma decisões independentes e executa ações para atingir objetivos, adaptando-se dinamicamente. 

> A principal diferença reside na autonomia e interação

De uma forma resumida seria assim:
```
Modelo = o "cérebro" que só pensa e responde.
Você dá uma pergunta, ele devolve uma resposta. Fim. Ele não faz nada além disso sozinho.

Agente = uma pessoa que usa o cérebro para fazer coisas.
Ele recebe um objetivo, pensa, usa ferramentas (busca na web, envia e-mail, edita arquivo, clica em botões), verifica o resultado, e continua até terminar a tarefa. 
```

> Resumindo em uma frase: o modelo sabe, o agente faz.

## O que é janela de contexto, temperatura e alucinação?

Janela de Contexto pense nela como a memória de trabalho do modelo — o tamanho da "mesa" onde ele pode colocar papéis enquanto pensa. 

- Tem um limite (medido em tokens, que são pedaços de palavras). 
Tudo que cabe na mesa, ele enxerga. 
- Tudo que não cabe, ele esquece — como se nunca existiu.
- Quando a conversa fica muito longa, as mensagens antigas "saem da mesa" e o modelo para de lembrá-las. 
- Problema extra — "lost in the middle": quando a mesa está cheia, o modelo tende a prestar mais atenção no começo e no fim, e a ignorar o meio. Informação importante no centro do texto pode ser simplesmente esquecida.

> Analogia: você lê um livro de 800 páginas e tenta responder uma pergunta sobre o capítulo 350. Provavelmente vai "chutar" ou lembrar de outra parte.

Temperatura é um botão de criatividade que vai de 0 a 2 (geralmente se usa entre 0 e 1).

| Valor | Comportamento | Bom para |
|-------|---------------|----------|
| 0 (minímo) | Sempre escolhe a palavra mais provável.  Resposta previsível e "robótica". | Fatos, código, resumos técnicos |
| 0.7 (padrão) | Equilíbrio entre precisão e variedade | Conversas gerais |
| 1.0-2.0 (máximo) | Aceita palavras improváveis. Mais criativo, mas pode "divagar" | Poemas, brainstorming, histórias |

Como funciona por baixo: o modelo calcula uma lista de palavras possíveis com probabilidades. A temperatura "achata" ou "aperta" essa lista:

- Temperatura baixa → só a palavra mais provável ganha.
- Temperatura alta → até palavras improváveis têm chance. 

> Analogia: é como pedir a alguém "me dá uma resposta segura" (temperatura baixa) vs. "me dá algo surpreendente" (temperatura alta).

Alucinação é quando o modelo inventou algo com total confiança, como se fosse verdade.

Por que acontece?

- O modelo foi treinado para prever a próxima palavra, não para verificar fatos. 
- Quando ele não sabe a resposta, em vez de dizer "não sei", ele preenche o vazio com algo que soa plausível.
- Causas principais: lacunas nos dados de treino, conhecimento desatualizado (knowledge cutoff), prompts vagos, ou temperatura alta demais.

Exemplo clássico: pedir "me dê a referência bibliográfica do artigo X" e o modelo inventar um DOI, autores ou título que não existem — mas com aparência 100% real.

> Analogia: é como um aluno que, em vez de dizer "não estudei isso", chuta uma resposta que parece correta e ainda fala com convicção.

### Como os três se conectam

| Problema | Causa | Efeito |
|----------|-------|--------|
| Esquece o início da conversa | Janela de contexto cheia | Respostas incoerentes |
| Resposta "divergente" ou errada | Temperatura alta demais | Texto criativo mas impreciso |
| Inventa fatos com confiança | Alucinação (modelo "chuta") | Informação falsa que parece verdadeira |

## O que são Funções puras vs. Funções probabilísticas?

Função Pura (Determinística)

Regra simples: mesma entrada → sempre a mesma saída. Sem surpresas, sem aleatoriedade.
```
f(2) = 4   →   sempre 4, todo mundo, todo dia
```

Exemplos do mundo real:

- ``2 + 2 = 4``
- Converter 100 km para milhas → sempre 62.14
- Um algoritmo de ordenação com a mesma lista → sempre a mesma ordem

Propriedades-chave:

- Previsível a 100%
- Fácil de testar (se quebra, quebra sempre)
- Sem efeito colateral (não muda nada fora de si) 

Função Probabilística

Regra: mesma entrada → uma distribuição de possíveis saídas. A cada chamada, você pode obter um resultado diferente — sempre "razoável", mas não idêntico.
```
g("Me dê uma frase sobre o mar")
  → 1ª vez: "O mar era calmo naquela manhã."
  → 2ª vez: "As ondas quebravam violentamente."
  → 3ª vez: "O cheiro de sal impregnava o ar."   
```

Propriedades-chave:

- A saída vem de uma distribuição de probabilidade (ex: 40% chance de "calmo", 30% de "violento", 30% de "sal")
-A cada chamada, amostra (sorteia) um resultado dessa distribuição
- Nunca "errada" — só variável

### Onde o LLM se encaixa?

Aqui está o detalhe que muita gente confunde:

| Camada | Natureza |
|--------|----------|
| Cálculo interno (forward pass) | ✅ Determinístico — mesmo input → mesmos logits (probabilidades) |
| Amostragem (temperatura, top-k, top-p) | ❌ Probabilístico — escolhe um token da distribuição |

Ou seja: o modelo sabe as probabilidades de cada palavra possível (parte determinística), mas sorteia qual delas vai usar (parte probabilística).

> Analogia: imagine um dado com pesos diferentes. A distribuição de pesos é fixa (determinístico). Mas cada vez que você joga, o resultado pode ser outro (probabilístico). 

### E como "forçar" um LLM a se comportar como função pura?

| Alavanca | Efeito |
|----------|--------|
| Temperatura = 0 | Sempre escolhe o token de maior probabilidade → saída determinística (quase sempre idêntica) |
| Top-k = 1 | Mesmo efeito: só o token #1 conta |
| Seed fixa (quando disponível) | Garante que o sorteio use a mesma "semente" → mesmo resultado |

Com temperature=0 + top_k=1, o LLM se aproxima de uma função pura — mesmo prompt, mesma resposta, toda vez.

Resumo em uma frase

- Função pura: "o que vai sair, eu já sei."
- Função probabilística: "o que vai sair, eu só sei o quão provável é." 

E o LLM é, por natureza, probabilístico — mas você pode empurrá-lo para o lado determinístico quando precisar de previsibilidade (código, dados, compliance) ou deixá-lo livre quando precisar de criatividade (texto, brainstorm, arte).

## Como funciona as Camadas de cache?

Existem 3 camadas de cache em LLMs, cada uma num nível diferente da pilha

### 1. KV Cache (interno ao modelo)

O que é: Uma "memória de curto prazo" que guarda os cálculos de atenção (vetores Key e Value) já feitos para os tokens anteriores.
```
Sem cache:  "Olá" → "mundo" → "como" → "vai"
            Cada token recalcula a atenção de TODOS os anteriores. O(n²)

Com cache:  "Olá" → guarda K,V
            "mundo" → usa cache de "Olá" + guarda K,V
            "como" → usa cache de "Olá"+"mundo" + guarda K,V
            "vai" → usa cache de tudo + guarda K,V. O(n)   
```

| Característica | Detalhe |
|----------------|---------|
| Quando | Durante um único request |
| Quem ativa | Ninguém — é automático |
| Onde fica | VRAM da GPU |
| Benefício | Geração de tokens ~10x mais rápida |
| Custo | Memória (quanto mais contexto, mais VRAM) |

> Analogia: é como anotar em um rascunho cada linha que você já calculou numa prova de matemática, em vez de recalcular tudo a cada passo.

### 2. Prompt Caching / Prefix Caching (nível API)

O que é: O provedor (OpenAI, Anthropic, Google…) persiste o KV cache entre requests diferentes, desde que o prefixo do prompt seja idêntico.
```
Request 1: [system prompt de 5.000 tokens] + "Qual o clima?"
           → Calcula tudo, salva o KV do prefixo no cache.

Request 2: [system prompt de 5.000 tokens] + "Qual a população de SP?"
           → Detecta o mesmo prefixo → PULA o cálculo dele → só processa a pergunta nova.   
```

| Característica | Detalhe |
|----------------|---------|
| Quando | Entre requests (vários usuários, mesmo sistema) |
| Quem ativa | O provedor (às vezes é automático, às vezes você marca com cache_control) |
| Onde fica | Servidor do provedor (com TTL de ~5 min a 1 h) |
| Benefício | Custo do prefixo cai 50–90%; latência (TTFT) cai até 85% |
| Requísito | Prefixo idêntico token a token (ordem, formatação, tudo) |

> Analogia: é como um restaurante que pré-prepara o molho base. Cada prato novo só precisa do ingrediente final — não refaz o molho do zero.

### 3. Semantic Caching (nível aplicação)

O que é: Você (o desenvolvedor) guarda respostas completas em um banco vetorial e, quando uma nova pergunta é semanticamente parecida, devolve a resposta cacheada sem nem chamar o modelo.
```
Pergunta: "Como faço para resetar minha senha?"
  → Gera embedding → busca no vector DB
  → Encontra "Para resetar sua senha, vá em Config > Segurança..." (similaridade 0.93)
  → Retorna a resposta cacheada. Modelo NUNCA foi chamado.

Pergunta: "Esqueci minha senha, o que eu faço?"
  → Mesmo match semântico → mesma resposta cacheada.   
```

| Característica | Detalhe |
|----------------|---------|
| Quando | Antes de qualquer chamada ao LLM |
| Quem ativa | 	Você (no código da aplicação) |
| Onde fica | Seu banco vetorial (Redis, Pinecone, Weaviate…) |
| Benefício | Zero custo de inference; resposta em ~ms |
| Risco | Resposta desatualizada (precisa de TTL / invalidação) |

> Analogia: é como um call center com script. Se a pergunta já foi respondida antes (mesmo com outras palavras), o atendente devolve a resposta pronta sem passar para o especialista.

Como se combinam na prática
```
Sua app → [Semantic Cache?] → [API do provedor → Prompt Cache → KV Cache → Geração]
              ↓ hit                    ↓ hit
         Retorna direto          Pula prefixo, só processa o resto   
```

Cada camada "corta" um pedaço do trabalho. Juntas, podem reduzir o custo de inference em 90%+ para workloads com padrões repetitivos (FAQs, RAG com corpus fixo, agentes com ferramentas fixas).

##  Por que a IA não sabe o que sabe?

Porque o "saber" de um LLM não é um arquivo que ele pode abrir.  É um padrão distribuído em bilhões de números, e não existe um "índice" que diga "isso eu sei, aquilo eu não sei".

Da para desdobrar em 4 camadas

### 1. O conhecimento não está "guardado" — está espalhado

Quando você pensa "saber", imagina uma enciclopédia: você abre a página "Pernambuco" e lê. 

Num LLM, "saber que Pernambuco é um estado do Brasil" não está em nenhum lugar específico. Está distribuído entre bilhões de pesos (números) espalhados por todas as camadas da rede. Não existe um "arquivo Pernambuco.txt".

> Analogia: é como saber andar de bicicleta. O "conhecimento" não está em um músculo específico — está no padrão de ativação de vários músculos ao mesmo tempo. Você não consegue "abrir" esse conhecimento e ler o que ele diz. 

Consequência: o modelo não tem como "procurar" no próprio cérebro e dizer "ah, aqui tem Pernambuco, mas aqui não tem o PIB de 2019". Ele só sabe quando o padrão ativa — e às vezes ativa de forma errada. 

### 2. O modelo tem um sinal de confiança — mas não consegue lê-lo

Aqui está o detalhe contraintuitivo:

- Por baixo, o modelo calcula probabilidades para cada token. Se a resposta é "Pernambuco", o token "Pernambuco" vai ter 95% de probabilidade. Se ele está "chutando", talvez 12%.
- Esse sinal existe e correlaciona com a correção da resposta.
- Mas o modelo não tem um "painel" que mostra "confiança: 12%". Ele não consegue introspectar — ou seja, olhar para dentro e dizer "ei, estou com pouca certeza aqui".

Um paper de 2025 (Song et al., "Language Models Fail to Introspect About Their Knowledge of Language") testou isso diretamente: pediu aos modelos que reportassem sua própria confiança, e descobriu que os auto-relatos não refletem um acesso privilegiado ao estado interno.  O modelo "acha" que está confiante, mas isso é um padrão aprendido do treino, não uma leitura real dos seus próprios logits.

> Analogia: é como ter um termômetro embutido no corpo que funciona perfeitamente, mas você não consegue ver a temperatura. Você só sabe se "sente frio" — e às vezes o "sentir" é impreciso.

### 3. O treino ensinou a sempre responder — não a dizer "não sei"

Durante o pré-treino, o modelo foi otimizado para prever o próximo token. Sempre. Não existia a opção "não sei" como resposta válida — o objetivo era sempre gerar alguma continuação plausível.

Quando ele não tem um padrão forte para a pergunta, ele escolhe o token mais provável mesmo assim — e isso é a alucinação.

O "não sei" que o modelo diz hoje (após o fine-tuning com RLHF) é um comportamento aprendido, não uma auto-avaliação genuína.  Ele aprendeu que "dizer não sei" soa seguro, mas não porque ele sabe que não sabe.

> Analogia: um aluno que, em vez de ter aprendido a dizer "não sei", aprendeu que dizer "não sei" com convicção é a resposta mais "segura" em provas. Ele não sabe que não sabe — ele só imita a postura de quem sabe que não sabe.

### 4. O "saber" é um gradiente, não um interruptor

Para humanos, saber é quase binário: ou você sabe a capital da França, ou não sabe.

Para um LLM, "saber" é uma probabilidade contínua

O modelo tem um sinal interno de "quão certo eu estou", mas não tem acesso a ele.  Ele foi treinado para sempre gerar a próxima palavra, seu conhecimento é um gradiente difuso (não um arquivo), e o "não sei" que ele diz é um comportamento aprendido — não uma auto-avaliação genuína.

## Sobre Arquitetura Transformer e Singularidade

Arquitetura Transformer
### O problema que ela resolveu

Antes do Transformer (2017), os modelos processavam texto sequencialmente (RNNs): palavra 1 → palavra 2 → palavra 3… Cada palavra só "via" o que já passou. Problemas:

- Lento (não paraleliza)
- Esquece o começo de frases longas (o "estado" vai se degradando) 

A ideia central: Self-Attention

O Transformer elimina a sequência. Todas as palavras são processadas ao mesmo tempo, e cada uma "olha" para todas as outras para decidir o que é relevante.

> Analogia: imagine uma sala com 10 pessoas. Em vez de uma fila onde cada um repassa o recado ao próximo (RNN), todo mundo fala com todo mundo ao mesmo tempo e decide "quem é relevante pra mim agora" (Transformer). 

### Como funciona (simplificado)

Cada token gera 3 vetores:

| Vetor | Pergunta que responde |
|-------|-----------------------|
| Query (Q) | "O que eu estou procurando?" |
| Key (K) | "O que eu tenho pra oferecer?" |
| Value (V) | "Qual é a informação real?" |

O token calcula: "Meu Q bate com quais Ks da sala?" → quanto mais "bate", mais peso ele dá ao V daquele token.
```
"O gato preto dormiu no sofá"

Para o token "dormiu":
  Q("dormiu") vs K("gato")  → alto peso  (quem dormiu?)
  Q("dormiu") vs K("sofá")  → alto peso  (onde?)
  Q("dormiu") vs K("preto") → peso baixo (pouco relevante)   
```

### Multi-Head

Em vez de uma "sala de conversa", o modelo tem várias salas em paralelo (ex: 32, 64, 128 heads). Cada head aprende a prestar atenção em coisas diferentes:

- Head 1: relações gramaticais (sujeito → verbo)
- Head 2: referências ("ele" → "gato")
- Head 3: contexto semântico (palavras relacionadas)

Depois, os resultados de todos os heads são combinados.

O "restante" do bloco

Cada camada do Transformer é:
```
Token → [Self-Attention] → [Feed-Forward (rede neural simples)] → Token atualizado
         "quem importa?"      "refinar meu entendimento"   
```

Isso se repete N vezes (ex: 32, 48, 128 camadas). Cada camada refina um pouco mais a representação.

### Por que "Attention Is All You Need"?

Porque a atenção substituiu a recorrência.  Não precisa mais de "memória de estado" passada de palavra em palavra — a atenção conecta qualquer posição com qualquer outra diretamente, em paralelo. 

Variantes que existem hoje

| Tipo | Usa | Exemplo |
|------|-----|---------|
| Encoder-only | Só "lê" e entende | BERT |
| Decoder-only | Só "gera" (previsão de próximo token) | GPT, LLaMA, Gemini, Claude |
| Encoder-Decoder | Lê + gera | T5, original 2017 |

Os LLMs atuais (ChatGPT, Gemini, Claude) são decoder-only: recebem o prompt, e preveem o próximo token repetidamente. 

## Singularidade (de IA) o que é?

O momento em que a IA ultrapassa a capacidade cognitiva humana de forma ampla e generalizada — e, a partir daí, melhora a si mesma num ciclo que acelera exponencialmente, tornando-se imprevisível. 

> Analogia: é como se você tivesse um assistente que, em um dia, se torna melhor que você em tudo (programar, pesquisar, criar, decidir) — e no dia seguinte ele se torna 2x melhor que si mesmo. E no outro, 4x. Em poucas semanas, já não dá pra prever o que ele vai fazer.

### Por que é tão difícil prever?

- Não sabemos se a arquitetura atual (Transformer) é suficiente para AGI, ou se precisa de algo novo (memória contínua, aprendizado contínuo, mundo interno).
- Escalar não é o mesmo que generalizar. Modelos maiores ficam melhores, mas ainda "quebram" em tarefas que um humano de 8 anos resolveria (raciocínio causal, senso comum, planejamento de longo prazo).
- O gargalo pode não ser compute. Pode ser algorítmico, de dados, ou de segurança — e aí a curva "plana" de repente.

Em resumo: o Transformer é o motor (a arquitetura que permite processar e gerar linguagem em escala), e a Singularidade é a pergunta: "quando esse motor vai se tornar tão poderoso que deixa de ser controlável por quem o dirige?" — e a resposta honesta é: ninguém sabe, e o intervalo de estimativas vai de 2027 a 2060+.
