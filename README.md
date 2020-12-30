# Dicas de desenvolvimento de software

Dicas de desenvolvimento que será útil no dia a dia, algumas coisas são óbvias mas não custa lembrar

## Organização
- Mantenha seu código organizado, identado, limpo
- Mantenha as pastas do projeto organizadas armazenando não só o código fonte mas documentação, arquivos relevantes para o projeto, manuais etc
- Comente seu código, especialmente quando a lógica é complexa pois outras pessoas podem ter que realizar manutenções e até se você voltar no código
depois de um tempo, vai entender rapidamente o que ele faz

## Transparência
- Tudo que o programa faz tem que esta disponível para o usuário, seja em telas ou documentação. Ele tem que saber o que o programa faz e não como uma caixa preta com exceção de informação sigilosa
- As mensagens que o programa emite tem que ser claras, principalmente as de erro indicando a causa, a solução, onde a regra esta etc

## Requisitos
- Faça o levantamento dos requisitos com quem for usar o sistema, não com pessoas que não entendem como ele funciona no dia a dia, mesmo que isso for contra hierarquias na empresa. Se isso for ignorado, quando o sistema for implantado, simplesmente ele não será utilizado, não vai suprir as necessidades e você terá perdido tempo e será taxado de não saber desenvolver
- Versione e documente o desenvolvimento: Quem pediu? Por que pediu? Quando foi feito o desenvolvimento?

## Desenvolvimento
- Use POO
- Use frameworks quando realmente for ajudar no projeto pois as vezes para usar um framework no seu projeto, vai dar mais trabalho que fazer o código puro ou utilizando suas próprias classes ou bibliotecas
- Reutilize códigos repetitivos por meio de funções, métodos ou serviços Web
- Use REST e JSON para criar WebServices
- Use multi tarefa quando aplicável
- Use processamento descentralizado quando aplicável
- Use cache, isso deixará seu site extremamente rápido
- Use CDN, isso deixará a parte estática do seu site extremamente rápida e escalável em qualquer lugar do mundo 

## Banco de dados
- Use índices nas suas consultas de banco de dados
- Como atualizar uma tabela da forma mais eficiente possível [(link)](https://github.com/vcd94xt10z/programming-tips/blob/master/banco-carga-de-dados.md)
- Use tabela de cache para acelerar consultas complexas 
- Em processamentos demorados, não deixe a conexão aberta desnecessáriamente pois isso pode derrubar o banco de dados se uma tabela bloquear os dados e todas as outros processos ficarem esperando.