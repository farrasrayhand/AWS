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
    #ssb_s3_blue {
    background-color: #329ad6;
    font-weight: bold;
    font-size: 90%;
    color: white;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
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
    #ssb_white {
    background-color: white;
    font-weight: bold;
    font-size: 90%;
    color: #545b64;
    position: relative;
    top: -1px;
    border-color: #545b64;
    border-radius: 2px;
    border-width: 1px;
    border-style: solid;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssl_alexa_ocean {
    color: #00a0d2;
    font-weight: bold;
    }
    #ssb_services {
    background-color: #232f3e;
    font-weight: bold;
    font-size: 90%;
    color: white;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssb_orange {
    background-color:#ec7211;
    font-weight:bold;
    font-size:90%;
    color:white;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    white-space:
    nowrap;
    }
    #ssbox_cloudformation_blue {
    font-weight:bold;
    background-color:#f1faff;
    font-size:90%;
    border-color:#00A1C9;
    border-width:1px;
    border-style:solid;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    }
    #ssb_ssm_white {
    background-color:white;
    font-weight:bold;
    font-size:90%;
    color:#545b64;
    border-color:#545b64;
    border-radius:2px;
    border-width:1px;
    border-style:solid;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    }
 </style>


# Laborat??rio 6: Ajuste a escala e o balanceamento de carga da arquitetura

<!-- Note to translators: This is based on Technical Essentials Lab 3. Copy the translation from there. Do not re-translate the whole document. -->

&nbsp;&nbsp;

**Vers??o 4.6.6 (TESS3) + altera????o personalizada**

Este laborat??rio orienta voc?? a usar os servi??os Elastic Load Balancing (ELB) e Auto Scaling para balancear cargas e dimensionar automaticamente a infraestrutura.

O **Elastic Load Balancing** distribui automaticamente o tr??fego de entrada das aplica????es entre v??rias inst??ncias do Amazon EC2. Ele permite obter toler??ncia a falhas nas aplica????es ao disponibilizar de forma transparente a capacidade necess??ria de balanceamento de carga para rotear o tr??fego das aplica????es.

O **Auto Scaling** ajuda a manter a disponibilidade da aplica????o e permite aumentar ou reduzir a capacidade do Amazon EC2 de forma autom??tica, de acordo com condi????es definidas por voc??. Voc?? pode usar o Auto Scaling para ajudar a garantir que est?? executando o n??mero desejado de inst??ncias do Amazon EC2. O Auto Scaling tamb??m pode aumentar automaticamente o n??mero de inst??ncias do Amazon EC2 durante picos de demanda para manter a performance, e diminuir a capacidade durante per??odos ociosos para reduzir os custos. O Auto Scaling ?? ideal para aplica????es com padr??es de demanda est??veis ou que passam por varia????es de utiliza????o hor??rias, di??rias ou semanais.

&nbsp;

**Objetivos**

Depois de concluir este laborat??rio, voc?? ser?? capaz de:

- Criar uma nova imagem de m??quina da Amazon (AMI) por meio de uma inst??ncia em execu????o;
- Criar um balanceador de carga;
- Criar uma configura????o de execu????o e um grupo de Auto Scaling;
- Ajustar a escala automaticamente de novas inst??ncias em uma sub-rede privada;
- Criar alarmes do Amazon CloudWatch e monitorar a performance da sua infraestrutura.

&nbsp;

**Dura????o**

Este laborat??rio tem dura????o aproximada de **30 minutos**.

&nbsp;

**Cen??rio**

Voc?? come??a com a seguinte infraestrutura:

<img src="images/starting-architecture.png" alt="Arquitetura inicial" width="800">

&nbsp;

O estado final da infraestrutura ??:

&nbsp;

<img src="images/final-architecture.png" alt="Arquitetura final" width="800">

&nbsp;

&nbsp;
___
## Acessar o Console de Gerenciamento da AWS

1. Na parte superior destas instru????es, clique em <span id="ssb_voc_grey">Start Lab</span> (Iniciar laborat??rio) para iniciar o laborat??rio.

   Um painel Start Lab (Iniciar laborat??rio) ?? aberto exibindo o status do laborat??rio.

2. Aguarde at?? ver a mensagem ???**Lab status: in creation** (Status do laborat??rio: em cria????o)??? e clique no **X** para fechar o painel Start Lab (Iniciar laborat??rio).

   **Observa????o**: pode levar aproximadamente 10??minutos ou mais para que o status do laborat??rio mude para ???ready??? (pronto).

3. Na parte superior destas instru????es, clique em <span id="ssb_voc_grey">AWS</span>

   Isso abrir?? o Console de Gerenciamento da AWS em uma nova guia do navegador. O sistema far?? o seu login automaticamente.

   **Dica**: se uma nova guia do navegador n??o for aberta normalmente, um banner ou um ??cone na parte superior do navegador indicar?? que o navegador est?? impedindo que o site abra janelas pop-up. Clique no banner ou ??cone e escolha "Allow pop ups" (Permitir pop-ups).

4. Organize a guia do Console de Gerenciamento da AWS para que ela seja exibida com estas instru????es. Em um cen??rio ideal, voc?? poder?? ver as duas guias do navegador ao mesmo tempo, para facilitar o acompanhamento das etapas do laborat??rio.

&nbsp;
___
## Tarefa 1: Criar uma AMI para o Auto Scaling

Nesta tarefa, voc?? criar?? uma AMI pelo _Servidor da Web 1_ existente. Isso salvar?? o conte??do do disco de inicializa????o para que novas inst??ncias possam ser executadas com conte??do id??ntico.

5. No **Console de Gerenciamento da AWS**, no menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Servi??os), clique em **EC2**.

6. No painel de navega????o esquerdo, clique em **Instances** (Inst??ncias).

   Primeiro, voc?? confirmar?? que a inst??ncia est?? em execu????o.

7. Aguarde at?? que as **Status Checks** (Verifica????es de status) do **Servidor da Web 1** exiba *2/2 checks passed* (2/2 verifica????es aprovadas). Clique em Refresh (Atualizar) <i class="fas fa-sync"></i> para atualizar.

   Agora, voc?? criar?? uma AMI com base nessa inst??ncia.

8. Selecione <i class="far fa-check-square"></i> **Web Server 1** (Servidor da Web 1).

9. No menu <span id="ssb_grey">Actions<i class="fas fa-angle-down"></i></span> (A????es), clique em **Image** (Imagem) > **Create Image** (Criar imagem) e configure:

   - **Image name** (Nome da imagem): `Web Server AMI (AMI do servidor da Web)`
   - **Image description** (Descri????o da imagem): `Lab AMI for Web Server (AMI do laborat??rio para servidor da Web)`

10. Clique em <span id="ssb_blue">Create Image</span> (Criar imagem)

   A tela de confirma????o exibe o **AMI ID** (ID da AMI) para a nova AMI.

11. Clique em <span id="ssb_blue">Close</span> (Fechar)

   Voc?? usar?? essa AMI ao iniciar o grupo de Auto Scaling posteriormente no laborat??rio.

&nbsp;
___
## Tarefa 2: Criar um balanceador de carga

Nesta tarefa, voc?? criar?? um balanceador de carga que pode balancear o tr??fego entre v??rias inst??ncias EC2 e zonas de disponibilidade.

12. No painel de navega????o ?? esquerda, clique em **Load Balancers (Balanceadores de carga)**.

13. Clique em <span id="ssb_blue">Create Load Balancer</span> (Criar balanceador de carga)

   V??rios tipos diferentes de balanceador de carga s??o exibidos. Voc?? usar?? um _Application Load Balancer_ que opera no n??vel de solicita????o (camada 7), roteando o tr??fego para os destinos (inst??ncias EC2, cont??ineres, endere??os IP e fun????es do Lambda) com base no conte??do da solicita????o. Para obter mais informa????es, consulte: <a href="https://aws.amazon.com/elasticloadbalancing/features/#compare" target="_blank">Compara????o de balanceadores de carga</a>

14. Em **Application Load Balancer**, clique em <span id="ssb_blue">Create</span> (Criar) e configure:

   - **Name** (Nome): `LabELB`
   - **VPC:** _Lab VPC_ (na se????o **Availability Zones** (Zonas de disponibilidade))
   - **Availability Zones** (Zonas de disponibilidade): selecione <i class="far fa-check-square"></i> ambas para ver as sub-redes dispon??veis.
   - Selecione **Public Subnet 1** (Sub-rede p??blica 1) e **Public Subnet 2** (Sub-rede p??blica 2)

   Isso configura o balanceador de carga para operar em v??rias zonas de disponibilidade.

15. Clique em <span id="ssb_grey">Next: Configure Security Settings</span> (Pr??ximo: definir configura????es de seguran??a).

   <i class="fas fa-comment"></i> Voc?? pode ignorar o aviso _"Improve your load balancer's security." (Melhore a seguran??a do seu balanceador de carga)_.

16. Clique em <span id="ssb_grey">Next: Configure Security Groups</span> (Pr??ximo: configurar grupos de seguran??a).

   Um _Web Security Group (Grupo de seguran??a da Web)_ j?? foi criado para voc??, o que permite o acesso HTTP.

17. Selecione <i class="far fa-check-square"></i> **Web Security Group** (Grupo de seguran??a da Web) e desmarque <i class="far fa-square"></i> **default** (padr??o).

18. Clique em <span id="ssb_grey">Next: Configure Routing</span> (Pr??ximo: configurar roteamento).

   O roteamento configura para onde enviar as solicita????es que chegam no balanceador de carga. Voc?? criar?? um _Grupo de destino_ que ser?? usado pelo Auto Scaling.

19. Em **Name** (Nome), insira: `LabGroup`

20. Clique em <span id="ssb_grey">Next: Register Targets</span> (Pr??ximo: registrar destinos).

   O Auto Scaling registrar?? automaticamente inst??ncias como destinos posteriormente no laborat??rio.

21. Clique em <span id="ssb_grey">Next: Review</span> (Pr??ximo: revis??o).

22. Clique em <span id="ssb_blue">Create</span> (Criar) e em <span id="ssb_grey">Close</span> (Fechar)

   O balanceador de carga mostrar?? um estado de _provisioning (provisionamento)_. N??o h?? necessidade de esperar at?? que ele esteja pronto. Siga para a pr??xima etapa.

&nbsp;
___
## Tarefa 3: Criar uma configura????o de execu????o e um grupo de Auto Scaling

Nesta tarefa, voc?? criar?? uma _configura????o de execu????o_ para seu grupo de Auto Scaling. Uma configura????o de execu????o ?? um modelo que um grupo de Auto Scaling usa para iniciar inst??ncias do EC2. Ao criar uma configura????o de execu????o, voc?? especifica informa????es para as inst??ncias, como a AMI, o tipo de inst??ncia, um par de chaves, grupo de seguran??a e discos.

23. No painel de navega????o ?? esquerda, clique em **Launch Configurations** (Configura????es de execu????o).

24. Clique em <span id="ssb_orange">Create launch configuration</span> (Criar configura????o de execu????o)

25. Defina estas configura????es:

   - **Launch configuration name**  (Nome da configura????o de execu????o): `LabConfig`

   - **Amazon Machine Image (AMI)** (Imagem de m??quina da Amazon (AMI)): escolha *Web Server AMI (AMI do servidor da Web)*

   - **Instance type** (Tipo de inst??ncia):

      - Selecione <span id="ssb_white">Choose instance type</span> (Escolher tipo de inst??ncia)
      - Selecione *t3.micro*
      - Selecione <span id="ssb_orange">Choose</span> (Escolher)

      **Observa????o:** se voc?? tiver executado o laborat??rio na regi??o us-east-1, selecione o tipo de inst??ncia **t2.micro**. Para localizar a regi??o, procure no canto superior direito do console do Amazon EC2.

      **Observa????o:** se receber a mensagem de erro ???Something went wrong. Please refresh and try again (Algo deu errado. Atualize e tente novamente)???, voc?? poder?? ignor??-la e continuar com o exerc??cio.

   - **Configura????o adicional**

      - **Monitoring** (Monitoramento): </i>Selecione <i class="far fa-check-square"></i> *Enable EC2 instance detailed monitoring within CloudWatch (Habilitar monitoramento detalhado de inst??ncia do EC2 no CloudWatch)*

      Isso permite que o Auto Scaling reaja rapidamente a altera????es na utiliza????o.

26. Em **Security groups** (Grupos de seguran??a), voc?? definir?? a configura????o de execu????o para usar o _Web Security Group (Grupo de seguran??a da Web)_ que criou anteriormente.

   - Escolha **Select an existing security group** (Selecionar um grupo de seguran??a existente)
   - Selecione <i class="far fa-check-square"></i> **Web Security Group** (Grupo de seguran??a da Web)

27. Em **Key pair** (par de chaves), configure:

   - **Key pair options** (Op????es de par de chaves): *Choose an existing key pair (Escolher um par de chaves existente)*
   - **Existing key pair** (par de chaves existente): vockey
   - Selecione <i class="far fa-check-square"></i> **I acknowledge...** (Eu reconhe??o...)
   - Clique em <span id="ssb_orange">Create launch configuration (Criar configura????o de execu????o)</span>

   Agora, voc?? criar?? um grupo de Auto Scaling que usa essa configura????o de execu????o.

28. Marque a caixa de sele????o para a configura????o de execu????o *LabConfig*.

29. No menu <span id="ssb_white">Actions<i class="fas fa-caret-down"></i></span> (A????es), escolha *Create Auto Scaling group* (Criar grupo de Auto Scaling)

30. Insira o nome do grupo de Auto Scaling:

   - **Name** (Nome): `Lab Auto Scaling Group (Grupo de Auto Scaling do laborat??rio)`

31. Escolha <span id="ssb_orange">Next</span> (Pr??ximo).

32. Na p??gina **Network** (Rede), configure

   - **Network** (Rede): _Lab VPC (VPC de laborat??rio)_

      <i class="fas fa-comment"></i> Voc?? pode ignorar a mensagem "No public IP address" (Sem endere??o IP p??blico)

   - **Subnet** (Sub-rede): selecione _Private Subnet 1 (10.0.1.0/24) (Sub-rede privada 1 [10.0.1.0/24])_**e** _Private Subnet 2 (10.0.3.0/24) (Sub-rede privada 2 [10.0.3.0/24])_

   Isso executar?? inst??ncias do EC2 em sub-redes privadas em ambas as zonas de disponibilidade.

33. Escolha <span id="ssb_orange">Next</span> (Pr??ximo).

34. Em **Load balancing** (Balanceamento de carga):

   - Selecione <i class="far fa-check-square"></i> **Enable load balancing** (Habilitar balanceamento de carga)
   - Escolha **Application Load Balancer or Network Load Balancer** (Application Load Balancer ou Network Load Balancer)
   - **Escolha um grupo de destino para o balanceador de carga**: LabGroup

35. Em **Additional settings** (Configura????es adicionais), selecione <i class="far fa-check-square"></i> **Habilitar coleta de m??tricas de grupo no CloudWatch**

   Essa a????o captura m??tricas em intervalos de 1 minuto, o que permite que o Auto Scaling reaja rapidamente a mudan??as nos padr??es de uso.

36. Escolha <span id="ssb_orange">Next</span> (Pr??ximo).

37. Em **Group Size** (Tamanho do grupo), configure:

   - **Desired capacity** (Capacidade desejada): 2
   - **Minimum capacity** (Capacidade m??nima): 2
   - **Maximum capacity** (Capacidade m??xima): 6

   Isso permitir?? que o Auto Scaling adicione/remova inst??ncias automaticamente, mantendo sempre de 2 a 6 inst??ncias em execu????o.

38. Em **Scaling policies** (Pol??ticas de escalabilidade), escolha *Target tracking scaling policy (pol??tica de escalabilidade de rastreamento de destino)* e configure:

   - **Lab policy name** (Nome da pol??tica do laborat??rio): `LabScalingPolicy`
   - **Metric type** (Tipo de m??trica): _Average CPU Utilization (Utiliza????o m??dia de CPU)_
   - **Target value** (Valor de destino): `60`

   Isso informa ao Auto Scaling para manter uma utiliza????o de CPU _m??dia_ em _todas as inst??ncias_ de 60%. O Auto Scaling adicionar?? ou remover?? automaticamente a capacidade conforme necess??rio para manter a m??trica no valor de destino especificado ou pr??ximo a ele. Ele se ajusta ??s flutua????es na m??trica devido a um padr??o de carga flutuante.

39. Escolha <span id="ssb_orange">Next</span> (Pr??ximo).

   O Auto Scaling pode enviar uma notifica????o quando ocorre um evento de escalabilidade. Voc?? usar?? as configura????es padr??o.

40. Escolha <span id="ssb_orange">Next</span> (Pr??ximo).

   As tags aplicadas ao grupo de Auto Scaling ser??o propagadas automaticamente para as inst??ncias executadas.

41. Escolha <span id="ssb_orange">Add tag</span> (Adicionar tag ) e configure o seguinte:

   - **Key** (Chave): `Name (Nome)`
   - **Value** (Valor): `Lab Instance (Inst??ncia de laborat??rio)`

42. Clique em <span id="ssb_orange">Next</span> (Pr??ximo)

43. Revise os detalhes de seu grupo de Auto Scaling e clique em <span id="ssb_orange">Create Auto Scaling group</span> (Criar grupo de Auto Scaling). Se receber a mensagem de erro **Failed to create Auto Scaling group** (Falha ao criar o grupo de Auto Scaling), clique em <span id="ssb_blue">Retry Failed Tasks</span> (Tentar tarefas com falha novamente).

   O grupo de Auto Scaling mostrar?? inicialmente uma contagem de inst??ncias igual a zero, mas novas inst??ncias ser??o executadas para atingir a contagem **Desired** (Desejada) de 2 inst??ncias.

&nbsp;
___
## Tarefa 4: Verificar se o balanceamento de carga est?? funcionando

Nesta tarefa, voc?? verificar?? se o balanceamento de carga est?? funcionando corretamente.

44. No painel de navega????o esquerdo, clique em **Instances** (Inst??ncias).

   Voc?? deve ver duas novas inst??ncias chamadas de **Lab Instance** (Inst??ncia do laborat??rio). Elas foram iniciadas pelo Auto Scaling.

   <i class="fas fa-comment"></i> Se as inst??ncias ou nomes n??o forem exibidos, aguarde 30 segundos e clique em atualizar <i class="fas fa-sync"></i>no canto superior direito.

   Primeiro, voc?? confirmar?? que as novas inst??ncias foram aprovadas na verifica????o de integridade.

45. No painel de navega????o ?? esquerda, clique em **Target Groups** (Grupos de destino), na se????o _Load Balancing (Balanceamento de carga)_.

46. Escolha *LabGroup*

47. Clique na guia **Targets** (Destinos).

   Dois destinos **Lab Instance** (Inst??ncia do laborat??rio) devem ser listados para esse grupo de destino.

48. Aguarde at?? que o **Status** de ambas as inst??ncias mude para *healthy (??ntegro)*. Clique em atualizar <i class="fas fa-sync"></i> no canto superior direito para verificar se h?? atualiza????es.

   O status _Healthy (??ntegro)_ indica que a inst??ncia passou na verifica????o de integridade do balanceador de carga. Isso significa que o balanceador de carga enviar?? tr??fego para a inst??ncia.

   Agora voc?? pode acessar o grupo de Auto Scaling por meio do balanceador de carga.

49. No painel de navega????o ?? esquerda, clique em **Load Balancers** (Balanceadores de carga).

50. No painel inferior, copie o **DNS name** (Nome do DNS) do balanceador de carga, omitindo ???(A Record)??? (Registro A).

   Ele deve ser semelhante a: _LabELB-1998580470.us-west-2.elb.amazonaws.com_

51. Abra uma nova guia do navegador da Web, cole o nome DNS que voc?? acabou de copiar e pressione Enter.

   O aplicativo deve aparecer em seu navegador. Isso indica que o load balancer recebeu a solicita????o, a enviou para uma das inst??ncias do EC2 e, em seguida, repassou o resultado.

&nbsp;
___
## Tarefa 5: testar o Auto Scaling

Voc?? criou um grupo de Auto Scaling com um m??nimo de duas inst??ncias e um m??ximo de seis inst??ncias. Atualmente, duas inst??ncias est??o em execu????o porque o tamanho m??nimo ?? duas e o grupo n??o est?? atualmente sob nenhuma carga. Agora, voc?? aumentar?? a carga para fazer com que o Auto Scaling acrescente inst??ncias adicionais.

52. Volte para o Console de Gerenciamento da AWS, mas n??o feche a guia da aplica????o. Voc?? retornar?? a ele em breve.

53. No menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Servi??os), clique em **CloudWatch**.

54. No painel de navega????o ?? esquerda, clique em **Alarms** (Alarmes) (*n??o em* **ALARM** (ALARME)).

   Dois alarmes ser??o exibidos. Eles foram criados automaticamente pelo grupo de Auto Scaling. Eles manter??o automaticamente a carga m??dia da CPU pr??xima a 60%, permanecendo tamb??m dentro da limita????o de ter duas a seis inst??ncias.

   ???    <i class="fas fa-exclamation-triangle" style="color:red"></i> **Observa????o**: siga estas etapas somente se voc?? n??o vir os alarmes em 60 segundos.

   - No menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Servi??os), clique em **EC2**.
   - No painel de navega????o ?? esquerda, clique em **Auto Scaling Groups** (Grupos de Auto Scaling) e, em seguida, clique em **Scaling Policies** (Pol??ticas de escalabilidade).
   - Clique em <span id="ssb_grey">Actions</span> (A????es) e em **Edit** (Editar).
   - Altere o **Target Value** (Valor de destino) para `50`.
   - Clique em <span id="ssb_blue">Save</span> (Salvar).
   - No menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Servi??os), clique em **CloudWatch**.
   - No painel de navega????o ?? esquerda, clique em **Alarms** (Alarmes) (*n??o em* **ALARM** (ALARME)) e confirme se v?? dois alarmes.



55. Clique no alarme **OK**, que tem _AlarmHigh (Alarme alto)_ no nome.

   <i class="fas fa-comment"></i> Se nenhum alarme estiver mostrando **OK**, aguarde um minuto e clique em Refresh (Atualizar) <i class="fas fa-sync"></i> no canto superior direito at?? que o status do alarme mude.

   O **OK** indica que o alarme _n??o_ foi acionado. ?? o alarme para **CPU Utilization > 60** (Utiliza????o de CPU > 60), que adicionar?? inst??ncias quando a CPU m??dia estiver alta. O gr??fico deve mostrar n??veis muito baixos de CPU no momento.

   Agora, voc?? informar?? ao aplicativo para executar c??lculos que devem aumentar o n??vel de CPU.

56. Volte para a guia do navegador com a aplica????o Web.

57. Clique em **Load Test** (Carregar teste) ao lado do logotipo da AWS.

   Isso far?? com que a aplica????o gere cargas elevadas. A p??gina do navegador ser?? atualizada automaticamente para que todas as inst??ncias no grupo de Auto Scaling gerem carga. N??o feche esta guia.

58. Volte para a guia do navegador com o console do **CloudWatch**.

   Em menos de 5 minutos, o alarme **AlarmLow** (Alarme baixo) dever?? mudar para **OK** e o status do alarme **AlarmHigh** (Alarme alto) dever?? mudar para *ALARM* (ALARME).

   <i class="fas fa-comment"></i> Voc?? pode clicar em Refresh (Atualizar) <i class="fas fa-sync"></i> no canto superior direito a cada 60 segundos para atualizar a exibi????o.

   Voc?? deve ver o gr??fico **AlarmHigh**(Alarme alto) indicando uma porcentagem crescente de CPU. Depois de passar da linha de 60% por mais de 3??minutos, o Auto Scaling acionar?? a adi????o de novas inst??ncias.

59. Aguarde at?? que o alarme **AlarmHigh** (Alarme alto) entre no estado _ALARM (ALARME)_.

   Agora voc?? pode visualizar as inst??ncias adicionais que foram executadas.

60. No menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Servi??os), clique em **EC2**.

61. No painel de navega????o esquerdo, clique em **Instances** (Inst??ncias).

   Agora deve haver mais de duas inst??ncias rotuladas **Lab Instance** (Inst??ncia do laborat??rio) em execu????o. As novas inst??ncias foram criadas pelo Auto Scaling em resposta ao alarme.

&nbsp;
___
## Tarefa 6: Encerrar o servidor da Web 1

Nesta tarefa, voc?? encerrar?? o _Servidor da Web 1_. Essa inst??ncia foi usada para criar a AMI usada por seu grupo de Auto Scaling, mas ela n??o ?? mais necess??ria.

62. Selecione <i class="far fa-check-square"></i> **Web Server 1** (Servidor da Web 1) (e verifique se ?? a ??nica inst??ncia selecionada).

63. No menu <span id="ssb_grey">Actions<i class="fas fa-angle-down"></i></span> (A????es), clique em **Instance State** (Estado da inst??ncia) > **Terminate** (Encerrar).

64. Escolha <span id="ssb_blue">Yes, Terminate</span> (Sim, encerrar).

&nbsp;
___
## Laborat??rio conclu??do

<i class="icon-flag-checkered"></i> Parab??ns! Voc?? concluiu o laborat??rio.

65. Clique em <span id="ssb_voc_grey">End Lab</span> (Encerrar laborat??rio) na parte superior desta p??gina e, em seguida, clique em <span id="ssb_blue">Yes</span> (Sim) para confirmar que voc?? deseja encerrar o laborat??rio.

   Um painel ser?? exibido com a mensagem: "DELETE has be initiated... You may close this message box now." (a EXCLUS??O foi iniciada... Voc?? pode fechar esta caixa de mensagem agora).

66. Clique no **X** no canto superior direito para fechar o painel.

   Envie um e-mail com feedback, sugest??es ou corre????es para: *aws-course-feedback@amazon.com*
