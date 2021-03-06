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

# Laborat??rio 4: Como trabalhar com o EBS

<!-- Note to translators: This is based on SPL-02. Copy the translation from there. Do not re-translate the whole document. -->

**Vers??o 5.1.3 (spl2)**

## Vis??o geral do laborat??rio

<img src="images/lab-scenario.jpeg" alt="diagrama arquitet??nico" width="400">

O foco deste laborat??rio ?? o Amazon Elastic Block Store (Amazon EBS), um importante mecanismo de armazenamento subjacente para inst??ncias Amazon EC2. Neste laborat??rio, voc?? aprender?? a criar um volume Amazon EBS, associ??-lo a uma inst??ncia, aplicar um sistema de arquivos ao volume e fazer um backup de snapshot.

## T??picos abordados

Ao final deste laborat??rio, voc?? ser?? capaz de:

- Criar um volume Amazon EBS
- Associar e montar o volume em uma inst??ncia EC2
- Criar um snapshot do volume
- Criar um novo volume com base no seu snapshot
- Associar e montar o novo volume em sua inst??ncia EC2

## Pr??-requisitos do laborat??rio

Para concluir este laborat??rio com ??xito, voc?? deve estar familiarizado com o uso b??sico do Amazon EC2 e com a administra????o b??sica do servidor Linux. Voc?? deve estar acostumado a usar as ferramentas da linha de comando do Linux.

### Outros servi??os da AWS

Os outros servi??os da AWS que n??o s??o necess??rios para este laborat??rio s??o desabilitados pela pol??tica do IAM durante seu tempo de acesso ao laborat??rio. Al??m disso, os recursos dos servi??os usados neste laborat??rio s??o limitados ao que ?? exigido pelo laborat??rio e, em alguns casos, s??o ainda mais limitados como um aspecto intencional do projeto do laborat??rio. Se voc?? acessar outros servi??os ou executar a????es diferentes das que fazem parte deste guia de laborat??rio, haver?? erros.

### O que ?? Amazon Elastic Block Store?

O **Amazon Elastic Block Store (Amazon EBS)** oferece armazenamento persistente para inst??ncias Amazon EC2. Os volumes do Amazon EBS s??o vinculados ?? rede e persistem de maneira independente ?? vida ??til de uma inst??ncia. Os volumes do Amazon EBS s??o volumes altamente dispon??veis e extremamente confi??veis que podem ser aproveitados como uma parti????o de inicializa????o das inst??ncias do Amazon EC2 ou associados a uma inst??ncia em execu????o do Amazon EC2 como um dispositivo de blocos padr??o.

Quando utilizadas como uma parti????o de inicializa????o, as inst??ncias do Amazon EC2 podem ser interrompidas e posteriormente reiniciadas, permitindo que voc?? pague somente pelos recursos de armazenamento utilizados, mantendo o estado de sua inst??ncia. Os volumes do Amazon EBS oferecem melhor durabilidade em compara????o aos armazenamentos locais de inst??ncias do Amazon EC2, porque os volumes do Amazon EBS s??o automaticamente replicados no back-end (em uma ??nica zona de disponibilidade).

Para aqueles que desejam ainda maior durabilidade, o Amazon EBS fornece a capacidade de criar snapshots de momentos espec??ficos de seus volumes, sendo armazenados no Amazon Simple Storage Service (Amazon S3) e automaticamente duplicados pelas v??rias zonas de disponibilidade. Esses snapshots podem ser usados como ponto inicial para novos volumes do Amazon EBS e podem proteger seus dados para uma durabilidade a longo prazo. Voc?? pode tamb??m, facilmente, compartilhar esses snapshots com os colegas de trabalho e com outros colaboradores da AWS.

Este guia de laborat??rio explica os conceitos b??sicos do Amazon EBS detalhadamente. No entanto, ele pode incluir apenas uma breve vis??o geral dos conceitos do Amazon EBS. Para obter mais informa????es, consulte a <a href="http://aws.amazon.com/ebs/" target="_blank">Documenta????o do Amazon EBS</a>.

### Recursos de volume Amazon EBS

Os volumes do Amazon EBS contam com os seguintes recursos:

- **Armazenamento persistente:** a vida ??til do volume ?? independente de qualquer inst??ncia espec??fica do Amazon EC2.
- **Uso geral:** os volumes do Amazon EBS s??o dispositivos de blocos brutos e n??o formatados que podem ser usados em qualquer sistema operacional.
- **Alta performance:** os volumes do Amazon EBS s??o iguais ou melhores que as unidades locais do Amazon EC2.
- **Alta confiabilidade:** os volumes do Amazon EBS t??m redund??ncia integrada dentro de uma zona de disponibilidade.
- **Projetado para resili??ncia:** a AFR (taxa anual de falhas) do Amazon EBS est?? entre 0,1% e 1%.
- **Tamanho vari??vel:** os tamanhos de volume variam de 1??GB a 16??TB.
- **F??cil de usar:** os volumes do Amazon EBS podem ser facilmente criados, associados, inclu??dos em backup, restaurados e exclu??dos.

**Dura????o**
Este laborat??rio levar?? aproximadamente **30 minutos** para ser conclu??do.

## Como acessar o Console de Gerenciamento da AWS

1. Na parte superior destas instru????es, clique em <span id="ssb_voc_grey">Start Lab</span> (Iniciar laborat??rio) para iniciar o laborat??rio.

   Um painel Start Lab (Iniciar laborat??rio) ?? aberto exibindo o status do laborat??rio.

2. Aguarde at?? ver a mensagem "**Lab status: ready**" (Status do laborat??rio: pronto) e clique no **X** para fechar o painel Start Lab (Iniciar laborat??rio).

3. Na parte superior destas instru????es, clique em <span id="ssb_voc_grey">AWS</span>.

   Isso abrir?? o Console de Gerenciamento da AWS em uma nova guia do navegador. O sistema far?? o seu login automaticamente.

   **Dica**: se uma nova guia do navegador n??o abrir, normalmente haver?? um banner ou um ??cone na parte superior do navegador indicando que o navegador est?? impedindo que o site abra janelas pop-up. Clique no banner ou ??cone e escolha "Allow pop ups" (Permitir pop-ups).

4. Organize a guia do Console de Gerenciamento da AWS para que ela seja exibida com estas instru????es. Em um cen??rio ideal, voc?? poder?? ver as duas guias do navegador ao mesmo tempo, para facilitar o acompanhamento das etapas do laborat??rio.

&nbsp;
&nbsp;
## Tarefa 1: Criar um novo volume EBS

Nesta tarefa, voc?? criar?? e associar?? um volume Amazon EBS a uma nova inst??ncia Amazon EC2.

5. No **Console de Gerenciamento da AWS**, no menu **Services** (Servi??os), clique em **EC2**.

6. No painel de navega????o esquerdo, clique em **Instances** (Inst??ncias).

   Uma inst??ncia Amazon EC2 chamada **Lab** j?? foi executada para seu laborat??rio.

7. Observe a **zona de disponibilidade** da inst??ncia. Ela ser?? semelhante a *us-west-2a*.

8. No painel de navega????o esquerdo, clique em **Volumes**.

   Voc?? ver?? um volume existente que a inst??ncia Amazon EC2 est?? usando. Esse volume tem um tamanho de 8??GiB, o que facilita a distin????o do volume que voc?? criar?? em seguida, que ser?? de 1??GiB.

9. Clique em <span id="ssb_blue">Create Volume</span> (Criar volume) e configure:

   * **Volume Type:** (Tipo de volume) *General Purpose SSD (gp2)* (SSD de uso geral (gp2))
   * **Size (GiB):** (Tamanho) `1`. **OBSERVA????O**: voc?? pode estar impedido de criar volumes grandes.
   * **Availability Zone:** (Zona de disponibilidade) selecione a mesma zona de disponibilidade da sua inst??ncia EC2.
   * Clique em <span id="ssb_grey">Add Tag</span> (Adicionar tag).
   * No Editor de tags, insira:
      * **Key** (Chave): `Name` (Nome)
      * **Value:** (Valor) `My Volume`

10. Clique em <span id="ssb_blue">Create Volume</span> (Criar volume) e em <span id="ssb_blue">Close</span> (Fechar)

   Seu novo volume aparecer?? na lista, e o estado dele mudar?? de *creating* (em cria????o) para *available* (dispon??vel). Talvez voc?? tenha de clicar em **refresh** <span class="fas fa-sync"></span> (atualizar) para ver o novo volume.

&nbsp;
&nbsp;
## Tarefa 2: Associar o volume a uma inst??ncia

Agora voc?? pode associar seu novo volume ?? inst??ncia Amazon EC2.

11. Selecione <i class="fas fa-square" style="color:blue"></i> **My Volume**.

12. No menu **Actions** (A????es), clique em **Attach Volume** (Associar volume).

13. Clique no campo **Instance** (Inst??ncia) e, em seguida, selecione a inst??ncia exibida (Lab).

    Observe que o campo **Device** (Dispositivo) est?? definido como */dev/sdf*. Voc?? usar?? esse identificador de dispositivo em uma tarefa posterior.

14. Click <span id="ssb_blue">Attach</span> (Associar).
    O estado do volume agora ?? *in-use* (em uso).

&nbsp;
&nbsp;
## Tarefa 3: Conectar-se ?? sua inst??ncia Amazon EC2

### <i class="fab fa-windows"></i> Usu??rios do Windows: como usar o SSH para se conectar

<i class="fas fa-comment"></i> Estas instru????es s??o apenas para usu??rios do Windows.

Se voc?? estiver usando macOS ou Linux, <a href="#ssh-MACLinux">v?? para a pr??xima se????o</a>.

15. Leia os tr??s marcadores nesta etapa antes de come??ar a executar as a????es, porque voc?? n??o poder?? visualizar estas instru????es quando o painel Details (Detalhes) estiver aberto.

    * Clique no menu suspenso <span id="ssb_voc_grey">Details</span> (Detalhes) acima das instru????es que voc?? est?? lendo no momento e clique em <span id="ssb_voc_grey">Show</span> (Mostrar). Uma janela Credentials (Credenciais) ser?? aberta.

    * Clique no bot??o **Download PPK** (Fazer download do PPK) e salve o arquivo **labsuser.ppk**. Normalmente, seu navegador o salva no diret??rio Downloads.

    * Em seguida, clique no **X** para sair do painel Details (Detalhes).

16. Fa??a download do software necess??rio.

    * Voc?? usar?? o **PuTTY** para se conectar ??s inst??ncias Amazon EC2 com SSH. Se voc?? n??o tiver o PuTTY instalado no seu computador, <a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">fa??a download dele aqui</a>.

17. Abra o **putty.exe**

18. Configure o PuTTY para n??o atingir o tempo limite:

    * Clique em **Connection** (Conex??o)
    * Defina **Seconds between keepalives** (Segundos entre os keepalives) como `30`

    Isso permite que manter a sess??o do PuTTY aberta por mais tempo.

19. Configure sua sess??o do PuTTY:

    * Clique em **Session** (Sess??o)

    * **Host Name (or IP address)** (Nome do host ou endere??o IP): copie e cole o **IPv4 Public IP address** (Endere??o IP p??blico IPv4) da inst??ncia. Para encontr??-lo, retorne ao console do EC2 e clique em **Instances** (Inst??ncias). Marque a caixa ao lado da inst??ncia e, na guia *Description* (Descri????o), copie o valor **IPv4 Public IP** (IP p??blico IPv4).

    * De volta ao PuTTy, na lista **Connection** (Conex??o), expanda <i class="far fa-plus-square"></i> **SSH**

    * Clique em **Auth** (Autentica????o) (n??o expanda)

    * Clique em **Browse** (Procurar)

    * Procure e selecione o arquivo labsuser.ppk do qual voc?? fez download

    * Clique em **Open** (Abrir) para selecion??-lo

    * Clique em **Open** (Abrir)

20. Clique em **Yes** (Sim) para confiar no host e conectar-se a ele.

21. Quando for solicitado **login as** (fazer login como), insira `ec2-user`

    Isso conectar?? voc?? ?? inst??ncia EC2.

22. <a href="#ssh-after">Usu??rios do Windows: clique aqui para avan??ar para a pr??xima tarefa.</a>

<a id='ssh-MACLinux'></a>
### Usu??rios do macOS <i class="fab fa-apple"></i> e Linux <i class="fab fa-linux"></i>

Estas instru????es s??o apenas para usu??rios do Mac/Linux. Se voc?? for um usu??rio do Windows, <a href="#ssh-after">avance para a pr??xima tarefa.</a>

23. Leia todas as instru????es nesta ??nica etapa antes de come??ar a executar as a????es, porque voc?? n??o poder?? v??-las quando o painel Details (Detalhes) estiver aberto.

    * Clique no menu suspenso <span id="ssb_voc_grey">Details</span> (Detalhes) acima das instru????es que voc?? est?? lendo no momento e clique em <span id="ssb_voc_grey">Show</span> (Mostrar). Uma janela Credentials (Credenciais) ser?? aberta.

    * Clique no bot??o **Download** (Fazer download) e salve o arquivo **labsuser.pem**.

    * Em seguida, clique no **X** para sair do painel Details (Detalhes).

24. Abra uma janela do terminal e altere o diret??rio `cd` para o diret??rio no qual foi feito download do arquivo labsuser.pem.

    Por exemplo, execute este comando, se ele tiver sido salvo no diret??rio Downloads:

    ```plain
    cd ~/Downloads
    ```

25. Execute o comando abaixo para alterar as permiss??es na chave para somente leitura:

    ```plain
    chmod 400 labsuser.pem
    ```

26. Retorne ao Console de Gerenciamento da AWS e, no servi??o do EC2, clique em **Instances** (Inst??ncias).

    A inst??ncia **Lab** deve estar selecionada.

27. Na guia *Description* (Descri????o), copie o valor **IPv4 Public IP** (IP p??blico IPv4).

28. Retorne ?? janela do terminal e execute este comando (substitua **<public-ip\>** pelo endere??o IP p??blico real que voc?? copiou):

    ```plain
    ssh -i labsuser.pem ec2-user@<public-ip>
    ```

29. Digite `yes` quando solicitado para permitir a primeira conex??o com este servidor SSH remoto.

    Como voc?? est?? usando um par de chaves para autentica????o, n??o ser?? necess??rio fornecer uma senha.

    <a id='ssh-after'></a>


&nbsp;
&nbsp;
## Tarefa 4: Criar e configurar seu sistema de arquivos

Nesta tarefa, voc?? adicionar?? o novo volume a uma inst??ncia Linux como um sistema de arquivos ext3 no ponto de montagem /mnt/data-store.

<i class="fas fa-info-circle"></i> Se voc?? usa o PuTTY, pode clicar com o bot??o direito do mouse na janela do PuTTY para colar texto.

30. Visualize o armazenamento dispon??vel em sua inst??ncia:

    ```plain
    df -h
    ```

    Voc?? deve ver uma sa??da semelhante a esta:

    ```plain
    Filesystem      Size  Used Avail Use% Mounted on
    devtmpfs        488M   60K  488M   1% /dev
    tmpfs           497M     0  497M   0% /dev/shm
    /dev/xvda1      7.8G  982M  6.7G  13% /
    ```

    Ela mostra o volume de disco original de 8??GB. Seu novo volume ainda n??o aparece.

31. Crie um sistema de arquivos ext3 no novo volume:

    ```plain
    sudo mkfs -t ext3 /dev/sdf
    ```

32. Crie um diret??rio para montar o novo volume de armazenamento:

    ```plain
    sudo mkdir /mnt/data-store
    ```

33. Monte o novo volume:

    ```plain
    sudo mount /dev/sdf /mnt/data-store
    ```

    Para configurar a inst??ncia Linux para montar esse volume sempre que for iniciada, voc?? precisar?? adicionar uma linha a */etc/fstab*.

    ```plain
    echo "/dev/sdf   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
    ```

34. Visualize o arquivo de configura????o para verificar a configura????o na ??ltima linha:

    ```plain
    cat /etc/fstab
    ```

35. Visualize o armazenamento dispon??vel novamente:

    ```plain
    df -h
    ```

    Agora a sa??da incluir?? a linha adicional - */dev/xvdf*:

    ```plain
    Filesystem      Size  Used Avail Use% Mounted on
    devtmpfs        488M   60K  488M   1% /dev
    tmpfs           497M     0  497M   0% /dev/shm
    /dev/xvda1      7.8G  982M  6.7G  13% /
    /dev/xvdf       976M  1.3M  924M   1% /mnt/data-store
    ```

36. No volume montado, crie um arquivo e adicione um texto a ele.

    ```plain
    sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
    ```

37. Verifique se o texto foi gravado em seu volume.

    ```plain
    cat /mnt/data-store/file.txt
    ```

&nbsp;
&nbsp;
## Tarefa 5: Criar um snapshot do Amazon EBS

Nesta tarefa, voc?? criar?? um snapshot do seu volume EBS.

Voc?? pode criar qualquer n??mero de snapshots pontuais e consistentes dos volumes do Amazon EBS a qualquer momento. Os snapshots do Amazon EBS s??o armazenados no Amazon S3 com alta durabilidade. Novos volumes do Amazon EBS podem ser criados com base em snapshots para clonagem ou restaura????o de backups. Os snapshots do Amazon EBS tamb??m podem ser facilmente compartilhados entre usu??rios da AWS ou copiados para regi??es da AWS.

38. No **Console de Gerenciamento da AWS**, clique em **Volumes** e selecione <i class="fas fa-square" style="color:blue"></i> **My Volume**.

39. No menu **Actions** (A????es), clique em **Create Snapshot** (Criar snapshot).

40. Clique em <span id="ssb_grey">Add Tag</span> (Adicionar tag) e configure:

    * **Key** (Chave): `Name` (Nome)
    * **Value:** (Valor) `My Snapshot`
    * Clique em <span id="ssb_blue">Create Snapshot</span> (Criar snapshot) e em <span id="ssb_blue">Close</span> (Fechar)

    Seu snapshot ser?? listado no console de **Snapshots**.

41. No painel de navega????o esquerdo, clique em **Snapshots**.

    Seu snapshot ser?? exibido. Ele come??ar?? com um estado *pending* (pendente), o que significa que o snapshot est?? sendo criado. Em seguida, ele ser?? alterado para o estado *completed* (conclu??do). Somente os blocos de armazenamento usados s??o copiados para os snapshots, portanto, os blocos vazios n??o ocupam espa??o de armazenamento do snapshot.

42. Na sess??o SSH remota, exclua o arquivo que voc?? criou no volume.

    ```plain
    sudo rm /mnt/data-store/file.txt
    ```

43. Verifique se o arquivo foi exclu??do.

    ```plain
    ls /mnt/data-store/
    ```

    Seu arquivo foi exclu??do.

&nbsp;
&nbsp;
## Tarefa 6: Restaurar o snapshot do Amazon EBS

Se voc?? quiser recuperar os dados armazenados em um snapshot, poder?? usar o comando **Restore** (Restaurar) para restaurar o snapshot em um novo volume EBS.

### Criar um volume por meio do seu snapshot

44. No **Console de Gerenciamento da AWS**, selecione <i class="fas fa-square" style="color:blue"></i> **My Snapshot**.

45. No menu **Actions** (A????es), clique em **Create Volume** (Criar volume).

46. Em **Availability Zone** (Zona de disponibilidade), selecione a mesma zona de disponibilidade usada anteriormente.

47. Clique em <span id="ssb_grey">Add Tag</span> (Adicionar tag) e configure:

    * **Key** (Chave): `Name` (Nome)
    * **Value:** (Valor) `Restored Volume` (Volume restaurado)
    * Clique em <span id="ssb_blue">Create Volume</span> (Criar volume).
    * Clique em <span id="ssb_blue">Close</span> (Fechar)

    Ao restaurar um snapshot para um novo volume, voc?? tamb??m pode modificar a configura????o, como alterar o tipo de volume, o tamanho ou a zona de disponibilidade.

### Associar o volume restaurado ?? sua inst??ncia EC2

48. No painel de navega????o esquerdo, clique em **Volumes**.

49. Selecione <i class="fas fa-square" style="color:blue"></i> **Restored Volume** (Volume restaurado).

50. No menu **Actions** (A????es), clique em **Attach Volume** (Associar volume).

51. Clique no campo **Instance** (Inst??ncia) e, em seguida, selecione a inst??ncia exibida (Lab).

    Observe que o campo **Device** (Dispositivo) est?? definido como */dev/sdg*. Voc?? usar?? esse identificador de dispositivo em uma tarefa posterior.

52. Clique em <span id="ssb_blue">Attach</span> (Associar).

    O estado do volume agora ?? *in-use* (em uso).

### Montar o volume restaurado

53. Crie um diret??rio para montar o novo volume de armazenamento:

    ```plain
    sudo mkdir /mnt/data-store2
    ```

54. Monte o novo volume:

    ```plain
    sudo mount /dev/sdg /mnt/data-store2
    ```

55. Verifique se o volume montado tem o arquivo que voc?? criou anteriormente.

    ```plain
    ls /mnt/data-store2/
    ```

    Voc?? deve ver um arquivo .txt.

&nbsp;
&nbsp;
## Conclus??o

<i class="far fa-thumbs-up" style="color:blue"></i> Parab??ns! Agora voc??:

- Criou um volume Amazon EBS
- Associou o volume a uma inst??ncia EC2
- Criou um sistema de arquivos no volume
- Adicionou um arquivo ao volume
- Criou um snapshot do seu volume
- Criou um novo volume com base no snapshot
- Associou e montou o novo volume na sua inst??ncia EC2
- Verificou se o arquivo que voc?? criou anteriormente estava no volume rec??m-criado

## Laborat??rio conclu??do

<i class="icon-flag-checkered"></i> Parab??ns! Voc?? concluiu o laborat??rio.

56. Clique em <span id="ssb_voc_grey">End Lab</span> (Encerrar laborat??rio) na parte superior desta p??gina e, em seguida, clique em <span id="ssb_blue">Yes</span> (Sim) para confirmar que voc?? deseja encerrar o laborat??rio. ??

    Um painel ser?? exibido com a mensagem: "DELETE has be initiated... You may close this message box now." (A EXCLUS??O foi iniciada... Voc?? pode fechar esta caixa de mensagem agora.).

57. Clique no **X** no canto superior direito para fechar o painel.

## Recursos adicionais

<a href="http://aws.amazon.com/ebs/" target="_blank">Recursos, fun????es e defini????es de pre??o do Amazon Elastic Block Store</a>

<a href="http://aws.amazon.com/training/" target="_blank">AWS Training and Certification</a>

Envie feedback, sugest??es ou corre????es por e-mail para: <aws-course-feedback@amazon.com>
