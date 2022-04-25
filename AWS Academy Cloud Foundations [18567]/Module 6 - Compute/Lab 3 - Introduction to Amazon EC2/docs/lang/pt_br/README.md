# Laboratório 3: Introdução ao Amazon EC2

<!-- Note to translators: This is based on SPL-200. Copy the translation from there. Do not re-translate the whole document. -->

**Versão 1.1.7 (spl200)**

## Visão geral

<img src="images/lab-scenario.jpeg" alt="diagrama arquitetônico" width="400">

&nbsp;

Este laboratório apresenta uma visão geral básica de como executar, redimensionar, gerenciar e monitorar uma instância do Amazon EC2.

O **Amazon Elastic Compute Cloud (Amazon EC2)** é um serviço da Web que fornece capacidade computacional redimensionável na nuvem. Ele foi projetado para facilitar aos desenvolvedores a computação em nuvem na escala da Web.

A interface de Web service simples do Amazon EC2 permite que você obtenha e configure capacidade com o mínimo de esforço. Ela oferece um controle completo de seus recursos de computação e permite a execução no ambiente de computação comprovado da Amazon. O Amazon EC2 reduz o tempo necessário para obter e inicializar novas instâncias do servidor em minutos, permitindo o rápido escalonamento da capacidade para mais ou para menos, de acordo com a evolução dos requisitos de computação.

O Amazon EC2 altera a economia da computação, permitindo que você pague somente pela capacidade que realmente utiliza. O Amazon EC2 oferece aos desenvolvedores as ferramentas para criar aplicativos resistentes a falhas e isolá-los de situações de falha comuns.

&nbsp;
### Tópicos abordados

Ao final deste laboratório, você será capaz de:

* Executar um servidor web com proteção contra encerramento habilitada
* Monitorar sua instância EC2
* Modificar o grupo de segurança que seu servidor web está usando para permitir acesso HTTP
* Redimensionar sua instância Amazon EC2 de acordo com a necessidade
* Explorar os limites do EC2
* Testar a proteção contra encerramento
* Encerrar sua instância EC2

&nbsp;

### Duração

O laboratório levará aproximadamente **35 minutos** para ser concluído.

&nbsp;
&nbsp;
## Acessar o Console de Gerenciamento da AWS

1. Na parte superior destas instruções, clique em <span id="ssb_voc_grey">Start Lab (Iniciar laboratório)</span> para iniciar o laboratório.

   Um painel Start Lab (Iniciar laboratório) é aberto exibindo o status do laboratório.

2. Aguarde até que a mensagem “**Lab status: ready** (Status do laboratório: pronto)” seja exibida e clique no **X** para fechar o painel Start Lab (Iniciar laboratório).

3. Na parte superior destas instruções, clique em <span id="ssb_voc_grey">AWS</span>

   Isso abrirá o Console de Gerenciamento da AWS em uma nova guia do navegador. O sistema fará o seu login automaticamente.

   **Dica**: se uma nova guia do navegador não for aberta normalmente, um banner ou um ícone na parte superior do navegador indicará que o navegador está impedindo que o site abra janelas pop-up. Clique no banner ou ícone e escolha "Allow pop ups" (Permitir pop-ups).

4. Organize a guia do Console de Gerenciamento da AWS para que ela seja exibida com estas instruções. Em um cenário ideal, você poderá ver as duas guias do navegador ao mesmo tempo, para facilitar o acompanhamento das etapas do laboratório.

&nbsp;
&nbsp;
## Tarefa 1: Executar a instância do Amazon EC2

Nesta tarefa, você executará uma instância do Amazon EC2 com _proteção contra encerramento_. A proteção contra encerramento impede que você encerre acidentalmente uma instância do EC2. Você implantará sua instância com um script de dados do usuário que permitirá implantar um servidor da Web simples.

5. No **Console de Gerenciamento da AWS**, no menu **Services** (Serviços), clique em **EC2**.

6. Clique em <span id="ssb_orange">Launch instance<i class="fas fa-caret-down"></i></span> (Executar instância) e selecione <span id="ssb_white">Launch instance</span> (Executar instância)

### Etapa 1: Escolher uma imagem de máquina da Amazon (AMI)

<i class="fas fa-info-circle"></i> Uma **imagem de máquina da Amazon (AMI)** fornece as informações necessárias para executar uma instância, que é um servidor virtual na nuvem. Uma AMI inclui:

* Um modelo para o volume raiz da instância (por exemplo, um sistema operacional ou um servidor de aplicativos com aplicativos)
* Permissões de execução que controlam quais contas da AWS podem usar a AMI para executar instâncias
* Um mapeamento de dispositivos de blocos que especifica quais volumes devem ser associados à instância quando ela for executada

A lista **Quick Start** (Início rápido) contém as AMIs mais usadas. Você também pode criar sua própria AMI ou selecionar uma AMI no AWS Marketplace, uma loja online na qual você pode vender ou comprar software executado na AWS.

7. Clique em <span id="ssb_blue">Select</span> (Selecionar) ao lado de **Amazon Linux 2 AMI** (AMI do Amazon Linux 2) (na parte superior da lista).

&nbsp;

### Etapa 2: Escolher um tipo de instância

<i class="fas fa-info-circle"></i> O Amazon EC2 fornece uma ampla seleção de _tipos de instância_ otimizados para atender a diferentes casos de uso. Os tipos de instância compreendem combinações variadas de capacidade de CPU, memória, armazenamento e rede e oferecem a flexibilidade de escolher a combinação adequada de recursos para suas aplicações. Cada tipo de instância inclui um ou mais _tamanhos de instância_, permitindo que você dimensione seus recursos de acordo com os requisitos da sua meta de carga de trabalho.

Você usará uma instância **t2.micro**, que deve estar selecionada <i class="fas fa-square" style="color:blue"></i> por padrão. Esse tipo de instância tem uma CPU virtual e 1 GiB de memória. **OBSERVAÇÃO**: talvez você não possa usar outros tipos de instância neste laboratório.

8. Clique em <span id="ssb_grey">Next: Configure Instance Details</span> (Próximo: Configurar detalhes da instância)

&nbsp;
### Etapa 3: Configurar detalhes da instância

Esta página é usada para configurar a instância de acordo com seus requisitos. Isso inclui configurações de redes e monitoramento.

A **Network** (Rede) indica em qual Virtual Private Cloud (VPC) você deseja executar a instância. Você pode ter várias redes diferentes, por exemplo, para desenvolvimento, teste e produção.

9. Em **Network** (Rede), selecione **Lab VPC** (VPC de laboratório).

   A VPC de laboratório foi criada com base em um modelo do AWS CloudFormation durante o processo de configuração do laboratório. Essa VPC inclui duas sub-redes públicas em duas zonas de disponibilidade diferentes.

10. Em **Enable termination protection** (Habilitar proteção contra encerramento), selecione <i class="far fa-check-square"></i> **Protect against accidental termination** (Proteger contra encerramento acidental).

<i class="fas fa-info-circle"></i> Quando uma instância do Amazon EC2 não é mais necessária, ela pode ser _encerrada_, o que significa que a instância é interrompida e seus recursos são liberados. Uma instância encerrada não pode ser iniciada novamente. Se quiser evitar que a instância seja encerrada acidentalmente, você poderá habilitar a proteção contra _encerramento para a instância_, o que impede que ela seja encerrada.

11. Role para baixo e expanda <i class="fas fa-caret-right"></i> **Advanced Details** (Detalhes avançados).

Um campo para **User data** (Dados do usuário) será exibido.

<i class="fas fa-info-circle"></i>  Ao executar uma instância, você pode passar os _dados do usuário_ para a instância, que podem ser usados para realizar tarefas de configuração automatizadas comuns e até para executar scripts após a inicialização da instância.

A instância executa o Amazon Linux, portanto, você fornecerá um _script do shell_, que será executado quando a instância for iniciada.

12. Copie os seguintes comandos e cole-os no campo **User data** (Dados do usuário):

    ```plain
    #!/bin/bash
    yum -y install httpd
    systemctl enable httpd
    systemctl start httpd
    echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
    ```

    O script:
    
    * Instalará um servidor Web Apache (httpd)
    * Configurará o servidor Web para iniciar automaticamente na inicialização
    * Ativará o servidor Web
    * Criará uma página da Web simples


13. Clique em <span id="ssb_grey">Next: Add Storage</span> (Próximo: Adicionar armazenamento)


&nbsp;
### Etapa 4: Adicionar armazenamento

<i class="fas fa-info-circle"></i> O Amazon EC2 armazena dados em um disco virtual associado à rede chamado de *Elastic Block Store*.

Você executará a instância do Amazon EC2 usando um volume de disco padrão de 8 GiB. Esse será o volume raiz (também conhecido como volume de “inicialização”).

14. Clique em <span id="ssb_grey">Next: Add Tags</span> (Próximo: Adicionar tags).

&nbsp;
### Etapa 5: Adicionar tags

<i class="fas fa-info-circle"></i> As tags permitem categorizar os recursos da AWS de várias maneiras, por exemplo, por finalidade, proprietário ou ambiente. Isso é útil quando você tem muitos recursos do mesmo tipo. É possível identificar rapidamente um recurso específico com base nas tags que você atribuiu a ele. Cada tag consiste em uma chave e um valor, ambos definidos por você.

15. Clique em <span id="ssb_grey">Add Tag</span> (Adicionar tag) e configure:

   * **Key** (Chave): `Name (Nome)`
   * **Value** (Valor): `Web Server (Servidor da Web)`

16. Clique em <span id="ssb_grey">Next: Configure Security Group</span> (Próximo: Configurar grupo de segurança)

&nbsp;
### Etapa 6: Configurar grupo de segurança

<i class="fas fa-info-circle"></i> Um _grupo de segurança_ atua como um firewall virtual que controla o tráfego para uma ou mais instâncias. Ao executar uma instância, você pode associar um ou mais grupos de segurança a ela. Você adiciona _regras_ a cada grupo de segurança que permitem o tráfego de entrada ou de saída das instâncias associadas. É possível modificar as regras para um grupo de segurança a qualquer momento. As novas regras são aplicadas automaticamente a todas as instâncias associadas ao grupo de segurança.

17. Na **Etapa 6: Configurar grupo de segurança**, configure:

   * **O nome do grupo de segurança**:`Web Server security group (Grupo de segurança do servidor da Web)`
   * **Description** (Descrição): `Security group for my web server (Grupo de segurança do meu servidor da Web)`

   Neste laboratório, você não fará login em sua instância usando SSH. A remoção do acesso SSH reforçará a segurança da instância.

18. Exclua <i class="fas fa-times-circle"></i> a regra de SSH existente.

19. Clique em <span id="ssb_blue">Review and Launch</span> (Verificar e iniciar)

&nbsp;
### Etapa 7: Revisar a execução da instância

A página Review (Revisão) exibe a configuração da instância que você está prestes a executar.

20. Clique em <span id="ssb_blue">Launch</span> (Executar).

A janela **Select an existing key pair or create a new key pair (Selecionar um par de chaves existente ou criar um novo par)** será exibida.

<i class="fas fa-info-circle"></i> O Amazon EC2 utiliza criptografia de chave pública para criptografar e descriptografar as informações de login. Para fazer login na sua instância, você deve criar um par de chaves, especificar o nome dele ao executar a instância e inserir a chave privada ao se conectar à instância.

Neste laboratório, você não fará login na instância e, portanto, não precisará de um par de chaves.

21. Clique no menu suspenso **Choose an existing key pair** (Escolher um par de chaves existente)<i class="fas fa-angle-down"></i> e selecione *Proceed without a key pair (Continuar sem um par de chaves)*.

22. Selecione <i class="far fa-check-square"></i> **I acknowledge that...** (Eu confirmo que...).

23. Clique em <span id="ssb_blue">Launch Instances</span> (Executar instâncias)

   Agora sua instância será executada.

24. Clique em <span id="ssb_blue">View Instances</span> (Visualizar instâncias).

   A instância aparecerá em um estado _pending (pendente)_, o que significa que ela está sendo iniciada. Em seguida, ele será alterado para _running (em execução)_, o que indica que a instância foi inicializada. Depois de um curto período, você poderá acessá-la.

   A instância recebe um _nome DNS público_, que você pode usar para acessá-la pela Internet.

   O <i class="fas fa-square" style="color:blue"></i> **servidor da Web** deve estar selecionado. A guia **Description** (Descrição) exibe informações detalhadas da instância.

   <i class="fas fa-comment"></i> Para visualizar mais informações na guia Description (Descrição), arraste o divisor da janela para cima.

   Leia as informações exibidas na guia **Description** (Descrição). Ela inclui informações sobre o tipo de instância e as configurações de segurança e de rede.

25. Aguarde até que a instância exiba o seguinte:

* **Instance State** (Estado da instância): <span style="color:green"><i class="fas fa-circle"></i></span> running (em execução)
* **Status Checks** (Verificações de status):  <span style="color:green"><i class="fas fa-check-circle"></i></span> 2/2 checks passed (2/2 verificações aprovadas)

<span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Parabéns!** Você executou com êxito sua primeira instância do Amazon EC2.

&nbsp;
&nbsp;
## Tarefa 2: Monitorar a instância

O monitoramento é uma parte importante para manter a confiabilidade, disponibilidade e performance das instâncias do Amazon Elastic Compute Cloud (Amazon EC2) e das soluções da AWS.

26. Clique na guia **Status Checks** (Verificações de status).

   <i class="fas fa-info-circle"></i> Com o monitoramento de status de instâncias, você pode rapidamente determinar se o Amazon EC2 detectou problemas que podem impedir suas instâncias de executar aplicações. O Amazon EC2 realiza verificações automáticas em cada instância do EC2 em execução para identificar problemas de hardware e software.

   Observe que as verificações **System reachability** (Acessibilidade do sistema) e **Instance reachability** (Acessibilidade da instância) foram aprovadas.

27. Clique na guia **Monitoring** (Monitoramento).

   Essa guia exibe as métricas do Amazon CloudWatch para sua instância. Atualmente, não há muitas métricas para exibir porque a instância foi executada recentemente.

   Clique em um gráfico para acessar uma visualização expandida.

   <i class="fas fa-info-circle"></i> O Amazon EC2 envia métricas para o Amazon CloudWatch referentes às suas instâncias EC2. Por padrão, o monitoramento básico (cinco minutos) está habilitado. Você pode habilitar o monitoramento detalhado (um minuto).

28. No menu <span id="ssb_grey_square">Actions (Ações)<i class="fas fa-caret-down"></i></span>, selecione **Monitor and troubleshoot** (Monitorar e solucionar problemas) <i class="fas fa-caret-right"></i> **Get System Log** (Obter log do sistema).

   O log do sistema exibe a saída do console da instância, que é uma ferramenta essencial para o diagnóstico de problemas. É especialmente útil para resolver problemas de kernel e problemas de configuração de serviço que possam fazer com que uma instância seja encerrada ou torne-se inalcançável antes de seu daemon SSH ser iniciado. Se você não vir um log do sistema, aguarde alguns minutos e, em seguida, tente novamente.

29. Examine os resultados e observe que o pacote HTTP foi instalado a partir dos **dados do usuário** que você adicionou quando criou a instância.

<img src="images/Console-output.png" alt="Saída do console" width="600">

30. Escolha **Cancel** (Cancelar).

31. No menu <span id="ssb_grey_square">Actions<i class="fas fa-caret-down"></i></span> (Ações), selecione **Monitor and troubleshoot** (Monitorar e solucionar problemas) <i class="fas fa-caret-right"></i> **Get Instance Screenshot** (Obter captura de tela da instância)

   Isso mostra como seria o console da instância do Amazon EC2, se uma tela fosse adicionada a ele.

<img src="images/Screen-shot.png" alt="Captura de tela" width="600">

   <i class="fas fa-info-circle"></i> Se você não conseguir acessar sua instância por SSH ou RDP, poderá fazer uma captura de tela da sua instância e visualizá-la como imagem. Esse procedimento oferece visibilidade do status da instância e permite uma resolução de problemas mais rápida.

32. Escolha **Cancel (Cancelar)**.

   <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Parabéns!** Você explorou várias maneiras de monitorar sua instância.

&nbsp;
&nbsp;
## Tarefa 3: Atualizar o grupo de segurança e acessar o servidor da Web

Ao executar a instância EC2, você forneceu um script que instalou um servidor web e criou uma página da web simples. Nesta tarefa, você acessará o conteúdo do servidor da Web.


33. Clique na guia **Details** (Detalhes).

34. Copie o **IPv4 Public IP** (IP Público IPv4) da instância para a área de transferência.

35. Abra uma nova guia no navegador da Web, cole o endereço IP que você acabou de copiar e pressione **Enter**.

   **Pergunta:** você consegue acessar seu servidor da Web? Por que não?

   Você **não** consegue acessar seu servidor da Web neste momento porque o _grupo de segurança_ não está permitindo o tráfego de entrada na porta 80, que é usada para solicitações da web HTTP. Essa é uma demonstração do uso de um grupo de segurança como firewall para restringir o tráfego de entrada e saída da instância em uma rede.

   Para corrigir isso, você atualizará o grupo de segurança para permitir o tráfego da Web na porta 80.

36. Mantenha a guia do navegador aberta, mas volte para a guia **EC2 Management Console** (Console de gerenciamento do EC2).

37. No painel de navegação esquerdo, clique em **Security Groups** (Grupos de segurança).

38. Selecione <i class="fas fa-square" style="color:blue"></i> **Web Server security group** (Grupo de segurança do servidor da Web).

39. Clique na guia **Inbound** (Entrada).

   No momento, o grupo de segurança não tem regras.

40. Clique em <span id="ssb_grey_square">Edit rules</span> (Editar regras) e configure:

   * **Type** (Tipo): _HTTP_
   * **Source** (Fonte): _Anywhere (Qualquer lugar)_
   * Clique em <span id="ssb_orange">Save rules</span> (Salvar regras)

41. Volte para a guia do servidor Web que você abriu anteriormente e atualize <i class="fas fa-sync"></i> a página.

   Você deve ver a mensagem _Hello From Your Web Server! (Olá do seu servidor da Web!)_

   <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Parabéns!** Você modificou com êxito o grupo de segurança para permitir tráfego HTTP de entrada em sua instância do Amazon EC2.

&nbsp;
&nbsp;
## Tarefa 4: Redimensionar a instância - tipo de instância e volume EBS

À medida que suas necessidades mudam, você pode avaliar que sua instância está sendo utilizada em excesso (muito pequena) ou subutilizada (muito grande). Se isso acontecer, você poderá alterar o _tipo de instância_. Por exemplo, se uma instância _t2.micro_ for muito pequena para sua carga de trabalho, você poderá alterá-la para uma instância _m5.medium_. Da mesma forma, também poderá alterar o tamanho de um disco.


### Interromper a instância

Antes de redimensionar uma instância, você deve _interrompê-la_.

<i class="fas fa-info-circle"></i> Quando você interrompe uma instância, ela é desligada. Não há cobrança para uma instância EC2 interrompida, mas a cobrança de armazenamento para volumes do Amazon EBS associados permanece.

42. No **EC2 Management Console** (Console de gerenciamento do EC2), no painel de navegação esquerdo, clique em **Instances** (Instâncias).

   <i class="fas fa-square" style="color:blue"></i> O **servidor Web** já deve estar selecionado.

43. No menu <span id="ssb_grey_square">Instance State<i class="fas fa-caret-down"></i></span> (Estado da instância), selecione **Stop instance** (Parar instância).

44. Escolha <span id="ssb_orange">Stop</span> (Parar)

   A instância executará um desligamento normal e, em seguida, interromperá a execução.

45. Aguarde até que o **Instance State** (Estado da instância) exiba: <span style="color:red"><i class="fas fa-circle"></i></span> stopped (interrompido)

### Alterar o tipo de instância

46. No menu <span id="ssb_grey_square">Actions (Ações)<i class="fas fa-caret-down"></i></span>, selecione **Instance Settings** (Configurações da instância) <i class="fas fa-caret-right"></i> **Change Instance Type** (Alterar tipo de instância) e configure:

   * **Instance type** (Tipo de instância): * t2.small*
   * Clique em <span id="ssb_orange">Apply</span> (Aplicar).

   Quando a instância for iniciada novamente, ela será uma _t2.small_, que tem duas vezes mais memória que uma instância _t2.micro_. **OBSERVAÇÃO**: talvez você não possa usar outros tipos de instância neste laboratório.

### Redimensionar o volume EBS

47. No menu de navegação esquerdo, clique em **Volumes**.

48. No menu <span id="ssb_grey">Actions (Ações)<i class="fas fa-caret-down"></i></span>, selecione **Modify Volume** (Modificar volume).

   Atualmente, o volume do disco é de 8 GiB. Agora, você aumentará o tamanho desse disco.

49. Altere o tamanho para: `10` **OBSERVAÇÃO**: talvez você não possa criar grandes volumes do Amazon EBS neste laboratório.

50. Selecione <span id="ssb_grey">Modify</span> (Modificar).

51. Clique em <span id="ssb_blue">Yes</span> (Sim) para confirmar e aumentar o tamanho do volume.

52. Clique em <span id="ssb_blue">Close</span> (Fechar)

### Iniciar a instância redimensionada

Neste momento, você iniciará a instância novamente, que agora terá mais memória e mais espaço em disco.

53. No painel de navegação esquerdo, clique em **Instances** (Instâncias).

54. No menu <span id="ssb_grey_square">Instance State<i class="fas fa-caret-down"></i></span> (Estado da instância), selecione **Start instance** (Iniciar instância).

55. Selecione <span id="ssb_orange">Start</span> (Iniciar)

   <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Parabéns!** Você redimensionou com êxito sua instância do Amazon EC2. Nessa tarefa, você alterou o tipo de instância de _t2.micro_ para _t2.small_. Você também modificou o volume do disco raiz de 8 GiB para 10 GiB.

&nbsp;
&nbsp;
## Tarefa 5: Explorar os limites do EC2

O Amazon EC2 oferece recursos diferentes que você pode usar. Esses recursos incluem imagens, instâncias, volumes e snapshots. Quando você cria uma conta da AWS, há limites padrão para esses recursos, de acordo com a região.

56. No painel de navegação esquerdo, clique em **Limits** (Limites).

57. Na lista suspensa, escolha **Running instances** (Execução de instâncias).

   Observe que há um limite no número de instâncias que você pode iniciar nesta região. Ao executar uma instância, a solicitação não deve fazer com que seu uso exceda o limite de instâncias atual nessa região.

   É possível solicitar o aumento de muitos desses limites.

&nbsp;
&nbsp;
## Tarefa 6: Testar a proteção contra encerramento

Você pode excluir sua instância quando não precisar mais dela. Esse procedimento é chamado de _encerramento_ da instância. Você não poderá se conectar ou reiniciar uma instância depois que ela tiver sido encerrada.

Nesta tarefa, você aprenderá a usar a _proteção contra encerramento_.

58. No painel de navegação esquerdo, clique em **Instances** (Instâncias).

59. No menu <span id="ssb_grey_square">Instance State<i class="fas fa-caret-down"></i></span> (Estado da instância), selecione **Terminate instance** (Encerrar instância).

60. Em seguida, escolha <span id="ssb_orange">Terminate</span> (Encerrar)

   Observe que há uma mensagem informando: *Failed to terminate the instance i-1234567xxx. The instance 'i-1234567xxx' may not be terminated. Modify its 'disableApiTermination' instance attribute and try again (Falha ao encerrar a instância i-1234567xxx. Não é possível encerrar a instância “i-1234567xxx”. Modifique o atributo “disableApiTermination”da instância e tente novamente)*.

   Essa é uma proteção para evitar o encerramento acidental de uma instância. Se você realmente desejar encerrar a instância, será necessário desabilitar a proteção de encerramento.

61. No menu <span id="ssb_grey_square">Actions<i class="fas fa-caret-down"></i></span> (Ações), selecione **Instance Settings** (Configurações da instância) <i class="fas fa-caret-right"></i> **Change Termination Protection** (Alterar proteção contra encerramento).

62. Remova a seleção ao lado de <i class="far fa-square"></i> **Enable** (Habilitar)

63. Clique em <span id="ssb_orange">Salve</span> (Salvar)

   Agora você pode encerrar a instância.

64. No menu <span id="ssb_grey_square">Instance State<i class="fas fa-caret-down"></i></span> (Estado da instância), selecione **Terminate** (Encerrar).

65. Escolha <span id="ssb_orange">Terminate</span> (Encerrar)

   <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Parabéns!** Você testou com êxito a proteção contra encerramento e encerrou sua instância.

&nbsp;
&nbsp;

## Laboratório concluído

<i class="icon-flag-checkered"></i> Parabéns! Você concluiu o laboratório.

66. Clique em <span id="ssb_voc_grey">End Lab</span> (Encerrar laboratório) na parte superior desta página e, em seguida, clique em <span id="ssb_blue">Yes</span> (Sim) para confirmar que você deseja encerrar o laboratório.  

   Um painel será exibido com a mensagem: "DELETE has be initiated... You may close this message box now." (a EXCLUSÃO foi iniciada... Você pode fechar esta caixa de mensagem agora).

67. Clique no **X** no canto superior direito para fechar o painel.

Envie um e-mail com feedback, sugestões ou correções para: *aws-course-feedback@amazon.com*

&nbsp;
&nbsp;
## Recursos adicionais

* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html" target="_blank">Executar sua instância</a>
* <a href="https://aws.amazon.com/ec2/instance-types" target="_blank">Tipos de instância Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html" target="_blank">Imagens de máquina da Amazon (AMIs)</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html" target="_blank">Amazon EC2 - Dados do usuário e scripts de shell</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html" target="_blank">Volume do dispositivo raiz do Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html" target="_blank">Como atribuir tags aos seus recursos do Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html" target="_blank">Grupos de segurança</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html" target="_blank">Pares de chaves do Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html?icmpid=docs_ec2_console" target="_blank">Verificações de status para suas instâncias</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-console.html" target="_blank">Como acessar a saída do console e reinicializar instâncias</a>
* <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ec2-metricscollected.html" target="_blank">Métricas e dimensões do Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html" target="_blank">Como redimensionar a instância</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html" target="_blank">Interromper e iniciar a instância</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html" target="_blank">Limites de serviço do Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html" target="_blank">Encerrar sua instância</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html" target="_blank">Proteção contra encerramento de uma instância</a>
