# Módulo 6 – Laboratório guiado: Criar uma Virtual Private Cloud

[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: ILT-TF-100-ARCHIT-6 branch dev_65"

## Visão geral e objetivos do laboratório

A rede tradicional é difícil. Envolve equipamentos, cabeamento, configurações complexas e habilidades especializadas. O Amazon Virtual Private Cloud (Amazon VPC) oculta a complexidade e simplifica a implantação de redes privadas seguras.

Este laboratório mostra como criar sua própria Virtual Private Cloud (VPC), implantar recursos e criar conexões de emparelhamento privado entre VPCs.

Depois de concluir este laboratório, você deverá ser capaz de:

- Implantar uma VPC
- Criar e associar um gateway da Internet à VPC
- Criar uma sub-rede pública
- Criar sub-rede privada
- Criar um servidor de aplicações para testar a VPC



No **final** deste laboratório, sua arquitetura será semelhante ao seguinte exemplo:

![Arquitetura](images/module-6-guided-lab-architecture.png)


<br/>
##Duração

Este laboratório levará aproximadamente **30 minutos** para ser concluído.

<br/>

## Restrições de serviço da AWS

Neste ambiente de laboratório, o acesso aos serviços e ações de serviço da AWS pode estar restrito àqueles necessários para concluir as instruções do laboratório. Você poderá encontrar erros se tentar acessar outros serviços ou executar ações além do que está descrito neste laboratório.

<br/>

## Acessar o Console de Gerenciamento da AWS

1. Na parte superior destas instruções, escolha <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratório) para iniciar o laboratório.

   Um painel **Start Lab** (Iniciar laboratório) é aberto com o status do laboratório.

   <i class="fas fa-info-circle"></i> **Dica**: se você precisar de mais tempo para concluir o laboratório, escolha novamente o botão <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratório) para reiniciar o cronômetro do ambiente.

2. Aguarde até que o painel **Start Lab** (Iniciar laboratório) exiba a mensagem *Lab status: ready (Status do laboratório: pronto)* e, em seguida, escolha **X** para fechar o painel.

3. Na parte superior destas instruções, escolha <span id="ssb_voc_grey">AWS</span>.

   Essa ação abrirá o Console de Gerenciamento da AWS em uma nova guia do navegador. O sistema fará seu login automaticamente.

   <i class="fas fa-exclamation-triangle"></i> **Dica**: se uma nova guia do navegador não for aberta, normalmente você verá uma faixa ou um ícone na parte superior do navegador com uma mensagem informando que o navegador está impedindo que o site abra janelas pop-up. Escolha a faixa ou o ícone e depois **Allow pop ups** (Permitir pop-ups).

4. Organize a guia do **Console de Gerenciamento da AWS** para que ela seja exibida com essas instruções. O ideal é ver as duas guias do navegador abertas ao mesmo tempo, para facilitar o acompanhamento das etapas do laboratório.

   <i class="fas fa-exclamation-triangle"></i> **Não altere a região, a menos que seja especificamente instruído a fazer isso**.

<br/>

## Tarefa 1: criar uma VPC

Você começará usando a Amazon VPC para criar uma nova **Virtual Private Cloud** (VPC – Nuvem privada virtual).

Uma VPC é uma rede virtual dedicada à sua conta da Amazon Web Services (AWS). Ela fica isolada logicamente de outras redes virtuais na Nuvem AWS. É possível executar recursos da AWS, como instâncias do Amazon Elastic Compute Cloud (Amazon EC2), na VPC. Você pode configurar a VPC modificando sua faixa de endereços IP e criar sub-redes. Também é possível configurar tabelas de rotas, gateways de rede e configurações de segurança.

5. No **Console de Gerenciamento da AWS**, no menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Serviços), escolha **VPC**.

   O console da VPC oferece um assistente de VPC que pode criar várias arquiteturas de VPC automaticamente. No entanto, neste laboratório, você criará os componentes da VPC manualmente.

6. No painel de navegação à esquerda, escolha **Your VPCs** (Suas VPCs).

   Uma VPC padrão é fornecida para que você possa executar recursos assim que começar a usar a AWS. Há também uma **VPC compartilhada** que você usará mais tarde no laboratório. No entanto, agora você criará sua própria _Lab VPC_.

   A VPC terá um intervalo Classless Inter-Domain Routing (CIDR – Roteamento entre domínios sem classe) de **10.0.0.0/16**, que inclui todos os endereços IP que começam com **10.0.x.x** contendo mais de 65.000 endereços. Posteriormente, você dividirá os endereços em _sub-redes_separadas.

7. Escolha <span id="ssb_blue">Create VPC</span> (Criar VPC) e defina estas configurações:

   - **Name tag** (Tag de nome): `Lab VPC`
   - **IPv4 CIDR block** (Bloco CIDR IPv4): `10.0.0.0/16`
   - Escolha <span id="ssb_blue">Create</span> (Criar) e <span id="ssb_blue">Close</span> (Fechar).

   <i class="fas fa-comment"></i> Se essas opções não aparecerem, cancele sua configuração. No painel de navegação à esquerda, certifique-se de que escolheu **Your VPCs** (Suas VPCs). Em seguida, escolha **Create VPC** (Criar VPC) novamente.

8. Selecione <i class="far fa-check-square"></i>**Lab VPC** e certifique-se de que é a única VPC selecionada.

9. Na metade inferior da página, escolha a guia **Tags**.

   Tags são úteis para identificar recursos. Por exemplo, você pode usar uma tag para identificar centros de custo ou ambientes diferentes (como desenvolvimento, teste ou produção).

10. Escolha <span id="ssb_grey">Actions<i class="fas fa-angle-down"></i></span> (Ações) e selecione **Edit DNS hostnames** (Editar nomes de host DNS).

   Essa opção atribui um nome de sistema de nomes de domínio (DNS) _amigável_ às instâncias do EC2 na VPC, como:

    _ec2-52-42-133-255.us-west-2.compute.amazonaws.com_

11. Selecione <i class="far fa-check-square"></i>**enable** (habilitar), escolha <span id="ssb_blue">Save</span> (Salvar) e, em seguida, escolha <span id="ssb_blue">Close</span> (Fechar)

   Todas as instâncias do EC2 executadas na VPC agora receberão automaticamente um nome de host DNS. Você também pode adicionar um nome de DNS mais significativo (como _app.exemplo.com_) posteriormente usando o Amazon Route 53.



## Tarefa 2: criar sub-redes

Uma sub-rede é um subintervalo de endereços IP dentro da VPC. Os recursos da AWS podem ser executados em uma sub-rede especificada. Use uma _sub-rede pública_ para recursos que devem ser conectados à Internet e use uma _sub-rede privada_ para recursos que devem permanecer isolados da Internet.

Nesta tarefa, você criará uma sub-rede pública e uma sub-rede privada:

![Sub-redes](images/module-6-guided-lab-task-2.png)

<br/>
### Criar uma sub-rede pública

A sub-rede pública será usada para recursos voltados para a Internet.

12. No painel de navegação à esquerda, escolha **Subnets** (Sub-redes).

13. Escolha <span id="ssb_blue">Create subnet</span> (Criar sub-rede) e configure estes ajustes:

    - **Name tag** (Tag de nome): `Public Subnet (Sub-rede pública)`
    - **VPC:** _Lab VPC_
    - **Availability Zone** (Zona de disponibilidade): selecione a _primeira_ zona de disponibilidade na lista (_não_ escolha **No Preference** (Sem preferência)).
    - **IPv4 CIDR block** (Bloco CIDR IPv4): `10.0.0.0/24`
    - Escolha <span id="ssb_blue">Create</span> (Criar) e <span id="ssb_blue">Close</span> (Fechar).

    <i class="fas fa-comment"></i> A VPC tem um bloco CIDR de *10.0.0.0/16*, que inclui todos os endereços IP *10.0.x.x.* A sub-rede que você acabou de criar tem um bloco CIDR de *10.0.0.0/24*, que inclui todos os endereços IP *10.0.0.x.* Eles podem ser semelhantes, mas a sub-rede é menor que a VPC devido ao _/24_ no intervalo CIDR.

    Agora, você vai configurar a sub-rede para atribuir um endereço IP público automaticamente a todas as instâncias executadas nela.

14. Selecione <i class="far fa-check-square"></i>**Public Subnet** (Sub-rede pública).

15. Escolha <span id="ssb_grey">Actions<i class="fas fa-angle-down"></i></span> (Ações) e selecione **Modify auto-assign IP settings** (Modificar configurações de IP de atribuição automática), e:

    - Selecione <i class="far fa-check-square"></i>**Auto-assign IPv4** (Atribuir IPv4 automaticamente)
    - Escolha <span id="ssb_blue">Save</span> (Salvar)

    <i class="fas fa-comment"></i> Embora essa sub-rede tenha sido denominada _Public Subnet (Sub-rede pública)_, ela ainda não é pública. Uma sub-rede pública deve ter um gateway da Internet, que você anexará na próxima tarefa.

<br/>

### Criar uma sub-rede privada

A sub-rede privada será usada para recursos que devem permanecer isolados da Internet.

16. Use o que você acabou de aprender para criar outra sub-rede com estas configurações:

    - **Name tag** (Tag de nome): `Private Subnet` (Sub-rede privada)
    - **VPC:** _Lab VPC_
    - **Availability Zone** (Zona de disponibilidade): selecione a _primeira_ zona de disponibilidade na lista (_não_ escolha **No Preference** (Sem preferência)).
    - **IPv4 CIDR block** (Bloco CIDR IPv4): `10.0.2.0/23`

    O bloco CIDR de *10.0.2.0/23* inclui todos os endereços IP que começam com *10.0.2.x* e *10.0.3.x*. Isso é duas vezes maior que a sub-rede pública, pois a maioria dos recursos deve ser mantida em sub-redes privadas, a menos que devam estar especificamente acessíveis pela Internet.

    Sua VPC agora tem duas sub-redes. No entanto, a sub-rede pública é totalmente isolada e não pode se comunicar com recursos fora da VPC. Em seguida, você configurará a sub-rede pública para se conectar à Internet por meio de um gateway da Internet.

<br/>

## Tarefa 3: criar um gateway da Internet

Um *gateway da Internet* é um componente da VPC dimensionado horizontalmente, redundante e altamente disponível que permite a comunicação entre instâncias em uma VPC e na Internet. Ele não impõe riscos de disponibilidade ou restrições de largura de banda no tráfego da rede.

Um gateway da Internet tem duas finalidades:

- Fornecer um destino para que as tabelas de rotas se conectem à Internet
- Executar a conversão de endereços de rede (NAT) para instâncias que receberam endereços IPv4 públicos

Nesta tarefa, você criará um gateway da Internet para que o tráfego da Internet possa acessar a sub-rede pública.

17. No painel de navegação à esquerda, escolha **Internet Gateways** (Gateways da Internet).

18. Escolha <span id="ssb_blue">Create Internet Gateway</span> (Criar gateway da Internet) e configure estes ajustes:

    - **Name tag** (Tag de nome): `Lab IGW (IGW do laboratório)`
    - Escolha <span id="ssb_blue">Create</span> (Criar) e <span id="ssb_blue">Close</span> (Fechar).

    Agora, você pode anexar o gateway da Internet à _Lab VPC_.

19. Selecione <i class="far fa-check-square"></i> **Lab IGW** (IGW do laboratório) e certifique-se de que é o único gateway que você selecionou.

20. Escolha <span id="ssb_grey">Actions<i class="fas fa-angle-down"></i></span> (Ações) e **Attach to VPC** (Anexar à VPC) e defina estas configurações:

    - **VPC:** na lista, selecione _Lab VPC_
    - Escolha <span id="ssb_blue">Attach</span> (Associar)

    Essa ação anexará o gateway da Internet à sua _Lab VPC_. Embora tenha criado um gateway da Internet e o tenha anexado à sua VPC, você também deve configurar a _tabela de rotas_ de sub-rede pública para que ela use o gateway da Internet.

<br/>
## Tarefa 4: configurar tabelas de rotas

Uma *tabela de rotas* contém um conjunto de regras, chamado *rotas*, que são usadas para determinar para onde o tráfego de rede é direcionado. Cada sub-rede em uma VPC deve estar associada a uma tabela de rotas porque a tabela controla o roteamento da sub-rede. Uma sub-rede só pode ser associada a uma tabela de rotas por vez, mas você pode associar várias sub-redes à mesma tabela de rotas.

Para usar um gateway da Internet, a tabela de rotas de uma sub-rede deve conter uma rota que direcione o tráfego vinculado à Internet para o gateway da Internet. Se uma sub-rede estiver associada a uma tabela de rotas que tenha uma rota para um gateway da Internet, ela será conhecida como _sub-rede pública_.

Nesta tarefa, você vai:

* Criar uma _tabela de rotas públicas_ para tráfego vinculado à Internet
* Adicionar uma _rota_ à tabela de rotas para direcionar o tráfego de entrada da Internet para o gateway da Internet.
* Associar a sub-rede pública à nova tabela de rotas

21. No painel de navegação à esquerda, escolha **Route Tables** (Tabelas de rotas).

    Várias tabelas de rotas serão exibidas, mas há apenas uma tabela de rotas associada à *Lab VPC*. Essa tabela roteia o tráfego localmente, portanto, é chamada de _tabela de rotas privadas_.

22. Na coluna **VPC ID** (ID da VPC), selecione <i class="far fa-check-square"></i> a tabela de rotas que mostra **Lab VPC** (você pode expandir a coluna para ver os nomes).

23. Na coluna **Name** (Nome), escolha <i class="fas fa-pencil-alt"></i> então digite o nome `Private Route Table` e escolha <i class="fas fa-check-circle"></i>.

24. Na metade inferior da página, escolha a guia **Routes** (Rotas).

Há apenas uma rota. Ela mostra que todo o tráfego destinado a _10.0.0.0/16_ (que é o intervalo da _Lab VPC_) será roteado _localmente_. Isso permite que todas as sub-redes em uma VPC se comuniquem entre si.

Agora, você criará uma nova tabela de rotas públicas para enviar tráfego público para o gateway da Internet.

25. Escolha <span id="ssb_blue">Create route table</span> (Criar tabela de rotas) e configure estes ajustes:

    - **Name tag** (Tag de nome): `Public Route Table (Tabela de rotas públicas)`
    - **VPC:** _Lab VPC_
    - Escolha <span id="ssb_blue">Create</span> (Criar) e <span id="ssb_blue">Close</span> (Fechar).

26. Selecione <i class="far fa-check-square"></i> **Public Route Table** (Tabela de rotas públicas), e certifique-se de que é a única tabela de rotas que você selecionou.

27. Na guia **Routes** (Rotas), escolha <span id="ssb_grey">Edit routes</span> (Editar rotas).

    Agora você vai adicionar uma rota para direcionar o tráfego destinado à Internet (_0.0.0.0/0_) ao gateway da Internet.

28. Escolha <span id="ssb_grey">Add route</span> (Adicionar rota), em seguida configure estes ajustes:

    * **Destination** (Destino): `0.0.0.0/0`
    * **Target** (Destino): selecione _Internet Gateway (Gateway da Internet)_, depois selecione _Lab IGW (IGW do laboratório)_ da lista
    * Escolha <span id="ssb_blue">Save routes</span> (Salvar rotas) e <span id="ssb_blue">Close</span> (Fechar).

    A etapa final é _associar_ essa nova tabela de rotas à sub-rede pública.

29. Escolha a guia **Subnet Associations** (Associações de sub-rede).

30. Escolha <span id="ssb_grey">Edit subnet associations</span> (Editar associações de sub-rede).

31. Selecione a <i class="far fa-check-square"></i> linha com **Public Subnet** (Sub-rede pública).

32. Escolha <span id="ssb_blue">Save</span> (Salvar)

    A sub-rede pública agora é _pública_ porque tem uma entrada de tabela de rotas que envia tráfego para a Internet por meio do gateway da Internet.

    Para resumir, você pode criar uma sub-rede pública seguindo estas etapas:

    - Criar um _gateway da Internet_

    - Crie uma _tabela de rota_

    - Adicionar uma _rota_ à tabela de rotas que direciona o tráfego _0.0.0.0/0_ ao gateway da Internet

    - Associar a tabela de rotas a uma _sub-rede_, que se torna assim uma _sub-rede pública_

<br/>

## Tarefa 5: criar um grupo de segurança para o servidor de aplicações

Um *grupo de segurança* atua como um firewall virtual para que as instâncias controlem o tráfego de entrada e saída. Os grupos de segurança operam no nível da _interface de rede elástica para a instância_. Os grupos de segurança não operam no nível da _sub-rede_. Assim, cada instância pode ter seu próprio firewall controlando o tráfego. Se você não especificar um grupo de segurança específico no momento da inicialização, a instância será atribuída automaticamente ao _grupo de segurança padrão_ da VPC.

Nesta tarefa, você criará um grupo de segurança que permite que os usuários acessem o servidor de aplicações via HTTP.

33. No painel de navegação à esquerda, escolha **Security Groups (Grupos de segurança)**.

34. Escolha <span id="ssb_blue">Create security group</span> (Criar grupo de segurança) e configure estes ajustes:

    - **Security group name** (Nome do grupo de segurança): `App-SG`
    - **Description** (Descrição): `Allow HTTP traffic (Permitir tráfego HTTP)`
    - **VPC:** _Lab VPC_
    - Escolha <span id="ssb_blue">Create</span> (Criar) e <span id="ssb_blue">Close</span> (Fechar).

35. Selecione <i class="far fa-check-square" aria-hidden="true"></i>**App-SG**.

36. Escolha a guia **Inbound Rules** (Regras de entrada).

    As configurações de **Inbound Rules** (Regras de entrada) determinam qual tráfego é permitido chegar à instância. Você o configurará para permitir o tráfego HTTP (porta 80) que vem de qualquer lugar na Internet (_0.0.0.0/0_).

37. Selecione <span id="ssb_grey">Edit rules</span> (Editar regras).

38. Escolha <span id="ssb_grey">Add Rule</span> (Adicionar regra) e configure estes ajustes:

    - **Type** (Tipo): _HTTP_
    - **Source** (Origem): _Anywhere (Qualquer lugar)_
    - **Description** (Descrição): `Allow Web access (Permitir acesso à Web)`
    - Escolha <span id="ssb_blue">Save rules</span> (Salvar regras) e <span id="ssb_blue">Close</span> (Fechar).

    Você usa este _App-SG_ na próxima tarefa.

<br/>
## Tarefa 6: iniciar um servidor de aplicações na sub-rede pública

Para testar se sua VPC está configurada corretamente, você executará uma instância do EC2 na sub-rede pública. Você também confirmará que pode acessar a instância do EC2 a partir da Internet.

39. No menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Serviços), escolha **EC2**.

40. Escolha <span id="ssb_orange">Launch Instance</span> (Executar instância) e selecione **Launch Instance** (Executar instância) na lista suspensa. Configure estas opções:

    - Etapa 1 (escolher AMI):

     	- **AMI:** _Amazon Linux 2_

    - Etapa 2 (escolher tipo de instância):

       - **Instance Type** (Tipo de instância): _t2.micro_

    - Etapa 3 (configurar detalhes da instância):
       - **Network** (Rede): _Lab VPC_
       - **Subnet** (Sub-rede): _Public Subnet (Sub-rede pública)_
       - **IAM role** (Função do IAM): _Inventory-App-Role_
       - **User Data** (Dados do usuário) (em <i class="fas fa-caret-right"></i> **Advanced Details** (Detalhes avançados)):

       ```bash
       #!/bin/bash
       # Install Apache Web Server and PHP
       yum install -y httpd mysql
       amazon-linux-extras install -y php7.2
       # Download Lab files
       wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/ILT-TF-200-ACACAD-20-EN/mod6-guided/scripts/inventory-app.zip
       unzip inventory-app.zip -d /var/www/html/
       # Download and install the AWS SDK for PHP
       wget https://github.com/aws/aws-sdk-php/releases/download/3.62.3/aws.zip
       unzip aws -d /var/www/html
       # Turn on web server
       chkconfig httpd on
       service httpd start
       ```

    - Etapa 4 (adicionar armazenamento):

       - Usar configurações padrão (sem alterações)

    - Etapa 5 (adicionar tags):
       - Escolha <span id="ssb_grey">Add Tag</span> (Adicionar tag)
       - **Key** (Chave): `Name (Nome)`
       - **Value** (Valor): `App Server (Servidor de aplicativos)`

    - Etapa 6 (configurar grupo de segurança):

       - **Select an existing security group** (Selecionar um grupo de segurança existente): _App-SG_

       <i class="fas fa-comment"></i> Você receberá este aviso: _You will not be able to connect to the instance (Você não será capaz de se conectar à instância)_. Esse aviso é aceitável porque você não estará se conectando à instância. Toda a configuração é feita por meio do script de dados do usuário.

       - Escolha **Continue** (Continuar).

    - Etapa 7 (revisar):

       - <span id="ssb_blue">Executar</span>

41. Uma janela **Select an existing key pair or create a new key pair** (Selecione um par de chaves existente ou crie um novo par de chaves) será exibida:

* Selecione **Proceed without a key pair** (Prosseguir sem um par de chaves).
* Selecione <i class="far fa-check-square"></i> **I acknowledge that... (Eu reconheço que...)**.
* Escolha <span id="ssb_blue">Launch Instances</span> (Executar instâncias).

Uma página de status notifica que suas instâncias estão sendo iniciadas.

42. Escolha <span id="ssb_blue">View Instances</span> (Exibir instâncias).

43. Aguarde até que o servidor de aplicações seja totalmente iniciado. Ele deve exibir o seguinte status:

- **Instance State** (Estado da instância): <span style="color:green"><i class="fas fa-circle"></i></span> running (em execução)

<i class="fas fa-comment"></i> Você pode optar por renovar <i class="fas fa-sync" aria-hidden="true"></i> ocasionalmente para atualizar a exibição.

44. Selecione <i class="far fa-check-square"></i> **App Server** (Servidor de aplicativos).

45. Copie o endereço **IPv4 Public IP** (IP público do IPv4) da guia **Description (Descrição)**.

46. Abra uma nova guia do navegador da Web com esse endereço IP.

Se você configurou a VPC corretamente, a aplicação Inventory e esta mensagem devem ser exibidas: _Please configure settings to connect to database (Defina as configurações para se conectar ao banco de dados)_. Você ainda não definiu nenhuma configuração de banco de dados, mas a aparência da aplicação Inventory demonstra que a sub-rede pública foi configurada corretamente.

<span style="color:red"><i class="fas fa-exclamation-triangle"></i></span> Se a aplicação Inventory não aparecer, aguarde 60 segundos e atualize <i class="fas fa-sync"></i> a página para tentar novamente. Pode levar alguns minutos para que a instância do EC2 inicialize e execute o script que instala o software.


<br/>
## Enviar o trabalho

47. Na parte superior destas instruções, escolha <span id="ssb_blue">Submit</span> (Enviar) para gravar seu progresso e, quando solicitado, escolha **Yes** (Sim).

48. Se os resultados não forem exibidos após alguns minutos, volte ao topo destas instruções e escolha <span id="ssb_voc_grey">Grades</span> (Notas).

    **Dica**: você pode enviar seu trabalho várias vezes. Depois de alterar o trabalho, escolha **Submit** (Enviar) novamente. Seu último envio é o que será gravado para este laboratório.

49. Para ver o feedback detalhado do seu trabalho, escolha <span id="ssb_voc_grey">Details</span> (Detalhes) e depois <i class="fas fa-caret-right"></i> **View Submission Report** (Visualizar relatório de envio).

<br/>

## Laboratório concluído <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Parabéns! Você concluiu o laboratório.

50. Escolha <span id="ssb_voc_grey">End Lab</span> (Encerrar laboratório) na parte superior desta página e, em seguida, selecione <span id="ssb_blue">Yes</span> (Sim) para confirmar que você deseja encerrar o laboratório.

    Um painel indica que *DELETE has been initiated... You may close this message box now (A EXCLUSÃO foi iniciada... Você pode fechar esta caixa de mensagem agora)*.

51. Escolha o **X** no canto superior direito para fechar o painel.



*©2020, Amazon Web Services, Inc. e suas afiliadas. Todos os direitos reservados. Este trabalho não pode ser reproduzido ou redistribuído, no todo ou em parte, sem permissão prévia por escrito da Amazon Web Services, Inc. É proibido copiar, emprestar ou vender para fins comerciais.*