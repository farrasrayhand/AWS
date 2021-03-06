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
## Vis??o geral

Esta atividade disponibiliza para voc?? uma conta da Amazon Web Services (AWS) com um ambiente do AWS Elastic Beanstalk pr??-criado. Voc?? implantar?? um c??digo nela e observar?? os recursos da AWS que comp??em o ambiente do Elastic Beanstalk.

&nbsp;

### Dura????o

Esta atividade levar?? aproximadamente **30??minutos** para ser conclu??da.

&nbsp;
&nbsp;
## Como acessar o Console de Gerenciamento da AWS

1. Na parte superior destas instru????es, clique em <span id="ssb_voc_grey">Start Lab</span> (Iniciar laborat??rio) para iniciar o laborat??rio.

   Um painel **Start Lab** (Iniciar laborat??rio) ?? aberto exibindo o status do laborat??rio.

2. Aguarde at?? ver a mensagem *Lab status: in creation* (Status do laborat??rio: em cria????o). Para fechar o painel **Start Lab** (Iniciar laborat??rio), clique no **X**.

3. Na parte superior destas instru????es, clique em <span id="ssb_voc_grey">AWS</span>.

   O Console de Gerenciamento da AWS ?? aberto em uma nova guia do navegador. O sistema faz o seu login automaticamente.

   **Dica**: se uma nova guia do navegador n??o abrir, normalmente haver?? um banner ou um ??cone na parte superior do navegador indicando que o navegador est?? impedindo que o site abra janelas pop-up. Clique no banner ou ??cone e escolha **Allow pop ups** (Permitir pop-ups).

4. Organize a guia do **Console de Gerenciamento da AWS** para que ela seja exibida com estas instru????es. Em um cen??rio ideal, voc?? poder?? ver as duas guias do navegador ao mesmo tempo, o que facilita o acompanhamento das etapas da atividade.

&nbsp;
&nbsp;
## Tarefa 1: Acessar o ambiente do Elastic Beanstalk

5. No **Console de Gerenciamento da AWS**, no menu **Services** (Servi??os), escolha **Elastic Beanstalk**.

   Uma p??gina intitulada **All Applications** (Todos os aplicativos) deve ser aberta com uma caixa verde que lista os detalhes de um aplicativo existente do Elastic Beanstalk.

   **Observa????o**: se a caixa n??o estiver verde, ela ainda n??o foi totalmente iniciada. Aguarde alguns instantes, e ela dever?? mudar para *Health status: Green* (Status de integridade: verde).

   <img src="images/application.png" alt="elastic-beanstalk-app" width="600" style="border:1px solid black">

6. Na caixa verde de detalhes do aplicativo, clique no valor de URL exibido.

   A p??gina **Dashboard** (Painel) do ambiente do Elastic Beanstalk ?? aberta.

7. Observe que a p??gina mostra a integridade do seu aplicativo em verde (bom).

   O ambiente do Elastic Beanstalk est?? pronto para hospedar um aplicativo. No entanto, ele ainda n??o tem c??digo em execu????o.

8. Pr??ximo ?? parte superior da p??gina, clique na URL (a URL termina com *elasticbeanstalk.com*).

   Quando voc?? clica na URL, uma nova guia do navegador ?? aberta. No entanto, voc?? deve observar que ela exibe a mensagem *"HTTP Status 404 - Not Found"* (HTTP Status 404 - N??o encontrado). *Esse comportamento ?? esperado*, porque esse servidor de aplicativos ainda n??o tem nenhum aplicativo em execu????o. Retorne ao console do Elastic Beanstalk.

   Na pr??xima etapa, voc?? implantar?? o c??digo em seu ambiente do Elastic Beanstalk.

&nbsp;
&nbsp;
## Tarefa 2: Implantar um aplicativo de exemplo no Elastic Beanstalk

9. Para fazer download de um aplicativo de exemplo, clique neste link:
   https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/tomcat.zip

<!--the zip file is linked in this documentation page: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-getstarted.html-->

10. Retorne ao console do Elastic Beanstalk e clique em **Upload and Deploy** (Fazer upload e implantar).

11. Clique em **Browse or Choose File** (Procurar ou escolher arquivo), navegue at?? o arquivo **java-tomcat-v3.zip** do qual voc?? acabou de fazer download e abra-o.

12. Clique em **Deploy** (Implantar).

    Levar?? um ou dois minutos para o Elastic Beanstalk atualizar seu ambiente e implantar o aplicativo.

    **Observa????o**: se aparecer um aviso na p??gina do painel do Elastic Beanstalk informando que um perfil de inst??ncia ?? necess??rio para integra????o com o servi??o do AWS X-Ray, ignore esse aviso.

13. Ap??s a conclus??o da implanta????o, clique no valor de URL pr??ximo ?? parte superior da tela (ou, se a guia do navegador que exibia o status 404 ainda estiver aberta, atualize essa p??gina).

    O aplicativo web que voc?? implantou ?? exibido.

    <img src="images/web-app.png" alt="web-app" width="600" style="border:1px solid black">

    Parab??ns! Voc?? implantou com sucesso um aplicativo no Elastic Beanstalk.

14. Retorne ao console do Elastic Beanstalk e clique em **Configuration** (Configura????o).

    Observe os detalhes aqui.

    Por exemplo, na linha **Instances** (Inst??ncias), ele mostra o tipo de inst??ncia, o intervalo de monitoramento e os detalhes do grupo de seguran??a das inst??ncias Amazon Elastic Compute Cloud (Amazon EC2) que hospedam seu aplicativo web.

15. Role at?? a linha **Database** (Banco de dados) na parte inferior da p??gina.

    A linha **Database** (Banco de dados) est?? sem detalhes porque o ambiente n??o inclui um banco de dados.

16. Na linha **Database** (Banco de dados), clique em **Modify** (Modificar).

    Observe que, se desejado, voc?? pode adicionar facilmente um banco de dados a esse ambiente, basta definir algumas configura????es b??sicas e clicar em **Apply** (Aplicar). (No entanto, para a finalidade desta atividade, voc?? n??o precisa adicionar um banco de dados.)

17. Clique em **Monitoring** (Monitoramento).

    Navegue pelos gr??ficos para ver os tipos de informa????es dispon??veis para voc??.

&nbsp;
&nbsp;
## Tarefa 3: Explorar os recursos da AWS que oferecem suporte ao seu aplicativo

18. No menu **Services** (Servi??os), escolha **EC2**

19. Clique em **Instances** (Inst??ncias).

    Observe que duas inst??ncias est??o em execu????o (ambas cont??m *samp* em seus nomes). As duas inst??ncias oferecem suporte ao seu aplicativo web.

20. Voc?? pode continuar explorando os recursos de servi??o do Amazon EC2 criados pelo Elastic Beanstalk como desejar. Voc?? encontrar??:

    - Um *grupo de seguran??a* com a porta 80 aberta
    - Um *load balancer* ao qual as duas inst??ncias pertencem
    - Um *grupo de Auto Scaling* que executa de duas a seis inst??ncias, dependendo da carga da rede

    O Elastic Beanstalk que criou esses recursos, mas voc?? ainda tem acesso a eles.

&nbsp;
&nbsp;
## Atividade conclu??da

<i class="icon-flag-checkered"></i> Parab??ns! Voc?? concluiu a atividade.

21. Na parte superior desta p??gina, clique em <span id="ssb_voc_grey">End Lab</span> (Encerrar laborat??rio) e, em seguida, clique em <span id="ssb_blue">Yes</span> (Sim) para confirmar que voc?? deseja encerrar a atividade.

    Um painel ?? exibido com a mensagem: *DELETE has been initiated... You may close this message box now.* (A EXCLUS??O foi iniciada... Voc?? pode fechar esta caixa de mensagem agora.).

22. Clique no **X** no canto superior direito para fechar o painel.

Envie feedback, sugest??es ou corre????es por e-mail para: *aws-course-feedback@amazon.com*

&nbsp;
&nbsp;
