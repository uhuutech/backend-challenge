<img src="https://i.imgur.com/rNOFiz0.jpeg" width="100%">

# Introdução

Olá! Obrigado pelo interesse em participar do nosso processo seletivo para vaga de _Backend Developer_ na [Uhuu](https://uhuu.com/).

O objetivo do desafio proposto é permitir uma melhor avaliação das suas habilidades como candidato à vaga de _Backend Developer_. Este desafio deve ser feito apenas por você. Sua implementação e escolha de ferramentas poderá ser questionada em outra etapa.

Você provavelmente chegou aqui através da indicação de alguma pessoa da empresa após passar pelas outras etapas do processo seletivo. Se este não for o seu caso e mesmo assim você implementar alguma solução para este exercício ele **não** será avaliado.

> Você _pode_ usar o problema descrito aqui para exercitar suas habilidades de
> desenvolvimento, mas a sua solução será avaliada por alguém da Uhuu
> **apenas se** você estiver no processo seletivo da vaga de _Backend Developer_.

Se você tiver interesse em se candidatar para uma vaga da Uhuu siga as instruções no site: https://uhuu.gupy.io/.

Nessa página você encontra as vagas abertas hoje e todos os detalhes de nosso processo seletivo. Se você não encontrou uma vaga que pareça adequada confira a página novamente em um ou dois meses, ela é atualizada com certa frequência. 😉

## O problema

A _Corporação ACME ™_ é uma empresa com milhares de funcionários e filiais distribuídas em diversas cidades do Brasil. Funcionários da _Corporação ACME ™_, estão distribuídos nessas filiais em diversos municípios e seguem as respectivas agendas de feriados municipais.

Uma das estratégias da marca é o atendimento rápido e eficiente ao consumidor. Hoje a empresa paga um adicional em feriados do Rio de Janeiro para que tenha atendentes suficientes nesses dias. O atendimento está centralizado nessa cidade e seus funcionários absorvem toda a demanda de atendimento ao cliente do país.

Esse atendimento poderia ser feito de qualquer cidade, não necessariamente de apenas uma das filiais. Um projeto da empresa com objetivo de reduzir gastos com atendimento é distribuir os chamados entre as suas filiais para que algum funcionário faça esse atendimento. Dessa forma pode ser possível evitar o pagamento de adicional de feriado em feriados municipais e estaduais.

## Feriados

No Brasil existem feriados nacionais, estaduais e municipais. Além disso alguns feriados não possuem uma data fixa, ou seja, cada ano esses feriados caem em dias diferentes. Os _Feriados Móveis_ são: Carnaval, Sexta-Feira Santa, Páscoa e Corpus Christi. Três desses feriados são determinados a partir da data da Páscoa.

A sexta-feira santa é um feriado nacional, mas as regras do Carnaval e Corpus Christi variam de acordo com o município.

Regras dos feriados móveis:
- A terça-feira de carnaval ocorre 47 dias antes do domingo de Páscoa;
- Corpus Christi ocorre 60 dias após o domingo de Páscoa;
- A sexta-feira santa ocorre 2 dias antes do domingo de Páscoa;

A Páscoa é celebrada no primeiro domingo após a primeira lua cheia que ocorre depois do equinócio de Outono (março). A data em que esse dia cai em um determinado ano pode ser calculada com o [Algoritmo de Meeus](https://pt.wikipedia.org/wiki/C%C3%A1lculo_da_P%C3%A1scoa#Algoritmo_de_Meeus/Jones/Butcher). O pseudo-código do algoritmo é o seguinte:
```
 a = ANO MOD 19
 b = ANO \ 100
 c = ANO MOD 100
 d = b \ 4
 e = b MOD 4
 f = (b + 8) \ 25
 g = (b - f + 1) \ 3
 h = (19 × a + b - d - g + 15) MOD 30
 i = c \ 4
 k = c MOD 4
 L = (32 + 2 × e + 2 × i - h - k) MOD 7
 m = (a + 11 × h + 22 × L) \ 451
 MÊS = (h + L - 7 × m + 114) \ 31
 DIA = 1+ (h + L - 7 × m + 114) MOD 31
```

Em que `\` é uma divisão de inteiro, ou seja, `7 \ 3 = 2`.

Os feriados nacionais com data fixa são:

- 01/01 Ano Novo
- 21/04 Tiradentes
- 01/05 Dia do Trabalhador
- 07/09 Independência
- 12/10 Nossa Senhora Aparecida
- 02/11 Finados
- 15/11 Proclamação da República
- 25/12 Natal

O dia da Consciência Negra também é outra exceção peculiar. É um "dia comemorativo" nacional, mas não é considerado um feriado nacional; esse dia é decretado feriado municipal em milhares de cidades e é feriado estadual em alguns estados.

## Desafio

Para resolver esse problema você deve desenvolver uma API que permita consultar e cadastrar feriados estaduais e municipais.

O endpoint para consultar feriados deve seguir o seguinte formato:
```
/feriados/CODIGO-IBGE/ANO-MES-DIA/
```

Onde CODIGO-IBGE pode ser um número de dois dígitos, para representar uma unidade federativa ou um número com 7 dígitos para representar um município.

O MES deve ser um mês válido, ou seja, entre 1 e 12. Mesmo cuidado deve ser tomado com o dia. Espere um ano com 4 números, mês com 2 números e dia também com 2 números, ou seja, "AAAA-MM-DD".

A sua API deve ser populada com os feriados nacionais. Todos os feriados estaduais e municipais consultados nos testes serão criados através da API.

Esse endpoint deve responder os seguintes verbos: GET, PUT e DELETE.

Para simplificar o problema, assuma que pode haver no máximo 1 feriado por dia em cada município, e que serão feitas consultas apenas em dias de semana, ou seja, de segunda a sexta.

O comportamento esperado do GET é que ele retorne status 200 e o nome do feriado se existir um feriado no dia especificado.

Exemplo buscando o dia 20 de Novembro no estado do Rio de Janeiro:
```
GET /feriados/33/2023-09-07/
{
    "name": "Independência"
}
```

Se não houver um feriado no dia para o estado ou município consultado a API deve retornar 404.

O cadastro de um feriado estadual ou municipal segue estrutura semelhante à consulta, mas não contém o ano, apenas o mês e dia do feriado.

Exemplo de cadastro do aniversário de São Paulo SP no dia 25 de Janeiro:
```
PUT /feriados/3550308/01-25/
{
    "name": "Aniversário da cidade de São Paulo"
}
```

A API deve retornar o status 200 para indicar que a requisição foi bem sucedida. Opcionalmente ela pode retornar 201 se esse feriado ainda não estava cadastrado na base. Se já existir um feriado cadastrado neste dia para o estado ou município especificado, o nome do feriado deve ser atualizado.

Deve haver também a opção de apagar um feriado.

Exemplo de remoção do aniversário de São Paulo:
```
DELETE /feriados/3550308/01-25/
```

O endpoint deve retornar 404 se esse feriado não existir ou 204 se a requisição foi aceita e o feriado removido com sucesso.

Uma tentativa de remover um feriado estadual num município deve retornar 403.
Uma tentativa de remover um feriado nacional em um município ou em uma unidade federativa também deve retornar 403.

O cadastro e remoção de feriados móveis deve ter uma assinatura diferente. No lugar do dia, é passado o nome do feriado após o código do ibge.

Exemplos:
```
PUT /feriados/5108402/carnaval/
PUT /feriados/4312658/corpus-christi/
```
As requisições acima marcam que a terça-feira de carnaval é feriado em Várzea Grande e corpus christi é feriado em Não-Me-Toque.

No exemplo abaixo a terça-feira de carnaval deixa de ser considerado feriado em Várzea Grande:
```
DELETE /feriados/5108402/carnaval/
```

## Dados disponíveis

Neste repositório você encontra um csv chamado `municipios-2019.csv` com o registro de 5.570 municípios brasileiros em 2019 [extraído do site do IBGE](https://www.ibge.gov.br/explica/codigos-dos-municipios.php). Esse csv tem duas colunas: uma com o código do IBGE para cada município e um com o nome do município.

Note que os dois primeiros dígitos desse código do IBGE indica qual é o estado do município. A seguir uma tabela com a relação estado/prefixo também extraída do site do IBGE:

|Prefixo|UF|
|-------|--|
|     12|AC|
|     27|AL|
|     16|AP|
|     13|AM|
|     29|BA|
|     23|CE|
|     53|DF|
|     32|ES|
|     52|GO|
|     21|MA|
|     51|MT|
|     50|MS|
|     31|MG|
|     15|PA|
|     25|PB|
|     41|PR|
|     26|PE|
|     22|PI|
|     33|RJ|
|     24|RN|
|     43|RS|
|     11|RO|
|     14|RR|
|     42|SC|
|     35|SP|
|     28|SE|
|     17|TO|

A lista de feriados nacionais que deve ser usada foi apresentada na seção [_Feriados_](#feriados). Os feriados estaduais e municipais de interesse serão cadastrados na API que você desenvolver.

## Avaliação

Em um primeiro momento nós não iremos olhar o seu código. O projeto será testado de forma automatizada e se ele passar nos testes o recrutador entrará em contato para agendar a entrevista técnica.

Portanto você deve codificar seu projeto em TypeScript / JavaScript (temos preferência pelo NestJS, mas você pode usar o framework que se sentir mais confortável). Alem disso, o seu projeto deve possuir um banco de dados (pode ser relacional ou não relacional) e deve estar preparado para rodar utilizando o docker compose.

Nós executaremos três conjuntos de testes na sua API:

1. Testes básicos (abertos)
2. Testes avançados (fechados)
3. Testes extras (fechados)

Se a API não passar nos testes básicos, faremos mais duas tentativas. Se mesmo assim ela não passar nos testes básicos nós encerramos os testes.

Se a API passar nos testes básicos e não passar nos testes avançados, faremos mais duas tentativas. Se mesmo assim ela não passar nos testes avançados nós encerramos os testes.

Se a API passar pelos testes avançados você já garantiu a sua participação na próxima etapa do processo, mesmo assim vamos executar os testes extras para avaliar mais alguns pontos da sua solução.

Os testes básicos estão disponíveis neste repositório no arquivo `tests-open.js` e é recomendado que você os use durante o desenvolvimento para avaliar se a sua API está correta. Como explicado acima, você **não passará** para a próxima etapa se a sua solução não atender todos os testes desse arquivo. Use as verificações presentes nele para guiar o desenvolvimento da sua solução.

Você pode executar esses testes com o [k6](https://k6.io/). Para instalar o k6 basta [baixar o binário](https://github.com/loadimpact/k6/releases) para o seu sistema operacional (Windows, Linux ou Mac).

E para rodar os testes abertos especificar a variável de ambiente "API_BASE" com o endereço base da API testada.

Exemplo de aplicação rodando no localhost na porta 8000:
```
k6 run -e API_BASE='http://localhost:8000' tests-open.js
```

## Entrega

Para realizar a entrega do desafio, você deverá enviar para o recrutador o link para o repositório com seu código. Exemplo:

https://github.com/seu-nome/backend-challenge.git

Não se esqueça de criar um arquivo `README.md` contendo as instruções para construir o app localmente.

:bangbang: **Importante!** O repositório com seu código deve estar publico, para que possamos acessa-lo!

## Feedback

Na Uhuu, valorizamos muito o fator humano e feedbacks. Acreditamos que o feedback é essencial para melhorar, aprender e facilitar processos. Dessa forma, assim que o seu desafio for submetido, prometemos enviar um feedback técnico nos próximos dias usando todos os critérios de avaliação descritos acima.

## Dúvidas

Caso haja qualquer dúvida sobre o teste, entre em contato com o recrutador que nós responderemos o mais rápido possivel.

---
## Obrigado e bom desafio!