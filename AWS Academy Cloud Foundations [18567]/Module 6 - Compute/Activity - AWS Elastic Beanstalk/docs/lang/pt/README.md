<header>

<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.5.0/css/all.css" integrity="sha384-B4dIYHKNBt8Bc12p+WXckhzcICo0wtJAoU8YZTY5qE0Id1GSseTk6S+L3BlXeVIU" crossorigin="anonymous">

<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">

<!-- Latest compiled and minified JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>

</header>

<!--include:Logo-->

<style type="text/css">
  body {
      font-family:  "Roboto", "Helvetica", sans-serif;
      font-size: 12pt;
      font-color: Gray;
      line-height: 1.6;
      margin: 50px;
  }
  p {
      list-style-position: inside;
  }
  #ssb_blue {
    background-color: #257ACF;
    font-weight: bold;
    font-size: 90%;
    color: white;
    border-radius: 5px;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    white-space: nowrap;
  }
  #ssb_voc_grey {
    background-color: #F2F3F4;
    font-weight: normal;
    font-size: 90%;
    color: black;
    border-radius: 3px;
    border: 1px solid gray;
    padding-top: 5px;
    padding-bottom: 5px;
    padding-left: 6px;
    padding-right: 6px;
    white-space: nowrap;
  }
  #ssb_grey {
    background-color: #DEDEDE;
    font-weight: bold;
    font-size: 90%;
    color: #444;
    position: relative;
    top:-1px;
    border-radius: 5px;
    border-width: 1px;
    border-style: solid;
    border-color: #444;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    white-space: nowrap;
  }
  #ssl_alexa_ocean {
    color: #00a0d2;
    font-weight: bold;
  }
</style>

# Atividade: AWS Elastic Beanstalk

<!-- Note to translators: This activity is unique to this course. -->

&nbsp;
&nbsp;
## Visão geral

Esta atividade disponibiliza para você uma conta da Amazon Web Services (AWS) com um ambiente do AWS Elastic Beanstalk pré-criado. Você implantará um código nela e observará os recursos da AWS que compõem o ambiente do Elastic Beanstalk.

&nbsp;

### Duração

Esta atividade levará aproximadamente **30 minutos** para ser concluída.

&nbsp;
&nbsp;
## Como acessar o Console de Gerenciamento da AWS

1. Na parte superior destas instruções, clique em <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratório) para iniciar o laboratório.

   Um painel **Start Lab** (Iniciar laboratório) é aberto exibindo o status do laboratório.

2. Aguarde até ver a mensagem *Lab status: in creation* (Status do laboratório: em criação). Para fechar o painel **Start Lab** (Iniciar laboratório), clique no **X**.

3. Na parte superior destas instruções, clique em <span id="ssb_voc_grey">AWS</span>.

   O Console de Gerenciamento da AWS é aberto em uma nova guia do navegador. O sistema faz o seu login automaticamente.

   **Dica**: se uma nova guia do navegador não abrir, normalmente haverá um banner ou um ícone na parte superior do navegador indicando que o navegador está impedindo que o site abra janelas pop-up. Clique no banner ou ícone e escolha **Allow pop ups** (Permitir pop-ups).

4. Organize a guia do **Console de Gerenciamento da AWS** para que ela seja exibida com estas instruções. Em um cenário ideal, você poderá ver as duas guias do navegador ao mesmo tempo, o que facilita o acompanhamento das etapas da atividade.

&nbsp;
&nbsp;
## Tarefa 1: Acessar o ambiente do Elastic Beanstalk

5. No **Console de Gerenciamento da AWS**, no menu **Services** (Serviços), escolha **Elastic Beanstalk**.

   Uma página intitulada **All Applications** (Todos os aplicativos) deve ser aberta com uma caixa verde que lista os detalhes de um aplicativo existente do Elastic Beanstalk.

   **Observação**: se a caixa não estiver verde, ela ainda não foi totalmente iniciada. Aguarde alguns instantes, e ela deverá mudar para *Health status: Green* (Status de integridade: verde).

   <img src="images/application.png" alt="elastic-beanstalk-app" width="600" style="border:1px solid black">

6. Na caixa verde de detalhes do aplicativo, clique no valor de URL exibido.

   A página **Dashboard** (Painel) do ambiente do Elastic Beanstalk é aberta.

7. Observe que a página mostra a integridade do seu aplicativo em verde (bom).

   O ambiente do Elastic Beanstalk está pronto para hospedar um aplicativo. No entanto, ele ainda não tem código em execução.

8. Próximo à parte superior da página, clique na URL (a URL termina com *elasticbeanstalk.com*).

   Quando você clica na URL, uma nova guia do navegador é aberta. No entanto, você deve observar que ela exibe a mensagem *"HTTP Status 404 - Not Found"* (HTTP Status 404 - Não encontrado). *Esse comportamento é esperado*, porque esse servidor de aplicativos ainda não tem nenhum aplicativo em execução. Retorne ao console do Elastic Beanstalk.

   Na próxima etapa, você implantará o código em seu ambiente do Elastic Beanstalk.

&nbsp;
&nbsp;
## Tarefa 2: Implantar um aplicativo de exemplo no Elastic Beanstalk

9. Para fazer download de um aplicativo de exemplo, clique neste link:
   https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/tomcat.zip

<!--the zip file is linked in this documentation page: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-getstarted.html-->

10. Retorne ao console do Elastic Beanstalk e clique em **Upload and Deploy** (Fazer upload e implantar).

11. Clique em **Browse or Choose File** (Procurar ou escolher arquivo), navegue até o arquivo **java-tomcat-v3.zip** do qual você acabou de fazer download e abra-o.

12. Clique em **Deploy** (Implantar).

    Levará um ou dois minutos para o Elastic Beanstalk atualizar seu ambiente e implantar o aplicativo.

    **Observação**: se aparecer um aviso na página do painel do Elastic Beanstalk informando que um perfil de instância é necessário para integração com o serviço do AWS X-Ray, ignore esse aviso.

13. Após a conclusão da implantação, clique no valor de URL próximo à parte superior da tela (ou, se a guia do navegador que exibia o status 404 ainda estiver aberta, atualize essa página).

    O aplicativo web que você implantou é exibido.

    <img src="images/web-app.png" alt="web-app" width="600" style="border:1px solid black">

    Parabéns! Você implantou com sucesso um aplicativo no Elastic Beanstalk.

14. Retorne ao console do Elastic Beanstalk e clique em **Configuration** (Configuração).

    Observe os detalhes aqui.

    Por exemplo, na linha **Instances** (Instâncias), ele mostra o tipo de instância, o intervalo de monitoramento e os detalhes do grupo de segurança das instâncias Amazon Elastic Compute Cloud (Amazon EC2) que hospedam seu aplicativo web.

15. Role até a linha **Database** (Banco de dados) na parte inferior da página.

    A linha **Database** (Banco de dados) está sem detalhes porque o ambiente não inclui um banco de dados.

16. Na linha **Database** (Banco de dados), clique em **Modify** (Modificar).

    Observe que, se desejado, você pode adicionar facilmente um banco de dados a esse ambiente, basta definir algumas configurações básicas e clicar em **Apply** (Aplicar). (No entanto, para a finalidade desta atividade, você não precisa adicionar um banco de dados.)

17. Clique em **Monitoring** (Monitoramento).

    Navegue pelos gráficos para ver os tipos de informações disponíveis para você.

&nbsp;
&nbsp;
## Tarefa 3: Explorar os recursos da AWS que oferecem suporte ao seu aplicativo

18. No menu **Services** (Serviços), escolha **EC2**

19. Clique em **Instances** (Instâncias).

    Observe que duas instâncias estão em execução (ambas contêm *samp* em seus nomes). As duas instâncias oferecem suporte ao seu aplicativo web.

20. Você pode continuar explorando os recursos de serviço do Amazon EC2 criados pelo Elastic Beanstalk como desejar. Você encontrará:

    - Um *grupo de segurança* com a porta 80 aberta
    - Um *load balancer* ao qual as duas instâncias pertencem
    - Um *grupo de Auto Scaling* que executa de duas a seis instâncias, dependendo da carga da rede

    O Elastic Beanstalk que criou esses recursos, mas você ainda tem acesso a eles.

&nbsp;
&nbsp;
## Atividade concluída

<i class="icon-flag-checkered"></i> Parabéns! Você concluiu a atividade.

21. Na parte superior desta página, clique em <span id="ssb_voc_grey">End Lab</span> (Encerrar laboratório) e, em seguida, clique em <span id="ssb_blue">Yes</span> (Sim) para confirmar que você deseja encerrar a atividade.

    Um painel é exibido com a mensagem: *DELETE has been initiated... You may close this message box now.* (A EXCLUSÃO foi iniciada... Você pode fechar esta caixa de mensagem agora.).

22. Clique no **X** no canto superior direito para fechar o painel.

Envie feedback, sugestões ou correções por e-mail para: *aws-course-feedback@amazon.com*

&nbsp;
&nbsp;
