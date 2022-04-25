# Módulo 10 – Laboratório guiado: Automatizar a implantação da infraestrutura com o AWS CloudFormation

[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: ILT-TF-100-ARCHIT-6 branch dev_65"

## Visão geral e objetivos do laboratório

Implantar uma infraestrutura de maneira consistente e confiável é difícil, pois requer que as pessoas sigam procedimentos documentados sem pegar atalhos não registrados. Além disso, pode ser difícil implantar a infraestrutura fora do horário comercial, quando há menos funcionários disponíveis. O AWS CloudFormation altera isso definindo a infraestrutura em um modelo que pode ser implantado automaticamente, mesmo em uma programação automatizada.

Neste laboratório, você aprenderá como implantar várias camadas de infraestrutura com o AWS CloudFormation, atualizar uma pilha do CloudFormation e excluir uma pilha (mantendo alguns recursos).

Depois de concluir este laboratório, você deverá ser capaz de:

- Use o AWS CloudFormation para implantar uma camada de rede de virtual private cloud (VPC)
- Usar o AWS CloudFormation para implantar uma camada de aplicação que faz referência à camada de rede
- Examinar os modelos com o AWS CloudFormation Designer
- Excluirá uma pilha que tem uma política de exclusão

<br/>

## Duração

Este laboratório levará aproximadamente **20 minutos** para ser concluído.

<br/>

## Restrições de serviço da AWS

Neste ambiente de laboratório, o acesso aos serviços e ações de serviço da AWS pode estar restrito àqueles necessários para concluir as instruções do laboratório. Você poderá encontrar erros se tentar acessar outros serviços ou executar ações além do que está descrito neste laboratório.

<br/>

## Acessar o Console de gerenciamento da AWS

1. Na parte superior destas instruções, escolha <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratório) para iniciar o laboratório.

   Um painel **Start Lab** (Iniciar laboratório) é aberto com o status do laboratório.

   <i class="fas fa-info-circle"></i> **Dica**: se você precisar de mais tempo para concluir o laboratório, escolha novamente o botão <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratório) para reiniciar o cronômetro do ambiente.

2. Aguarde até que o painel **Start Lab** (Iniciar laboratório) exiba a mensagem *Lab status: ready* (Status do laboratório: pronto) e, em seguida, escolha **X** para fechar o painel.

3. Na parte superior destas instruções, escolha <span id="ssb_voc_grey">AWS</span>.

   Essa ação abrirá o Console de Gerenciamento da AWS em uma nova guia do navegador. O sistema fará seu login automaticamente.

   <i class="fas fa-exclamation-triangle"></i> **Dica**: se uma nova guia do navegador não for aberta, normalmente você verá uma faixa ou um ícone na parte superior do navegador com uma mensagem informando que o navegador está impedindo que o site abra janelas pop-up. Escolha a faixa ou o ícone e depois **Allow pop ups** (Permitir pop-ups).

4. Organize a guia do **Console de Gerenciamento da AWS** para que ela seja exibida com essas instruções. O ideal é ver as duas guias do navegador abertas ao mesmo tempo, para facilitar o acompanhamento das etapas do laboratório.

   <i class="fas fa-exclamation-triangle"></i> **Não altere a região, a menos que seja especificamente instruído a fazer isso**.

<br/>

## Tarefa 1: implantar uma camada de rede

É uma melhor prática implantar a infraestrutura em _camadas_. As camadas comuns são:

- Rede (Amazon VPC)
- Banco de dados
- Aplicação

Desta forma, os modelos podem ser reutilizados entre os sistemas. Por exemplo, você pode implantar uma topologia de rede comum entre ambientes de desenvolvimento, teste e produção ou implantar um banco de dados padrão para vários aplicações.

Nessa tarefa, você implantará um modelo do AWS CloudFormation que cria uma _camada de rede_ usando o Amazon VPC.

5. Clique com o botão direito do mouse no link a seguir e faça o download do modelo para o seu computador: [lab-network.yaml](../../../scripts/lab-network.yaml)

   <i class="fas fa-comment"></i> Se você quiser ver como os recursos da AWS são definidos, poderá abrir o modelo em um editor de texto.

   Os modelos podem ser escritos em JavaScript Object Notation (JSON) ou YAML Ain't Markup Language (YAML). YAML é uma linguagem de marcação semelhante ao JSON, mas é mais fácil de ler e editar.

6. No **Console de Gerenciamento da AWS**, no menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Serviços), escolha **CloudFormation**.

7. Se você vir esta mensagem, clique em <span style="color:blue">Try it out now and provide us feedback (Experimente agora e dê um feedback)</span>:

   ![Novo console](images/new-console.png)

8. Escolha <span id="ssb_orange">Create stack</span> e configure estes ajustes.

   **Etapa 1: Especificar o modelo**

   - **Origem do modelo:** <span id="ssbox_cloudformation_blue"><i class="far fa-dot-circle" style="color:#007dbc;"></i> Fazer upload de um arquivo de modelo</span>
   - **Fazer upload de um arquivo de modelo:** clique em <span id="ssb_ssm_white">Choose file</span> (Escolher arquivo) e selecione o arquivo **lab-network.yaml** obtido por download.
   - Escolha <span id="ssb_orange">Next</span> (Próximo)

   **Etapa 2: criar pilha**

   - **Stack name** (Nome da pilha): `lab-network`
   - Escolha <span id="ssb_orange">Next</span> (Próximo)

   **Etapa 3: configurar as opções de pilha**

   - Na seção **Tags**, insira esses valores.
      - **Key** (Chave): `application`
      - **Value** (Valor): `inventory`
   - Escolha <span id="ssb_orange">Next</span> (Próximo)

   **Etapa 4: revisar a rede do laboratório**

   - Selecione <span id="ssb_orange">Create stack</span> (Criar pilha)

   O *modelo* agora será usado pelo AWS CloudFormation para gerar uma *pilha* de recursos na conta da AWS.

   As *tags* especificadas serão automaticamente propagadas para os recursos que são criados, facilitando a identificação dos recursos usados por aplicações específicos.

9. Escolha a guia **Stack info** (Informações sobre a pilha).

10. Aguarde até que o **Status** mude para <span style="color: green;"><i class="far fa-check-circle"></i>CREATE_COMPLETE</span>.

    <i class="fas fa-comment"></i> Escolha **Refresh** (Atualizar) a <i class="fas fa-redo"></i> cada 15 segundos para atualizar a exibição, se necessário.

    Agora você pode examinar os recursos que foram criados.

11. Escolha a guia **Resources** (Resources).

    Você verá uma lista de recursos criados pelo modelo.

    <i class="fas fa-comment"></i> Se a lista estiver vazia, atualize a lista escolhendo **Atualizar** <i class="fas fa-redo"></i>.

12. Escolha a guia **Event** (Eventos) e percorra o log de eventos.

    O log de eventos mostra (de mais recente para menos recente) as atividades que foram realizadas pelo AWS CloudFormation. Eventos de exemplo incluem começar a criar um recurso e, em seguida, concluir a criação do recurso. Quaisquer erros que foram encontrados durante a criação da pilha serão listados nesta guia.

13. Escolha a guia **Outputs** (Saídas).

    Uma pilha do CloudFormation pode fornecer _informações de saída_, como o ID de recursos específicos e links para recursos.

    Duas saídas estão listadas.

    - **PublicSubnet: ** O ID da sub-rede pública que foi criada (por exemplo: _subnet-08aafd57f745035f1__
    - **VPC: ** O ID da VPC que foi criada (por exemplo: _vpc-08e2b7d1272ee9fb4_)

    As saídas também podem ser usadas para fornecer valores a outras pilhas. Isto é mostrado na coluna **Nome da exportação**. Nesse caso, a VPC e os IDs de sub-rede recebem nomes de exportação para que outras pilhas possam recuperar os valores. Essas outras pilhas podem criar recursos dentro da VPC e da sub-rede que acabaram de ser criados. Você usará esses valores na próxima tarefa.

14. Escolha a guia **Template** (Modelo).

    Esta guia mostra o modelo usado para criar a pilha, ou seja, o modelo que você carregou enquanto criou a pilha. Sinta-se livre para examinar o modelo e ver os recursos que foram criados. Também sinta-se livre para explorar a seção **Saídas** no final (esta seção definiu quais valores exportar).

<br/>
## Tarefa 2: implantar uma camada de aplicação

Agora que a _camada de rede_ está implantada, é hora de implantar uma _camada de aplicação_ que contenha uma instância do Amazon Elastic Compute Cloud (Amazon EC2) e um grupo de segurança.

O modelo do AWS CloudFormation _importará_ a VPC e os IDs de sub-rede das _saídas_ da pilha existente do CloudFormation. Em seguida, ele usará essas informações para criar o grupo de segurança na VPC e na instância do EC2 na sub-rede.

15. Clique com o botão direito do mouse no link a seguir e faça download do modelo para o seu computador: [lab-application.yaml](../../../scripts/lab-application.yaml)

    <i class="fas fa-comment">Observação</i> Se você quiser ver como os recursos são definidos, poderá abrir o modelo em um editor de texto.

16. No painel de navegação à esquerda, selecione**Stacks** (Pilhas).

17. Selecione **Criar pilha > Com novos recursos (padrão)**e, em seguida, defina essas configurações.

    **Etapa 1: Especificar o modelo**

    - **Fonte do modelo: ** <span id="ssbox_cloudformation_blue"> <i class="far fa-dot-circle" style="color:#007dbc;"> </i> Carregar um arquivo de modelo
    - **Carregar um arquivo de modelo: ** Clique em <span id="ssb_ssm_white">Choose file</span> (Escolher arquivo) e selecione o arquivo **lab-application.yaml** que você baixou.
    - Escolha <span id="ssb_orange"> Próximo

    **Etapa 2: criar pilha**

    - **Stack name** (Nome da pilha): `lab-application`
    - **NetworkStackName:** `lab-network`
    - Escolha <span id="ssb_orange">Next</span> (Próximo)

    <i class="fas fa-comment"></i> O parâmetro _Network Stack Name (Nome da pilha de rede)_ informa ao modelo o nome da primeira pilha que você criou (_lab-network_), para que você possa recuperar valores de _Outputs (Saídas)_.

    **Etapa 3: configurar as opções de pilha**

    - Na seção **Tags**, insira esses valores.
     - **Key (Chave): ** `application`
     - **Value (Valor): ** `inventory`
    - Escolha <span id="ssb_orange">Next</span> (Próximo)

    **Etapa 4: revisar a aplicação de laboratório**

    - Escolha <span id="ssb_orange">Create stack</span> (Criar pilha)

    Enquanto a pilha estiver sendo criada, examine os detalhes na guia **Events** (Eventos) e na guia **Resources** (Recursos). Você pode monitorar o andamento do processo de criação de recursos e o status do recurso.

18. Na guia **Informações da pilha**, aguarde até que o **Status** mude para <span style="color: green;"><i class="far fa-check-circle"></i>CREATE_COMPLETE</span>.

    Sua aplicação agora está pronta.

19. Escolha a guia **Outputs** (Saídas).

20. Copie o **URL** exibido e abra uma nova guia do navegador da Web. Cole o URL e pressione ENTER.

    A guia do navegador abrirá a aplicação que está sendo executada no servidor Web, criada por essa nova pilha do CloudFormation.

    Uma pilha do CloudFormation pode usar valores de referência de outra pilha do CloudFormation. Por exemplo, esta parte do modelo de _aplicação de laboratório_ faz referência ao modelo _de rede de laboratório_:

    ```yaml
      WebServerSecurityGroup:
        Type: AWS::EC2::SecurityGroup
        Properties:
          GroupDescription: Enable HTTP ingress
          VpcId:
            Fn::ImportValue:
              !Sub ${NetworkStackName}-VPCID
    ```

    A última linha usa o _nome da pilha de rede_ que você forneceu (_lab-network_) quando a pilha foi criada. Ela importa o valor de _Lab-Network-VPCID_ das _saídas_ da primeira pilha. Em seguida, ela insere o valor no campo VPC ID da definição do grupo de segurança. O resultado é que o grupo de segurança é criado na VPC que foi criado pela primeira pilha.

    Aqui está outro exemplo, que está no modelo do CloudFormation que você acabou de usar para criar a pilha de aplicações. Este código de modelo coloca a instância do EC2 na sub-rede criada pela pilha de rede:

    ```yaml
      SubnetId:
        Fn::ImportValue:
        !Sub ${NetworkStackName}-SubnetID
    ```

    Ele pega o _ID de sub-rede_ da pilha _lab-network_ e o usa na pilha _lab-application_ para executar a instância na sub-rede pública criada pela primeira pilha.


<br/>
## Tarefa 3: atualizar uma pilha

O AWS CloudFormation também pode _atualizar_ uma pilha que foi implantada. Quando você atualiza uma pilha, o AWS CloudFormation só modificará ou substituirá os recursos que estão sendo alterados. Quaisquer recursos que não estão sendo alterados serão deixados como estão.

Nessa tarefa, você atualizará a pilha _lab-application_ para modificar uma configuração no grupo de segurança.

Primeiro, você examinará as configurações atuais no grupo de segurança.

21. No **Console de Gerenciamento da AWS**, no menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Serviços), escolha **EC2**.

22. No painel de navegação à esquerda, escolha **Security Groups** (Grupos de segurança).

23. Marque a caixa de seleção para <i class="far fa-check-square"></i> **Aplicação de laboratório WebServerSecurityGroup...**.

24. Escolha a guia **Inbound Rules** (Regras de entrada).

    Atualmente, apenas uma regra está no grupo de segurança. A regra permite o tráfego _HTTP_.

    Agora, volte para o AWS CloudFormation para atualizar a pilha.

25. No menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Serviços), escolha **CloudFormation**.

26. Clique com o botão direito do mouse neste link e faça download do modelo atualizado para o seu computador: [lab-application2.yaml](../../../scripts/lab-application2.yaml)

    Esse modelo tem uma configuração adicional que permite o tráfego SSH de entrada na porta 22:

    ```yaml
      - IpProtocol: tcp
        FromPort: 22
        ToPort: 22
        CidrIp: 0.0.0.0/0
    ```

27. Na lista **Stacks** do **console do AWS CloudFormation**, selecione **aplicação de laboratório**.

28. Escolha a <span id="ssb_ssm_white">atualização</span> e defina estes ajustes.

    - Selecione <span id="ssbox_cloudformation_blue"><i class="far fa-dot-circle" style="color:#007dbc;"></i> Replace current template</span> (Substituir modelo atual)
    - **Origem do modelo:** <span id="ssbox_cloudformation_blue"><i class="far fa-dot-circle" style="color:#007dbc;"></i> Fazer upload de um arquivo de modelo</span>
    - **Fazer upload de um arquivo de modelo:** clique em <span id="ssb_ssm_white">Choose file</span> (Escolher arquivo) e selecione o arquivo **lab-application2.yaml** obtido por download.

29. Escolha <span id="ssb_orange">Próximo</span> em cada uma das próximas *três* telas para avançar para a página **Revisar aplicação de laboratório**.

    Na seção **Change set preview (Alterar visualização do conjunto)** na parte inferior da página, o AWS CloudFormation exibe os recursos que serão atualizados:

    ![Change set preview (Alterar visualização do conjunto)](images/change-set-preview.png)

    Essa visualização do conjunto de alterações indica que o AWS CloudFormation *modificará* o *WebServerSecurityGroup* sem precisar substituí-lo (_Replacement = False_). Esse conjunto de alterações significa que o grupo de segurança terá uma pequena alteração aplicada a ele, e nenhuma referência ao grupo de segurança precisará ser alterada.

30. Escolha <span id="ssb_orange">Update stack</span> (Atualizar pilha)

31. Na guia **Stack info** (Informações sobre a pilha), aguarde até o **Status** mudar para <span style="color: green;"><i class="far fa-check-circle"></i>UPDATE_COMPLETE</span>.

    <i class="fas fa-comment"></i> Atualize o status escolhendo **Atualizar** <i class="fas fa-redo"></i> a cada 15 segundos, se necessário.

    Agora, você pode verificar a alteração.

32. Retorne ao **console do Amazon EC2** e, no painel de navegação à esquerda, escolha **Security Groups** (Grupos de segurança).

33. Na lista **Security Groups** (Grupos de segurança), selecione **Lab-Application-WebServerSecurityGroup**.

    A guia **Regras de entrada** deve indicar uma regra adicional que permita o tráfego _SSH_ sobre a _porta TCP 22_.

    Esta subtarefa demonstra como as alterações podem ser implantadas em um processo repetível e documentado. Os modelos do AWS CloudFormation podem ser armazenados em um repositório de código-fonte (como o AWS CodeCommit). Dessa forma, você pode manter versões e um histórico dos modelos e da infraestrutura que foi implantada.


<br/>
## Tarefa 4: explorar modelos com o AWS CloudFormation Designer

*O AWS CloudFormation Designer* é uma ferramenta gráfica para criar, visualizar e modificar modelos do AWS CloudFormation. Com o Designer, você pode diagramar seus recursos de modelo usando uma interface de arrastar e soltar e, em seguida, editar seus detalhes por meio do editor JSON e YAML integrado.

Independentemente de ser um usuário novo ou experiente do AWS CloudFormation, o AWS CloudFormation Designer pode ajudar a ver rapidamente a inter-relação entre os recursos de um modelo e modificar facilmente os modelos.

Nessa tarefa, você ganhará experiência prática com o Designer.

34. No menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Serviços), escolha **CloudFormation**.

35. No painel de navegação à esquerda, escolha **Designer**.

    **Dica:** talvez seja necessário expandir o painel de navegação à esquerda escolhendo o ícone de menu.

[//]: # "I added the previous tip because I couldn't originally find the navigation pane in the console, until I realized that I needed to open it through the menu. I hope this addition is ok. I think it's likely that there's a standard Font Awesome icon for the menu, but unfortunately, I'm not familiar with it :( "

36. Escolha o menu **File** (Arquivo) <i class="fas fa-file"></i>, selecione **Open > Local file** (Abrir > Arquivo local) e selecione o modelo **lab-application2.yaml** que você obteve por download anteriormente.

O Designer exibirá uma representação gráfica do modelo:

![CloudFormation Designer](images/designer.png)

Em vez de desenhar um diagrama de arquitetura comum, o Designer é um editor visual de modelos do AWS CloudFormation, portanto, o Designer desenha os recursos definidos em um modelo e os relacionamentos entre eles.

37. Experimente as características do Designer. Algumas coisas para tentar são:

    - Clique nos recursos exibidos. O painel inferior exibirá a parte do modelo que define os recursos.

    - Tente arrastar um novo recurso (do painel **Resource types** (Tipos de recursos) à esquerda) para a área de design. A definição do recurso será inserida automaticamente no modelo.

    - Tente arrastar os círculos do conector de recursos para criar relacionamentos entre os recursos

    - Abra o modelo **lab-network.yaml** que você baixou anteriormente no laboratório e também explore seus recursos no Designer.


<br/>
## Tarefa 5: excluir a pilha

Quando os recursos não são mais necessários, o AWS CloudFormation pode excluir os recursos criados para a pilha.

Uma _política de exclusão_ também pode ser especificada em relação aos recursos. Ele pode preservar ou (em alguns casos) fazer backup de um recurso quando sua pilha é excluída. Esse recurso é útil para reter bancos de dados, volumes de disco ou qualquer recurso que possa ser necessário após a exclusão da pilha.

A pilha _de aplicação de laboratório_ foi configurada para tirar um snapshot de um volume de disco do Amazon Elastic Block Store (Amazon EBS) antes de ser excluído. O código no modelo que realiza essa configuração é:

```yaml
  DiskVolume:
    Type: AWS::EC2::Volume
    Properties:
      Size: 100
      AvailabilityZone: !GetAtt WebServerInstance.AvailabilityZone
      Tags:
        - Key: Name
          Value: Web Data
    DeletionPolicy: Snapshot
```

_DeletionPolicy_ na linha final está instruindo o AWS CloudFormation a criar um snapshot do volume de disco antes de ser excluído.

Agora, exclua a pilha _lab-application_ e veja os resultados da política de exclusão.

38. Retorne ao **console principal do AWS CloudFormation** escolhendo o</span> link <span style="color:#1166bb">Fechar na parte superior da página Designer (escolha **Deixar página**, se solicitado).

39. Na lista de pilhas, escolha o link **lab-application**.

40. Escolha <span id="ssb_ssm_white">Delete</span> (Excluir)

41. Escolha <span id="ssb_orange">Delete stack</span> (Excluir pilha)

Você pode monitorar o processo de exclusão na guia **Eventos** e atualizar a tela escolhendo **Atualizar** <i class="fas fa-redo"></i> ocasionalmente. Você também pode ver uma entrada de log de eventos que indique que o snapshot do EBS está sendo criado.

42. Aguarde até que a pilha seja excluída. Ele desaparecerá da lista de pilhas.

A pilha de aplicações __ removida, mas a pilha de rede permaneceu intocada. Esse cenário reforça a ideia de que diferentes equipes (por exemplo, a equipe de rede ou a equipe de aplicações) poderiam gerenciar suas próprias pilhas.

Agora você verificará se um snapshot do volume do EBS foi criado antes de o volume do EBS ser excluído.

43. No menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Serviços), escolha **EC2**.

44. No painel de navegação à esquerda, escolha **Snapshots**.

Você verá um snapshot com um horário **Started** (Iniciado às) nos últimos minutos.



<br/>

## Enviar o trabalho

45. Na parte superior destas instruções, escolha <span id="ssb_blue">Submit</span> (Enviar) para gravar o progresso e, quando solicitado, escolha **Yes** (Sim).

46. Se os resultados não forem exibidos após alguns minutos, volte ao topo destas instruções e escolha <span id="ssb_voc_grey">Grades</span> (Notas).

    **Dica**: você pode enviar seu trabalho várias vezes. Depois de alterar o trabalho, escolha **Submit** (Enviar) novamente. Seu último envio é o que será gravado para este laboratório.

47. Para ver o feedback detalhado do seu trabalho, escolha <span id="ssb_voc_grey">Details</span> (Detalhes) e depois <i class="fas fa-caret-right"></i> **View Submission Report** (Visualizar relatório de envio).

<br/>

## Laboratório concluído <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Parabéns! Você concluiu o laboratório.

48. Escolha <span id="ssb_voc_grey">End Lab</span> (Encerrar laboratório) na parte superior desta página e, em seguida, selecione <span id="ssb_blue">Yes</span> (Sim) para confirmar que você deseja encerrar o laboratório.

    Um painel indica que *DELETE has been initiated... You may close this message box now (A EXCLUSÃO foi iniciada... Você pode fechar esta caixa de mensagem agora)*.

49. Escolha o **X** no canto superior direito para fechar o painel.



*©2020, Amazon Web Services, Inc. e suas afiliadas. Todos os direitos reservados. Este trabalho não pode ser reproduzido ou redistribuído, no todo ou em parte, sem permissão prévia por escrito da Amazon Web Services, Inc. É proibido copiar, emprestar ou vender para fins comerciais.*

