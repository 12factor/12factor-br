## III. Configurações
### Armazene as configurações no ambiente

A *configuração* de uma aplicação é tudo o que é provável variar entre [deploys](/codebase) (teste, produção, ambientes de desenvolvimento, etc). Isto inclui:

* Recursos para a base de dados, Memcached, e outros [serviços de apoio](/backing-services)
* Credenciais para serviços externos como Amazon S3 ou Twitter
* Valores por deploy como o nome canônico do host para o deploy

Aplicações as vezes armazenam as configurações no código como constantes. Isto é uma violação do twelve-factor, o que exige uma **estrita separação da configuração a partir do código**. Configuração varia substancialmente entre deploys, código não.

A prova de fogo para saber se uma aplicação tem todas as configurãções corretamente consignadas fora do código é saber se a base de código poderia ser feita de código aberto a qualquer momento, sem comprometer as credenciais.

Note que esta definição de "configuração" **não** inclui configuração interna da aplicação, como `config/routes.rb` em Rails, ou como [módulos de códigos são conectados](http://static.springsource.org/spring/docs/2.5.x/reference/beans.html) em [Spring](http://www.springsource.org/). Este tipo de configuração não varia entre deploys, e por isso é melhor que seja feito no código.

Outra abordagem para configuração é o uso de arquivos de configuração que são verificados no controle de revisão, como `config/database.yml` em Rails. Isto é uma grande melhoria sobre o uso de constantes que são verificadas no repositório do código, mas ainda tem pontos fracos: é fácil de verificar por engano em um arquivo de configuração do repositório; há uma tendência para que os arquivos de configuração sejam espelhados em diferentes lugares e diferentes formatos, tornando-se difícil de ver e gerenciar todas as configurações em um só lugar. Além disso estes formatos tendem a ser específicos da liguagem ou framework.

**A aplicação twelve-factor armazena configuração em *variáveis de ambiente*** (muitas vezes abreviadas para *env vars* ou *env*). Env vars são fáceis de mudar entre deploys sem alterar qualquer código. ao contrário de arquivos de confuguração, há pouca chance de serem verificados acidentalmente no repositório do código; e ao contrário dos arquivos de configuração personalizados, ou outros mecanismos de configuração como Propriedades do Sistema Java, eles são por padrão agnósticos a linguagem e ao SO. 

Outro aspecto do gerenciamento de configuração é o agrupamento. Às vezes, aplicações de configuração em lotes em grupos nomeados (muitas vezes chamadas de "environments") em homenagem a deploys específicos, tais como `development`, `test`, e `production` ambientes em Rails. Este método não escala de forma limpa: quanto mais deploys da aplicação são criados, novos nomes de ambiente são necessários, tais como `staging` ou `qa`. A medida que o projeto cresce ainda mais, desenvolvedores podem adicionar seus próprios ambientes especiais como `joes-staging`, resultando em uma explosão combinatória de configurações que torna o gerenciamento de deploys da aplicação muito frágil.

Em uma aplicação twelve-factor, env vars são controles granulares, cada um totalmente ortogonal às outras env vars. Elas nunca são agrupadas como "environments", mas em vez disso são gerenciadas independente de cada deploy. Este é um modelo que escala sem problemas como a aplicação que se expande naturalmente em muitos deploys durante sua vida útil.
