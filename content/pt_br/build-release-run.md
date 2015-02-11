## V. Build, release, run
### Separe estritamente os estágios de build e execução

Uma [base de código](./codebase) é transformada num deploy (de não-desenvolvimento) através de três estágios:

* O *estágio de build* é uma transformação que converte um repositório de código em um pacote executável conhecido como *build*. Usando uma versão do código de um commit especificado pelo processo de desenvolvimento, o estágio de build obtém e fornece [dependências](./dependencies) e compila binários e ativos.
* O *estágio de release* pega a build produzida pelo estágio de build e a combina com a atual [configuração](./config) do deploy. A *release* resultante contém tanto o build quanto a configuração e está pronta para execução imediata no ambiente de execução.
* O *estágio de execução* (também conhecido como "runtime") roda o app no ambiente de execução, através do início de alguns dos [processos](./processes) do app com uma determinada release.

![Código vira uma build, que é combinada com a configuração para se criar uma release.](/images/release.png)

**O app doze-fatores usa separação estrita entre os estágios de build, release e execução.** Por exemplo, é impossível alterar código em tempo de execução, já que não meios de se propagar tais mudanças de volta ao estágio de build.

Ferramentas para deploy tipicamente oferecem ferramentas de gestão de release, mais notadamente a habilidade de se reverter à uma release prévia. Por exemplo, a ferramenta de deploy [Capistrano](https://github.com/capistrano/capistrano/wiki) armazena releases em um subdiretório chamado `releases`, onde a release atual é um link simbólico para o diretório da release atual. Seu comando `rollback` torna fácil reverter para uma release prévia.

Cada release deve sempre ter um identificador de release único, tal qual o timestamp da release (como `2011-04-06-20:32:17`) ou um número incremental (como `v100`). Releases são livro-razões onde apenas se acrescenta informações e uma release não pode ser alterada uma vez que é criada. Qualquer mudança deve gerar uma nova release.

Builds são iniciadas pelos desenvolvedores do app sempre que novos códigos entrem no deploy. A execução de runtime, todavia, pode acontecer automaticamente em casos como o reinício do servidor, ou um processo travado sendo reiniciado pelo gerenciador de processos. Assim, no estágio de runtime deve haver quanto menos partes móveis quanto possível, já que problemas que previnem um app de rodar pode causá-lo a travar no meio da noite quando não há desenvolvedores por perto. O estágio de build pode ser mais complexo, já que os erros estão sempre à vista do desenvolvedor que está cuidando do deploy.
