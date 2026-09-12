# 2 - Prompt e Context Engineering

Aula mais focada na revisão dos conteudos e conseitos passados na ultima aula sobre os fundamentos de IA.

### Recapitulação da ultima aula 

Começamos revisando sobre temperatura, problabilidade,

## O que é Engenharia de contexto?

A Engenharia de Contexto é a disciplina de projetar, estruturar e gerenciar as informações que um modelo de inteligência artificial (LLM) acessa antes de gerar uma resposta.  Diferente da engenharia de prompt, que foca na formulação da pergunta, a engenharia de contexto decide quais dados, memórias e ferramentas serão incluídos na "janela de contexto" do modelo para garantir precisão e relevância. 

O objetivo central é curar o conjunto ótimo de tokens que o modelo vê durante a inferência, evitando a sobrecarga de informações irrelevantes que podem degradar o desempenho.  Isso envolve integrar fontes externas (como bancos de dados vetoriais via RAG), manter histórico de conversas e aplicar regras persistentes, transformando interações isoladas em sistemas de IA com estado e memória. 

Em resumo, enquanto a engenharia de prompt é o "acabamento" da pergunta, a engenharia de contexto é o alicerce que fornece ao modelo o ambiente informacional completo necessário para executar tarefas complexas de forma confiável e escalável.

## Vibe coding vs Ai-assisted

Vibe coding é uma abordagem de desenvolvimento de software onde o usuário descreve o que deseja em linguagem natural e confia na Inteligência Artificial para gerar a maior parte do código, focando no resultado final e na prototipagem rápida, muitas vezes sem revisão técnica profunda.

Desenvolvimento assistido por IA (ou engenharia assistida por IA) é um método mais rigoroso onde o desenvolvedor mantém o controle total da arquitetura, lógica e estrutura, utilizando a IA como uma ferramenta para acelerar tarefas específicas, gerar boilerplate ou sugerir melhorias, mas sempre revisando e validando o código gerado.

### Principais Diferenças

| 

Em resumo, o vibe coding é ideal para quem quer validar uma ideia rapidamente sem se preocupar com a implementação técnica, enquanto o desenvolvimento assistido por IA é a prática profissional responsável que utiliza a IA para aumentar a produtividade mantendo a integridade e a qualidade do código

## System prompt

## Modelo ao Agente

## Harremes, skills e tools