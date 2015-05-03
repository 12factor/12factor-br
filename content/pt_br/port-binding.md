## VII. Port binding
### Exporte serviços por porta de ligação

Aplicações web são executadas algumas vezes dentro de um container de servidor web. Por exemplo, aplicações PHP podem ser executadas como um módulo dentro do [Apache HTTPD](http://httpd.apache.org/), ou aplicações Java que podem ser excutadas dentro do [Tomcat](http://tomcat.apache.org/).

**A aplicação twelve-factor é completamente auto-suficiente** e não depende da injeção do runtime de um servidor web dentro de um ambiente de execução para criar um serviço voltado para web. A aplicação web **exporta HTTP como um serviço ligado a uma porta**, e fica escutando solicitações recebidas nessa porta.

Em um ambiente local de desenvolvimento, o desenvolvedor visita a URL de um serviço como `http://localhost:5000` para acessar o serviço exportado pela sua aplicação. Na implantação, uma camada de roteamento lida roteando as solicitações de um hostname público para uma porta ligada aos processos web.  

Isto é tipicamente implementado usando [declaração de dependências](./dependencies) para adicionar uma biblioteca webserver para a aplicação, tais como [Tornado](http://www.tornadoweb.org/) para Python, [Thin](http://code.macournoyer.com/thin/) para Ruby, ou [Jetty](http://jetty.codehaus.org/jetty/) para java e outras linguagens baseadas na JVM. Isto acontece inteiramente no *user space*, ou seja, no código da aplicação. O contrato com a ambiente de execução é a ligação a uma porta para atender as solicitações.    

HTTP não é o único serviço que pode ser exportado por vinculação de porta. Quase qualquer tipo de software servidor pode ser executado através de um processo ligação de porta e ficar aguardando solicitações de entrada. Exemplos incluem [ejabberd](http://www.ejabberd.im/) (fallando [XMPP](http://xmpp.org/)), e [Redis](http://redis.io/) (falando o [Redis protocol](http://redis.io/topics/protocol)).

Note também que a abordagem de ligação de porta significa que uma aplicação pode se tornar o [serviço de apoio](./backing-services) para outra aplicação, fornecendo a URL para a aplicação de apoio como um recurso identificado na [configuração](./config) para a aplicação consumir.
